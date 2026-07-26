# Kiểm thử TrueTrace

Chạy từng lớp:

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

Test quan trọng nhất nằm tại
`truetrace-agent-engine/tests/test_graph_analyzer.py`: một tài khoản nhận 1 tỷ VND
và chuyển 900 triệu tới 20 tài khoản trong 60 giây phải bị nhận diện rapid mule
dispersion. `test_agents.py` kiểm tra tiếp việc đóng băng, escalated alert và STR
nháp bắt buộc human approval.
