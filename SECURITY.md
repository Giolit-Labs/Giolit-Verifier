# Security Policy

## Supported versions

| Version | Supported |
|---|---|
| 7.0.x (trial) | ✅ Supported — please update to the newest `v7.0.0-trial` build from Releases |

The verifier runs entirely on your computer and needs no network connection.
It does not transmit programs or results to Giolit Labs.

## Reporting a vulnerability

**Do not open a public issue for security vulnerabilities.**

Email **business.giolitlabs@gmail.com** with:

- `v7 --version` and `v7 --license` output,
- OS and installed library versions (`libllvm18`, `libclang-cpp18`, `libssl3`),
- the reproducer (C file + exact command) or a description of the finding,
- whether result bundles / PDFs may be shared.

We will acknowledge receipt within 3 business days and keep you informed until
a fix or mitigation is released.

## Scope notes

- The licence state in `~/.giolit/v7` (`GIOLIT_HOME`) is tamper-protected.
  Do not attempt to reset, alter or transfer it — this breaches `LICENSE.md`
  §4.4 and invalidates support.
- Result bundles record the analysed file's name, path and hash plus host
  identifiers (host name, machine ID, OS, CPU, user name). Review them before
  sharing — strip paths if needed.
- Password-protected licensed PDFs need `qpdf` (or `mupdf-tools`); keep
  `certificate-password.txt` confidential alongside the PDF.
