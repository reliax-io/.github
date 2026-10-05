# Reliax

**One call out, one certificate back.** Reliax assesses a scoring model's
predictions one decision at a time. It sits beside the model, without
retraining it or touching its weights, and attaches to each prediction a
certificate that says whether this answer can be relied on at a guaranteed
error rate, whether that guarantee covers this input, and whether the
population has moved. Routing to ALLOW, REVIEW or BLOCK reads only the
certified quantities, under a policy you write and version. BLOCK never means
decline: a person decides.

The first target is credit risk, where decisions are individually
consequential and already regulated.

## Try it

```
pip install reliax-sdk
```

```python
import reliax

reliax.configure("https://reliax.internal.example", api_key="...")
out = reliax.assess({"income": 54000, "dti": 0.31},
                    reliax.Context(score=0.04, segment="thin-file"))
out.route, out.reason_codes, out.certificate_text   # "ALLOW", ["CERTIFIED"], the stored wording
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
```

## Three kinds of output

Every field on a certificate carries one of three classes.

- **Guarantee.** A theorem that holds on exchangeable data at the stated level,
  with no assumption on the model. Routing rules read these and nothing else.
- **Exact.** Recomputed bit for bit from the record, so an auditor can replay it.
- **Signal.** A measurement without such a proof. Signals order the review
  queue and inform recalibration; no routing rule reads them.

The certificate carries three guarantees and one exact output:

| What it answers | How | Class |
|---|---|---|
| Can this answer be relied on? | A prediction set that contains the true outcome at least 1−α of the time, marginally and per declared segment, from a frozen calibration cohort named with its size, date and hash. A calibrated probability bracket accompanies it at every score level. | guarantee |
| Does the guarantee cover this input? | A credibility p-value of the input against the cohort | guarantee |
| Has the population moved? | An anytime-valid drift test on the stream, with false alarms bounded over the whole run, plus a dated outcome recheck | guarantee |
| What was decided, and why? | The route, the rule row that matched, the checks read on the way (the route trace), and the reason codes | exact |

Coverage is a property of the procedure over exchangeable data, not a
probability about any one prediction. Distribution-free per-instance
conditional coverage is not attainable, and we do not claim it. Nothing here
is a probability that a given decision is right, and the certificate never
prints a percentage next to a decision.

## Repositories

| Repository | What it is | Licence | Status |
|---|---|---|---|
| [`reliax-core`](https://github.com/reliax-io/reliax-core) | The method: conformal sets, the calibrated bracket, the drift test, the credibility p-value, the routing rule, and the advisory signals. Pure functions, no I/O. | Apache-2.0 | 0.2.1 |
| [`reliax-certificate`](https://github.com/reliax-io/reliax-certificate) | The certificate format: schema, hash chain, wording template, and the verifier. | Apache-2.0 | 0.1.0 |
| [`reliax-python`](https://github.com/reliax-io/reliax-python) | The client, published as `reliax-sdk`. | Apache-2.0 | 0.1.0 |
| [`reliax-evaluation`](https://github.com/reliax-io/reliax-evaluation) | Every number in the whitepaper, with the datasets, runners and pre-registration. | Apache-2.0 | Published |
| Reliax platform | Calibration builder, reliability engine, review queues, audit service, readable view, dashboard, domain packs. Runs inside your infrastructure. | Source-available | Not published here |

Anything that routes a decision or is written on the certificate is open and
stays open. The platform holds the state and the workflow.

## Results

Measured on public credit data. Every figure is produced by a committed runner
in [`reliax-evaluation`](https://github.com/reliax-io/reliax-evaluation), and
the negative results are reported next to the positive ones.

| | Measured |
|---|---|
| Coverage at a 0.95 target | 0.9515 ± 0.0040 (Taiwan, 30,000 applicants); 0.9532 ± 0.0167 (German) |
| Drift false alarms | 0 of 5 i.i.d. streams; a real subpopulation shift caught in 4 of 5 |
| Latency, full envelope | 8.0 ms median, 17.5 ms p95 |
| **Negative:** bad-approval capture under severe shift | The fused reliability score (0.023) and model confidence (0.078) both lose to random referral (0.096) at a 10% referral rate. The pre-registered target of twice the capture of model confidence was withdrawn for this reason. |

Full tables: the [whitepaper](https://reliax.io/whitepaper.html) and the
evaluation repository.

## Licensing

The method, the certificate format, the verifier and the client are
Apache-2.0 with its patent grant. An auditor replaying a certificate on their
own book is ordinary permitted use, with no licence to interpret.

The platform is what Reliax sells: the operated system around the open parts,
holding the cohorts, the queues, the recalibration workflow and the signed
trail. It is source-available, runs inside the customer's infrastructure, and
is not published here.
<!-- Keep one of the two sentences below and delete the other. The published
     text should name one licence and one term, not two candidates or a range. -->
It is released under the Functional Source License and converts to Apache-2.0
two years after each release.
<!-- or: -->
Its source-available terms will be published with the platform.

## Contributing

Read [`CONTRIBUTING.md`](https://github.com/reliax-io/.github/blob/main/CONTRIBUTING.md)
and open an issue before writing anything substantial. A CLA is required
before a first merge, because the platform is licensed separately.

Security and certificate-integrity issues go through
[`SECURITY.md`](https://github.com/reliax-io/.github/blob/main/SECURITY.md),
never a public issue.

## Elsewhere

Whitepaper and formatted results: [reliax.io](https://reliax.io)
