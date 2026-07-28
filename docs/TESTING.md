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

This verifier also runs in `.github/workflows/full-stack-ci.yml`. It validates both KYC decision branches and the full AML chain: Kafka event, fan-out detection within 60 seconds, temporary freeze, alert reaching 1 billion VND, and a linked STR in `DRAFT` status awaiting human approval.

The most important test is located at `truetrace-agent-engine/tests/test_graph_analyzer.py`: an account receives 1 billion VND and transfers 900 million to 20 accounts within 60 seconds, which must be identified as rapid mule dispersion. `test_agents.py` further tests the freeze, escalated alert, and draft STR requiring human approval.
