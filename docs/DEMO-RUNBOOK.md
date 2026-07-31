# TrueTrace recording runbook

This runbook prepares a repeatable five-minute demo without relying on static
KYC, AML, or STR rows. Spring/PostgreSQL is the compliance source of truth;
Kafka carries the events; the Go dashboard proxies those live records.

## 1. Boot and verify the stack

From `truetrace-deployment`:

```powershell
Copy-Item .env.example .env
docker compose up -d --build --wait
docker compose ps -a
```

`kafka-init` should exit with code 0. The other ten services should be healthy
or running. The demo configuration gives newly registered accounts VND 1
billion and enforces KYC approval before transfers; it does not update balances
with test SQL.

From the superproject root:

```powershell
python truetrace-deployment/scripts/full_stack_smoke.py
```

Do not record until this prints `TrueTrace full-stack smoke test passed`. Save
the final `Recording-ready structuring` line: it contains a run-specific
username, password, source account, and two untouched target account numbers.
The verifier has already proven the same two-transfer structuring rule with a
separate account, plus both KYC branches, all Kafka topic offsets, PostgreSQL
persistence, the rapid-dispersion path, freezes, linked draft STRs, review, and
submission.

## 2. Pre-login before recording

Customer portal:

1. Sign in with the run-specific `manual_structurer_...` username printed by the
   verifier and password `Test@12345`.
2. Open the Transfer page. This account already has an approved KYC session.

Compliance dashboard:

1. Open `http://localhost/soc/`.
2. Enter operator UID `10001` and click **Request Login Token**.
3. Read the newest token from the local dashboard container logs:

   ```powershell
   docker compose logs --since=2m dashboard-backend |
     Select-String "SECURITY AUTH OTP" |
     Select-Object -Last 1
   ```

4. Paste the 64-character token and sign in. The OTP lasts five minutes and is
   consumed at login, so finish this immediately before recording.

Prepare three clean tabs:

- `http://localhost` — customer portal;
- `http://localhost/soc/` — compliance dashboard;
- `http://localhost:9000` — Kafka UI.

Both `/soc` and `/soc/` are valid; `/soc` redirects to the canonical slash URL.

## 3. Honest live transfer sequence

For a visible Agent 2 interaction, send exactly two transfers from the printed
structuring source account:

1. VND `190000000` to the first printed target;
2. VND `190000000` to the second printed target.

These are real persisted transactions and each is near the configured VND 200
million threshold. The first event is an informational structuring finding.
The second completes a two-transaction, VND 380 million sliding-window pattern,
raises risk to at least 7/10, freezes the source account, creates a
`STRUCTURING` alert, and generates a linked draft STR. Kafka UI will show new
records in `truetrace.transactions`, `truetrace.findings.money_trail`,
`truetrace.alerts`, and `truetrace.reports.str`.

The verifier also leaves a separate rapid-dispersion case for the graph view:
VND 1 billion flows into a KYC-approved mule and VND 800 million is sent to 20
beneficiaries within 60 seconds.

## 4. Dashboard sequence

1. **KYC Center:** show the run-specific APPROVED and REJECTED sessions and
   their scores.
2. **AML Alerts:** open the new repeated-structuring alert; confirm risk at
   least 7/10, VND 380 million, exactly two transactions/two recipients, and
   the frozen source account. Then optionally show the 10/10 rapid-dispersion
   graph.
3. **Overview:** show counts derived from Spring/PostgreSQL, not dashboard seed
   counters.
4. **STR Reports:** open the DRAFT report linked to the live structuring alert.
   It cannot be submitted until a named officer moves it to READY_FOR_REVIEW.
   The automated verifier separately exercises both the rejected early submit
   and the successful reviewed submit, while leaving this new live report in
   DRAFT for the recording.

## 5. Fast diagnostics

```powershell
docker compose ps -a
docker compose logs --tail=150 backend agent-engine dashboard-backend
docker exec truetrace-kafka /opt/kafka/bin/kafka-topics.sh `
  --bootstrap-server localhost:29092 --list
```

If a screen is empty, first check the corresponding Spring API through
`http://localhost/api-bank/api/...`; the dashboard is deliberately a proxy and
does not fall back to invented business data.
