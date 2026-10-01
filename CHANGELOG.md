# Changelog

All notable changes to the Giolit Program Verifier distribution in this repository.

## [7.0.0] — Trial edition (2026)

Initial public repository release (`giolit-verifier-7.0.0-trial`).

- Verifier command `v7` (Linux x86-64, Ubuntu 24.04, LLVM/Clang 18) with checks:
  `assertion`, `overflow`, `div-by-zero`, `shift`, `array-bounds`,
  `valid-deref`, `valid-free`, `valid-memtrack`, `valid-memcleanup`;
  groups `all`, `arithmetic`, `memsafety`, `rte`.
- Report bundles: PDF certificate / counterexample report, JSON, SARIF 2.1.0,
  full proof (`proof.json`), statement (`certificate.json`), `reproduce.sh`.
- Public C++ SDK (`code/include/giolit`, `code/include/vtask`) + 5 example
  clients in `code/sdk-example`: `verify_file`, `unit_check`, `certify`,
  `license_info`, `ci_gate`.
- 12 tutorial examples (`code/share/giolit/examples`), Tutorial, Product
  Overview, README and Licence Terms in `documents/`.
- 177 benchmark verifications with known answers (102 SAFE, 75 UNSAFE,
  0 wrong) in `benchmarks/` + full evidence in `results/` (`SUMMARY.csv`,
  `index.html`).
- Trial licence: 1000 runs within 50 days; build validity 100 days.
  Contact `business.giolitlabs@gmail.com` / `www.giolit.com` for paid licences.

[7.0.0]: https://github.com/Giolit-Labs/Giolit-Verifier/releases/tag/v7.0.0
