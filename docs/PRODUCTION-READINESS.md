# TrueTrace production readiness

## Verified application gates

- Business APIs require JWT or the internal service token; unauthenticated AML access returns `401`.
- Agent-only mutations (freeze account, create AML alert, create STR, update KYC result) require `X-TrueTrace-Internal-Token`.
- Demo database seeding is disabled by the Spring `production` profile.
- H2 console is disabled and Hibernate runs with `ddl-auto=validate` in production.
- CORS origins, database credentials, JWT signing secret, and service token must be supplied at runtime.
- Agent Engine refuses to start in `TRUETRACE_ENV=production` when Vision, Qwen, identity registry, database, Kafka, Redis, or the service token still uses demo/default settings.
- Generated STRs are always `DRAFT`. Submission is rejected until a named reviewer marks the report `READY_FOR_REVIEW`; reviewer and submitter identities are persisted.

## Required external configuration

Never commit the values below. Store them in a managed secret service and inject them at runtime:

- `JWT_SECRET` (at least 256 bits of random entropy)
- `TRUETRACE_SECURITY_SYNC_TOKEN` (at least 32 random characters)
- PostgreSQL username/password and JDBC URL
- Alibaba Model Studio/DashScope keys for Vision and Qwen
- National identity-registry endpoint and API key
- Exact HTTPS origins in `TRUETRACE_ALLOWED_ORIGINS`

Production Agent Engine must use:

- `TRUETRACE_ENV=production`
- `VISION_API_PROVIDER=alibaba-model-studio` or `alibaba-ekyc`
- `LLM_PROVIDER=dashscope`
- non-local Kafka, Redis, PostgreSQL, and backend endpoints

Production backend must use:

- `SPRING_PROFILES_ACTIVE=production`
- an already migrated PostgreSQL schema (startup validates it and will not mutate it)
- `TRUETRACE_DEMO_SEED_ENABLED=false`

## Infrastructure release gates

The local Docker Compose stack is a deterministic demo environment, not a production topology. Before go-live, the bank must provide and validate:

1. Managed PostgreSQL with backups, point-in-time recovery, encryption, and an approved migration pipeline.
2. Kafka with TLS/SASL, durable replication, retention, and dead-letter/replay procedures.
3. Redis with TLS, authentication, persistence/HA appropriate to the selected state model.
4. HTTPS ingress/WAF, private service networking, rate limits, and certificate rotation.
5. Central secret management and workload identity; no literal Kubernetes or Helm secrets in Git.
6. Immutable image tags/digests, registry scanning, SBOM, signing, and promotion between environments.
7. Audit-log export to the bank SIEM with retention and access controls.
8. Load, failover, restore, penetration, privacy, and model-risk testing signed off by the bank.
9. Legal approval of the exact STR form and submission integration used by the target regulator.

## Verification commands

From the workspace root:

```powershell
docker compose -f truetrace-deployment\docker-compose.yml up -d --build --wait
python truetrace-deployment\scripts\full_stack_smoke.py
```

Component tests:

```powershell
cd truetrace-backend; mvn test
cd ..\truetrace-agent-engine; python -m pytest -q
cd ..\truetrace-dashboard\backend; go test ./...
```
