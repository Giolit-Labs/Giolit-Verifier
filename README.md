<p align="center">
  <img src="docs/logo.png" alt="Giolit Labs logo" width="220">
</p>

# Giolit Program Verifier 7.0 — Trial Edition

[![CI](https://github.com/Giolit-Labs/Giolit-Verifier/actions/workflows/ci.yml/badge.svg)](https://github.com/Giolit-Labs/Giolit-Verifier/actions/workflows/ci.yml)
[![Platform](https://img.shields.io/badge/platform-Ubuntu%2024.04%20x86__64-blue)](https://github.com/Giolit-Labs/Giolit-Verifier/releases)
[![Version](https://img.shields.io/badge/v7-7.0.0-green)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-Trial_1000_runs_50_days-orange)](LICENSE.md)

**Mathematical proof that your C code is free of run-time errors — or the exact input that makes it fail.**

| Result | Meaning |
|---|---|
| **PROVEN** | No execution, for any input, violates the check. The proof is included. |
| **VIOLATED** | A real execution violates it: the failing input and every step are included. |
| **UNREACHABLE** | The operation never runs (dead code, or not called from the entry function). |
| **UNKNOWN** | Not decided within the time budget (see the Tutorial). |

Checks: assertions, signed integer overflow, division by zero, invalid shifts, array bounds, pointer dereferences, `free()` and memory leaks — in sequential, recursive and multithreaded (POSIX threads) programs.

> Trial edition: 1000 runs within 50 days on one computer. Trial documents are watermarked. A purchased licence produces professional, password-protected documents with your licence number. See [Licensing](#licensing).

---

## Download

This repository hosts the trial bundle — both in the tree and as a Release asset:

- **`giolit-verifier-7.0.0-trial.tar.gz`** (22 MB) — the complete package: verifier, SDK, documents, benchmarks and result evidence.
- **`CHECKSUMS.txt`** — SHA-256 of the bundle. Verified:

```text
e9d52eeaf1a2747a41c6d0b70fd7dc175f588909af7b894aa6cf0e8de41c8480  giolit-verifier-7.0.0-trial.tar.gz
```

Verify after cloning:

```sh
shasum -a 256 -c CHECKSUMS.txt   # macOS
sha256sum -c CHECKSUMS.txt       # Linux
```

The same two files are attached to the [`v7.0.0-trial` Release](https://github.com/Giolit-Labs/Giolit-Verifier/releases/tag/v7.0.0-trial) for download without cloning.

## Documents (preview without downloading the zip)

`docs/` mirrors `documents/` inside the bundle, so GitHub renders previews directly:

- [Tutorial.pdf](docs/Tutorial.pdf) — start here (installation, first steps, all examples, SDK)
- [Product-Overview.pdf](docs/Product-Overview.pdf)
- [README.md](docs/README.md) / [README.pdf](docs/README.pdf)
- [License-Terms.md](docs/License-Terms.md) / [License-Terms.pdf](docs/License-Terms.pdf) (authoritative terms are also at [`LICENSE.md`](LICENSE.md))

## Quick start (Ubuntu 24.04 x86-64)

```sh
sudo apt update
sudo apt install libllvm18 libclang-cpp18 libclang-common-18-dev libc6-dev libssl3
# PDFs (recommended)
sudo apt install texlive-luatex texlive-latex-recommended texlive-latex-extra \
                 texlive-pictures texlive-fonts-extra qpdf

tar xzf giolit-verifier-7.0.0-trial.tar.gz
cd giolit-verifier-7.0.0-trial/code
./install.sh   # → ~/.local/giolit-v7, command in ~/.local/bin

v7 --version   # v7 7.0.0 (trial version)
v7 --license   # runs and days left (uses no run)

cp -r ~/.local/giolit-v7/share/giolit/examples ~/giolit-examples
cd ~/giolit-examples
v7 01_loop_invariant.c                  # ALL CHECKS PROVEN (exit 0)
v7 02_loop_bug.c                        # VIOLATIONS FOUND, input n = 8 (exit 1)
v7 --report=out/01 01_loop_invariant.c  # out/01/certificate.pdf + evidence bundle
```

Without LuaLaTeX the verifier works fully (console, JSON, SARIF); only the PDF is skipped.

## What's inside the bundle

| Path (inside the `.tar.gz`) | Contents |
|---|---|
| `code/` | `bin/v7`, `lib/libgiolit_*.so`, `include/` (public C++ API), `share/giolit/` (logo + 12 tutorial examples), `sdk-example/` (5 API clients), `install.sh` / `uninstall.sh` |
| `documents/` | `Tutorial.pdf` (start here), `Product-Overview.pdf`, `README.md` / `README.pdf`, `License-Terms.md` / `License-Terms.pdf` — mirrored at [`docs/`](docs/) for in-browser preview |
| `benchmarks/` | C benchmarks with a known answer: `sequential`, `recursive`, `concurrent` (+ `giolit-thread-suite`), `memory`, `industrial`, `projects`, `tutorial-examples`. `MANIFEST.tsv` lists every verification. |
| `results/` | Result evidence for every benchmark in `MANIFEST.tsv` (177 runs: **102 SAFE, 75 UNSAFE, 0 wrong**): PDF, `verifier-output.txt`, `results.json`, `results.sarif`, `proof.json`, `certificate.json`, sources, `reproduce.sh`. `SUMMARY.csv` + `index.html` summarise them. |
| `SHA256SUMS` | Checksums of every file *inside* the bundle (`sha256sum -c SHA256SUMS` after extracting) |
| `README.txt` | Short package readme |

## Commands

```sh
v7 [options] file.c [file.c ...]   # files are compiled and linked together
```

| Option | Meaning | Default |
|---|---|---|
| `--checks=LIST` | Check kinds or a group | `all` |
| `--entry=NAME` | Function to verify from | `main` |
| `--timeout=SECONDS` | Total time budget | `60` |
| `--report=DIR` | Bundle: PDF, JSON, SARIF, proof, sources | — |
| `--issued-to=NAME` | Recipient named on the PDF (with `--report`) | — |
| `--json=FILE`, `--sarif=FILE`, `--proof=FILE` | Machine-readable results | — |
| `--verbose` | Complete counterexample traces | off |
| `-I DIR`, `-D NAME[=V]`, `-std=...` | Passed to the C compiler | — |
| `--license` | Licence status (uses no run) | — |
| `--activate=FILE` | Apply a purchased licence | — |
| `--benchmark file.c [maxIterations] [seconds]` | SV-COMP style verdict: SAFE / UNSAFE / UNKNOWN | off |

Check kinds: `assertion`, `overflow`, `div-by-zero`, `shift`, `array-bounds`, `valid-deref`, `valid-free`, `valid-memtrack`, `valid-memcleanup`. Groups: `all`, `arithmetic`, `memsafety`, `rte`.

Exit codes: `0` proven · `1` violation · `2` unknown · `3` usage/input error · `4` no valid licence.

Full guide: [`docs/Tutorial.pdf`](docs/Tutorial.pdf) (same as `documents/Tutorial.pdf` inside the bundle).

## SDK and CI

The bundle's `code/sdk-example/` has five C++ API clients (`verify_file`, `unit_check`, `certify`, `license_info`, `ci_gate`; build with `./build.sh`). Gate builds on the exit code and upload `--sarif` to GitHub code scanning. See `.github/workflows/ci.yml` for a working Ubuntu 24.04 example.

## Licensing

Trial: **1000 runs within 50 days** of the first run; the build stops accepting trial runs 100 days after it was made. `v7 --license` shows status. State lives in `~/.giolit/v7` and is tamper-protected.

Purchase (terms, volumes, services, training): **business.giolitlabs@gmail.com** · **www.giolit.com**. Full terms: [`LICENSE.md`](LICENSE.md) (also [`docs/License-Terms.pdf`](docs/License-Terms.pdf)).

## Support

Giolit Labs, Ramavarmapuram P.O., Thrissur, Kerala, India — **www.giolit.com** — **business.giolitlabs@gmail.com**. Include `v7 --version`, `v7 --license`, and the result bundle if shareable.
