# TrueTrace Testing

Run each layer:

```powershell
cd truetrace-agent-engine
python -m pytest -q

cd ..\truetrace-backend
mvn test

cd ..\truetrace-dashboard\backend
go test ./...

cd ..\frontend
npm ci
npm test -- --run
npm run build
npm run lint

cd ..\..\truetrace-web-client
npm ci
npm test
npm run build
npm run lint

cd ..\truetrace-mobile-app
flutter test
flutter analyze

cd ..\truetrace-terraform
terraform fmt -check -recursive
terraform init -backend=false
terraform validate

cd ..\truetrace-deployment
docker compose --env-file .env.example config
```

Full integration testing with Docker:

```powershell
cd ..
docker compose -f truetrace-deployment/docker-compose.yml up -d --build --wait
python truetrace-deployment/scripts/full_stack_smoke.py
```

This verifier also runs in `.github/workflows/full-stack-ci.yml`, after the
Python, Java, Go, dashboard UI, and customer web unit/build gates. It validates:

- both KYC decision branches and direct PostgreSQL persistence;
- the KYC-before-transfer policy for every source account;
- offset increases on all six Kafka topics;
- a VND 1 billion inflow and 20-target fan-out within 60 seconds;
- persisted transactions, account freeze, AML alert, and alert-linked `DRAFT` STR;
- rejection of premature STR submission, then named review and submission;
- two additional VND 190 million structuring transfers using dynamic accounts,
  producing a `STRUCTURING` alert for VND 380 million, a freeze, and a linked
  draft STR.

The first structuring transfer is informational; the second raises the
sliding-window score to at least 7/10. The separate rapid-dispersion path
remains stronger coverage of the graph and 20-beneficiary fan-out logic.

The most important test is located at `truetrace-agent-engine/tests/test_graph_analyzer.py`: an account receives 1 billion VND and transfers 900 million to 20 accounts within 60 seconds, which must be identified as rapid mule dispersion. `test_agents.py` further tests the freeze, escalated alert, and draft STR requiring human approval.

The Go dashboard tests verify transparent method/path/query/body forwarding for
KYC approve/reject, AML escalate/close/freeze, STR review/submit, live stats,
and agent status. Its Kafka tests assert schema telemetry only; the dashboard
must never create duplicate compliance rows.
