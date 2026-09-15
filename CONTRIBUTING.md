# Contributing to Reliax

Thanks for taking the time. This file applies to every repository in the
Reliax organisation unless a repository overrides it.

## Before you start

Open an issue before writing code for anything non-trivial. Reliax
repositories carry claims that are checked by validators and auditors, so a
change that alters a reported number, a threshold or a guarantee needs to be
discussed before it is written, not after.

## The two kinds of repository

Reliax repositories are licensed differently depending on what they do, and
the licence is stated in each repository's `LICENSE` file. Check it before
contributing.

- **Apache-2.0 repositories** — the certificate specification, the verifier,
  the Python SDK and the evaluation package. Anyone may use these for any
  purpose, including commercial and production use. An auditor must be able
  to verify a Reliax certificate without asking anyone's permission, so these
  will stay permissively licensed.
- **Source-available repositories** — the reliability engine. The source is
  public and free for non-production use, but production use requires a
  commercial licence. See that repository's `LICENSE` for the exact terms.

## Contributor Licence Agreement

Before your first pull request can be merged you must sign the Reliax
Contributor Licence Agreement. A bot will comment on your pull request with a
link; signing takes about a minute and covers all your future contributions
across the organisation.

The CLA exists because Reliax offers commercial licences for the engine. To do
that lawfully we need the right to license contributed code under terms other
than the one it arrived under. You keep the copyright in your contribution —
you are granting a licence, not signing it away.

## Reproducibility rules

These apply to `reliax-evaluation` and to any repository that reports measured
numbers.

1. **No hand-typed numbers.** Every figure in a README, paper or table must be
   produced by a runner script and stored in a results file. If you change a
   number, change the code that produces it and commit the regenerated
   results.
2. **Seeds and splits are fixed in code**, not passed on the command line.
3. **Negative results stay in.** Do not remove, soften or relocate a result
   because it is unflattering. Reporting failures in full is the point of
   these repositories, not an embarrassment to be managed.
4. **Do not touch frozen datasets.** Some evaluation datasets are sealed
   pending a pre-registered confirmatory test. If a file or document says a
   dataset is frozen, it is frozen. Do not download it, load it, or write code
   that would.
5. **Pre-registration is binding.** Changes to a pre-registered protocol go
   through the amendment process documented in the repository, before the
   relevant data is opened — never after seeing a result.

## Pull requests

- Branch from `main`. Keep pull requests focused on one change.
- Tests must pass. Add tests for new behaviour.
- Commits should be signed. Use `git commit -S`, or enable vigilant mode in
  your GitHub settings.
- Describe what you changed and, if a reported number moved, by how much and
  why.

## Reporting problems

For bugs and feature requests, open an issue.

For anything with a security or integrity dimension — a flaw in the hash
chain, a way to forge or replay a certificate, a dependency vulnerability —
do **not** open a public issue. Follow `SECURITY.md`.

## Questions

Use GitHub Discussions rather than email where you can, so the answer is
searchable by the next person.
