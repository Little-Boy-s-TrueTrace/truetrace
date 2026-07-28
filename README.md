# TrueTrace: Multi-Agent Deepfake & AML Autonomous Compliance System

<p align="center">
  <img src="https://img.shields.io/badge/Hackathon-Alibaba%20Cloud%20%26%20Qoder%20HCMC%20FSI-blue?style=for-the-badge" alt="Hackathon Badge"/>
  <img src="https://img.shields.io/badge/Track-Financial%20Services%20(BUILD)-green?style=for-the-badge" alt="Track Badge"/>
  <img src="https://img.shields.io/badge/AI%20Agents-3%20Autonomous-orange?style=for-the-badge" alt="Agents Badge"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Java-17-red?style=flat-square&logo=openjdk" alt="Java"/>
  <img src="https://img.shields.io/badge/Spring%20Boot-3-brightgreen?style=flat-square&logo=spring" alt="Spring Boot"/>
  <img src="https://img.shields.io/badge/Python-3.12-blue?style=flat-square&logo=python" alt="Python"/>
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react" alt="React"/>
  <img src="https://img.shields.io/badge/Next.js-16-black?style=flat-square&logo=next.js" alt="Next.js"/>
  <img src="https://img.shields.io/badge/Flutter-3-02569B?style=flat-square&logo=flutter" alt="Flutter"/>
  <img src="https://img.shields.io/badge/Kafka-3.9-231F20?style=flat-square&logo=apachekafka" alt="Kafka"/>
  <img src="https://img.shields.io/badge/PostgreSQL-16-4169E1?style=flat-square&logo=postgresql" alt="PostgreSQL"/>
  <img src="https://img.shields.io/badge/Docker-Compose-2496ED?style=flat-square&logo=docker" alt="Docker"/>
  <img src="https://img.shields.io/badge/Terraform-AWS-7B42BC?style=flat-square&logo=terraform" alt="Terraform"/>
  <img src="https://img.shields.io/badge/AI-Alibaba%20Qwen-FF6A00?style=flat-square" alt="Qwen"/>
</p>

> **Built with [Qoder](https://qoder.com)** -- AI-Powered Spec-Driven Development
> [Development Specs & Prompts](SPEC.md) | [Architecture (Vietnamese)](docs/ARCHITECTURE.vi.md) | [Test Matrix](docs/TESTING.md) | [Production Readiness](docs/PRODUCTION-READINESS.md)

---

## What is TrueTrace?

TrueTrace is a next-generation **autonomous compliance platform** designed for banks and financial institutions. It automates three critical compliance workflows using a collaborative Multi-Agent AI system:

| Agent | Problem Solved | How It Works |
|-------|---------------|-------------|
| **Deepfake Inspector** | Fraudulent identity during eKYC onboarding | AI Vision analyzes selfies & ID documents, detects deepfakes with Alibaba Qwen-VL, auto-approves or rejects in **< 10 seconds** |
| **Money-Trail Explorer** | Money laundering via mule accounts, structuring, circular flows | Builds real-time transaction graph, detects **6 AML patterns** simultaneously, **auto-freezes** accounts when risk > threshold |
| **AML Report Generator** | Manual STR report writing (2-4 hours per report) | LLM generates bilingual (English/Vietnamese) Suspicious Transaction Reports in **< 1 minute**, ready for human review |

> **Production guardrail**: TrueTrace provides decision support. AI findings are not
> legal conclusions, and STR submission always requires an authorized human reviewer.

---

## System Architecture

TrueTrace utilizes an event-driven architecture powered by Apache Kafka, Redis, and PostgreSQL, binding modern microservices into a unified compliance ecosystem:

```mermaid
graph TB
    subgraph "Client Layer"
        WC["truetrace-web-client<br/>(Next.js Portal)"]
        DA["truetrace-dashboard<br/>(React Admin Console)"]
        MA["truetrace-mobile-app<br/>(Flutter Bank App)"]
    end

    subgraph "Backend API Layer"
        BE["truetrace-backend<br/>(Spring Boot API)"]
        DB["truetrace-dashboard-backend<br/>(Go API Proxy)"]
    end

    subgraph "Event & Cache Bus"
        K["Apache Kafka<br/>(Event Streaming)"]
        R["Redis Cache<br/>(Agent State & Lock)"]
    end

    subgraph "Multi-Agent AI Engine"
        AE["truetrace-agent-engine<br/>(Python/AsyncIO)"]
        A1["Agent 1: Deepfake Inspector"]
        A2["Agent 2: Money-Trail Explorer"]
        A3["Agent 3: AML Reporter"]
    end

    subgraph "Data Store"
        PG["PostgreSQL Database"]
    end

    WC -->|"KYC Submissions"| BE
    MA -->|"Transactions"| BE
    DA -->|"Compliance Control"| DB
    DB -->|"Proxy Requests"| BE
    BE -->|"Publish Events"| K
    K -->|"Consume Telemetry"| AE
    AE --> A1
    AE --> A2
    AE --> A3
    A1 -->|"Identity Status Update"| BE
    A2 -->|"Mule Account Freeze"| BE
    A3 -->|"Submit STR Reports"| BE
    BE --> PG
    BE --> R
    DB --> PG
```

---

## Project Directory Structure

The TrueTrace project workspace contains the following components:

| Directory | Language / Tech Stack | Description |
|---|---|---|
| **truetrace-backend** | Java 17 / Spring Boot / Maven | Core banking ledger, accounts management, transaction posting, and compliance API endpoints. |
| **truetrace-agent-engine** | Python 3 / Kafka / Redis | Multi-Agent orchestrator that coordinates Python agents listening to Kafka compliance events. |
| **agent-deepfake-inspector** | Python / Vision LLM Prompt | Prompt configuration and validation schemas for the Deepfake KYC validation agent. |
| **agent-money-trail** | Python / Graph Prompt | Prompt configuration and heuristics for tracking structuring, velocity, and mule-account flows. |
| **agent-aml-reporter** | Python / LLM Text Generator | Prompt configuration for writing official, regulatory-compliant Suspicious Transaction Reports (STR). |
| **truetrace-dashboard** | React 19 / TypeScript / Go | Compliance admin console for bank officers to audit KYC sessions, AML alerts, and STRs. |
| **truetrace-web-client** | Next.js Standalone / Node 20 | Customer portal enabling users to log in, transfer money, and submit KYC videos. |
| **truetrace-mobile-app** | Flutter 3 / Dart | Customer mobile banking application showing transactions, transfer forms, and balances. |
| **truetrace-deployment** | Docker Compose / Helm / K8s | Container orchestration scripts and gateway configurations for local and cluster deployments. |
| **truetrace-terraform** | HCL / Terraform | Cloud infrastructure deployment definitions targeting AWS (ECS, RDS, MSK, ElastiCache, WAF). |

---

## Quick Start

### 1. Run the entire platform locally via Docker Compose

Prerequisite: Ensure **Docker Desktop** is running. Then navigate to the deployment folder:

```bash
cd truetrace-deployment
cp .env.example .env
docker-compose up --build -d
```

Once started, the services can be accessed at:
- **Customer Portal**: <http://localhost:3000>
- **Compliance Dashboard**: <http://localhost:80/soc/> (User: `admin`, Pass: `admin123`)
- **Core Bank Backend API**: <http://localhost:8080>
- **Kafka Message Visualizer (Kafdrop)**: <http://localhost:9000>

### 2. Verify all services are healthy

```bash
docker ps
```
You should see all 10 containers in the `truetrace-` network running and marked as healthy.

---

## Security & Compliance Safeguards

1. **Internal Token Sync**: All internal microservice-to-microservice REST communications are authenticated using `TRUETRACE_SECURITY_SYNC_TOKEN`.
2. **Log Sanitization**: Core backend service logs are configured to automatically strip credit card numbers, password terms, and JWT secrets to comply with PCI-DSS regulations.
3. **Mule Account Containment**: The Money-Trail Agent automatically blocks target accounts and pauses pending transfers if anomaly thresholds (>7.0 score) are triggered.

---

## Demo Scenario: End-to-End Compliance Pipeline

The following walkthrough demonstrates all 3 agents working in sequence:

### Act 1: Deepfake Detection (Agent 1)
```
Customer Portal --> Submit KYC (selfie + CCCD photos)
    | Kafka: truetrace.kyc.submissions
Agent 1 --> AI Vision analysis --> Auto APPROVE / REJECT / MANUAL_REVIEW
    |
Dashboard --> KYC Center shows result with deepfake & face-match scores
```

### Act 2: Money Laundering Detection (Agent 2)
```
Customer Portal --> Transfer money 3-4 times rapidly (within 60s)
    | Kafka: truetrace.transactions
Agent 2 --> Transaction graph analysis --> Detect velocity anomaly / mule split
    | Risk score >= 7.0
Backend --> AUTO-FREEZE source account + Create AML Alert
    |
Dashboard --> AML Alerts tab shows new alert with risk breakdown
```

### Act 3: Automated STR Report (Agent 3)
```
Agent 2 --> Publishes high-risk alert
    | Kafka: truetrace.alerts
Agent 3 --> LLM generates bilingual narrative (EN/VI)
    |
Backend --> Creates DRAFT STR Report
    |
Dashboard --> STR Reports tab shows report ready for human review
```

> **Key Insight**: All 3 agents work as an autonomous pipeline -- from fraud detection to account containment to regulatory reporting -- in **seconds** instead of **days**.

### AML Detection Patterns

| Pattern | Risk Points | Description |
|---------|:-----------:|-------------|
| Fan-out | +4 | One sender to many recipients |
| Fan-in | +4 | Many senders to one recipient |
| Circular Flow | +6 | Cycle detection up to 5 hops (A-B-C-A) |
| Velocity Anomaly | +3 | High transaction volume within 1 hour |
| Structuring | +5 | Amounts just beneath reporting limits |
| Rapid Mule Dispersion | +8 | Immediate forwarding of received funds |

---

## Hackathon

**Alibaba Cloud & Qoder Hackathon HCMC -- FSI (Financial Services Industry)**

- **Track**: BUILD (Developer Track)
- **Team**: Little Boy's
- **Built with**: [Qoder](https://qoder.com) Spec-Driven AI Development
- **AI Provider**: Alibaba Cloud Model Studio (Qwen-VL, Qwen-Plus)

See [SPEC.md](SPEC.md) for the complete Qoder development workflow and prompts used to build TrueTrace.

---

## License

This project is developed for the Alibaba Cloud & Qoder Hackathon HCMC 2026.
