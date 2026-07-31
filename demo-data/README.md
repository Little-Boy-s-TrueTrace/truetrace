# TrueTrace demo fixtures

These files are fictional, reproducible test fixtures. They are not real
identity documents or real customer data.

- Use `kyc-profile.json` for the exact name, CCCD number, and file names to
  enter during the recorded KYC pass scenario.
- Use `kyc-images/synthetic_deepfake_test.png` only for the explicit negative
  KYC test. The visible banner and file name identify it as a synthetic
  fixture, so deterministic demo mode can reject it without pretending that a
  real forensic model was called.
- Application records such as users, accounts, sessions, transactions, alerts,
  and STRs are still created dynamically and persisted in PostgreSQL. These
  files provide only repeatable binary inputs for the demo.
