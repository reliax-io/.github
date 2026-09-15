# Reliax

**The reliability layer for AI.** A per-decision trust envelope for regulated
AI systems: a formal coverage guarantee, a calibrated risk bracket, a drift
tripwire and a signed audit record on every prediction, carried through
multi-model chains and routed to allow, review or block under a policy you own.
No model modification.

## Repositories

| Repository | What it is | Licence |
|---|---|---|
| `certificate-spec` | The certificate format: JSON schema, hash-chain construction, versioning. | Apache-2.0 |
| `reliax-verify` | Independent verifier. Checks a certificate chain without contacting us. | Apache-2.0 |
| `reliax-python` | Client SDK. `pip install reliax` | Apache-2.0 |
| `reliax-evaluation` | Reproducibility package for the whitepaper. Every reported number regenerates from a runner script. | Apache-2.0 |
| `reliax-core` | The reliability engine. | BUSL-1.1, free for non-production use |

## Why the split

An auditor has to be able to verify our claims without our permission.
The specification, the verifier and the SDK are therefore permissively
licensed, permanently, and nothing about that is a trial or a teaser. The
engine is source-available: read it, run it, evaluate it, replay your own
history in shadow mode — a licence is needed only once its output starts
affecting live decisions.

We do not call this open source. Parts of it are; the engine is not. A company
whose product is the difference between a proven guarantee and an unproven
signal should be exact about that too.

## What holds and what does not

Three components are theorems and the routing runs on them: conformal
coverage, Venn-Abers calibration, and an anytime-valid drift martingale. Two
are signals and are labelled as signals: the triage rank and the calibration
opinion. Nothing sits in between, and the signals never enter a routing rule.

Results under distribution shift are reported in full, including the ones that
do not flatter the method. Under a severe shift our fusion score performs worse
than random referral, and that is in the README of the evaluation repository
rather than in a footnote. The triage ranking ships only if a pre-registered
benchmark, frozen before the confirmatory datasets are opened, says it should.

## Contributing

Read `CONTRIBUTING.md`. Open an issue before writing anything substantial. A
CLA is required before your first merge, because the engine is dual-licensed.

Security and certificate-integrity issues go through `SECURITY.md`, never a
public issue.

## Elsewhere

Whitepaper and formatted results: [reliax.io](https://reliax.io)
