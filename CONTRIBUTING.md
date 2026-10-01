# Contributing to Giolit-Verifier

Thanks for your interest in the Giolit Program Verifier.

## What this repository is

This repository hosts Giolit Labs' **trial bundle**
(`giolit-verifier-7.0.0-trial.tar.gz`, verified by `CHECKSUMS.txt`).
Inside the bundle: the `v7` verifier binaries and shared libraries
(`code/bin`, `code/lib`), the public C++ API headers (`code/include`),
tutorial examples, the SDK examples, documents, benchmarks and their result
evidence. The verification engine itself is distributed as binaries; only the
documented entry points in `code/include` are public.

## What we accept

- Bug reports with a minimal C reproducer, the exact `v7` command, `v7 --version`
  / `v7 --license` output, and the result bundle (`--report=DIR`) if shareable.
- Documentation fixes and tutorial feedback.
- New or improved benchmarks (C programs with a known SAFE/UNSAFE answer).
- SDK example improvements (C++17, must build with `code/sdk-example/build.sh` inside the extracted bundle).

We do **not** accept reverse-engineering of the binaries, licence-circumvention,
or removal of trial markings (see `LICENSE.md`, sections 2, 4.4 and 5).

## Reporting issues

1. Check `documents/Tutorial.pdf` inside the bundle (troubleshooting + “When the result is UNKNOWN”)
   and the table in `README.md`.
2. Verify integrity: `sha256sum -c CHECKSUMS.txt`, then after extracting, `sha256sum -c SHA256SUMS` inside the bundle folder.
3. Open an issue with the template: OS (`Ubuntu 24.04`?), exact command,
   expected vs actual result, reproducer files, `verifier-output.txt`.

## Benchmarks

- Add the `.c` file(s) under the right `benchmarks/<category>/` folder.
- Keep the SV-COMP style (`__VERIFIER_nondet_*`, `__VERIFIER_assume`,
  `__VERIFIER_assert`) where applicable.
- State the expected verdict (SAFE/UNSAFE) and the `v7` options in your PR
  description; maintainers will regenerate the `results/` evidence.

## SDK examples

- Follow the four-step pattern (`acquireToken` → `lowerProgram` →
  `verifyProgram` → read `Verification::result`).
- Every library entry point needs a valid `AccessToken`; do not cache tokens.
- Test on Ubuntu 24.04 with `libllvm18 libclang-cpp18 libclang-common-18-dev
  libc6-dev libssl3` installed.

## Pull requests

- Keep PRs focused; one benchmark family or one example per PR.
- Do not commit report output directories (`out/`,
  `--report` targets), PDFs you generated locally, or `~/.giolit` state.
  The `*.tar.gz` bundle itself is versioned — only maintainers update it.
- CI (`ci.yml`) must pass: install check, `01_loop_invariant.c` → exit 0,
  `02_loop_bug.c` → exit 1, SDK builds.

## Contact

Security issues: see `SECURITY.md`. Commercial questions (licences, services,
certification, training): `business.giolitlabs@gmail.com`.
