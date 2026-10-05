# Reliax

**One call out, one certificate back.** Reliax assesses a scoring model's
predictions one decision at a time. It sits beside the model, without
retraining it or touching its weights, and attaches to each prediction a
certificate that says whether this answer can be relied on at a guaranteed
error rate, whether that guarantee covers this input, and whether the
population has moved. Each decision is then routed to ALLOW, REVIEW or BLOCK.
The routing rule looks only at what the certificate guarantees, and the
thresholds it applies come from a policy that you write and keep under
version control.

```mermaid
flowchart LR
    M["Your model<br/>score, reason codes"] --> S
    subgraph open ["Open, Apache-2.0"]
        S["reliax-sdk<br/>one call out, one certificate back"]
        C["reliax-core<br/>guarantees and the routing rule"]
        F["reliax-certificate<br/>record format, hash chain, verifier"]
    end
    subgraph platform ["Reliax platform · source-available · in your infrastructure"]
        P["calibration sets, drift state,<br/>review queues, audit trail"]
    end
    S --> P
    P --> C
    C --> F
    F --> R[("hash-chained<br/>record")]
    R --> A["Auditor<br/>verifies without Reliax"]
```

- **ALLOW.** The decision can be acted on automatically
- **REVIEW.** The certificate holds but the case fails one of the
thresholds in your own policy, such as an ambiguous prediction set or a
bracket above your approve ceiling, so a person decides with the model's
answer and the certificate in front of them. 
- **BLOCK.** The assessment itself is no longer reliable: the population has drifted or the
input lies outside what was calibrated, so neither the model nor Reliax's
certificate is competent for this  case, and a person decides without relying
on either.

The first target is credit risk, where decisions are individually
consequential and already regulated.

## Try it locally

The method is a plain Python package and runs on its own. This example trains a
model on synthetic data, calibrates it, and routes one decision:

```
pip install reliax-core
```

```python
from reliax_core import ConformalCalibrator, KNNOODDetector, CredibilityReference, Policy, Envelope, evaluate

sets = ConformalCalibrator(model.predict_proba(X_cal), y_cal)  # the coverage guarantee
detector = KNNOODDetector().fit(X_cal)
credibility = CredibilityReference.from_detector(detector)     # "does the guarantee cover this input?"
policy = Policy(alpha=0.05)                                    # the thresholds you set

def route(x):
    env = Envelope(credibility=credibility.p_value(detector.distance(x)),
                   prediction_set=sets.prediction_set(model.predict_proba([x])[0], alpha=policy.alpha))
    d = evaluate(policy, env)
    print(f"{d.route:6} {d.reason_codes[0]:14} {d.trace_text()}")

route(clear_case)       # one label left standing
route(borderline_case)  # both labels left standing
route(unlike_anything)  # far from every calibration row
```

```
ALLOW  CERTIFIED      Route trace: row 6 matched, so every check above it passed.
REVIEW SET_AMBIGUOUS  Route trace: row 3 matched, so every check above it passed.
BLOCK  OOD_EXTREME    Route trace: row 2 matched, so every check above it passed.
```

The complete, runnable version with the model and the data is the
[quick start in reliax-core](https://github.com/reliax-io/reliax-core#quick-start).


## With the platform

The client talks to a running Reliax platform, which holds the calibration
cohorts, the drift state and the record store. Installing the client alone
does nothing until it is pointed at one.

```
pip install reliax-sdk
```

```python
import reliax

reliax.configure("https://reliax.internal.example", api_key="...")   # your platform instance
out = reliax.assess(
    {"income": 54000, "dti": 0.31},                                     # the model input
    reliax.Context(score=0.04, segment="thin-file"),                    # score: your model's output
)
print(out.route, out.reason_codes)   # ALLOW ['CERTIFIED']
print(out.certificate_text)          # the wording stored on the record, verbatim
```

PyPI package `reliax-sdk`, import name `reliax`, source at
[`reliax-python`](https://github.com/reliax-io/reliax-python). Standard
library only.

What comes back, stored verbatim on a hash-chained record:

```
Certificate · rxe_7f3a9b · credit PD · 2026-09-10T09:14:07Z
Prediction set {repay}, produced by a procedure that on calibration cohort C
(n = 7,500, frozen 2026-06-30, sha256 3f9a…) contains the true outcome at least 95% of the time.
Credibility 0.61: this input is consistent with cohort C. Exchangeability: no alarm.
Routing: ALLOW under policy credit-pd@4 · row 6 matched, so every check above it passed.
Read this correctly: this is not a probability that this applicant turns out well.
```

Anyone can check a record without Reliax, and the verifier works if Reliax no
longer exists:

```
pip install reliax-certificate
reliax verify record.json
