# Security Policy

This policy applies to every repository in the Reliax organisation.

## Reporting a vulnerability

**Do not open a public issue.**

Use GitHub's private vulnerability reporting on the affected repository
(Security tab → Report a vulnerability), or email **security@reliax.io**
[confirm address].

Please include:

- the repository and version or commit,
- what the issue is and what an attacker could achieve,
- steps to reproduce, including any proof-of-concept,
- your assessment of severity.

## What is in scope

Ordinary software vulnerabilities, and also anything that undermines the
integrity of a reliability certificate. Specifically, we want to hear about:

- a way to forge, alter or replay a certificate without detection,
- a way to break or rebuild the hash chain so that an edit does not show,
- a way to make the coverage, calibration or drift components report a
  guarantee they do not hold,
- a way to suppress or spoof a drift alarm,
- reconstruction of individual records from a published input fingerprint,
- vulnerabilities in our dependencies as we ship them.

Integrity flaws in the certificate are the highest-severity class of issue in
these repositories. The product exists so that a decision can be audited, so a
defect that lets a certificate lie is more serious than a crash.

## Response

- Acknowledgement within 3 working days.
- An assessment and a planned fix date within 10 working days.
- We will keep you updated through to the fix.

We ask for 90 days before public disclosure, and we will usually be faster. If
a fix changes a published measured result, the correction is published
alongside it.

## Credit

We will credit reporters in the release notes by name or handle, unless you
prefer otherwise. We do not currently run a paid bounty programme.

## Out of scope

- Findings from automated scanners without a demonstrated impact.
- Attacks requiring physical access to a user's machine.
- Social engineering of Reliax staff.
- Reports against `reliax.io` marketing pages rather than code.

## Safe harbour

We will not pursue legal action against researchers who act in good faith
under this policy: who avoid privacy violations and service disruption, who
only interact with accounts they own, and who give us reasonable time to fix
an issue before disclosing it.
