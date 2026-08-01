# TrueTrace: Multi-Agent Deepfake & AML Autonomous Compliance System

<p align="center">
  <img src="https://img.shields.io/badge/Hackathon-Alibaba%20Cloud%20%26%20Qoder%20HCMC%20FSI-blue?style=for-the-badge" alt="Hackathon Badge"/>
  <img src="https://img.shields.io/badge/Track-BUILD%20(Developer)-green?style=for-the-badge" alt="Track Badge"/>
  <img src="https://img.shields.io/badge/AI%20Agents-3%20Autonomous-orange?style=for-the-badge" alt="Agents Badge"/>
  <img src="https://img.shields.io/badge/Team-Little%20Boy's-purple?style=for-the-badge" alt="Team Badge"/>
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
  <img src="https://img.shields.io/badge/Terraform-Alibaba%20Cloud-7B42BC?style=flat-square&logo=terraform" alt="Terraform"/>
  <img src="https://img.shields.io/badge/AI-Alibaba%20Qwen--VL%20%26%20Qwen--Plus-FF6A00?style=flat-square" alt="Qwen"/>
</p>

<p align="center">
  <strong>Built with <a href="https://qoder.com">Qoder</a></strong> &mdash; AI-Powered Spec-Driven Development<br/>
  <a href="SPEC.md">Development Specs & Prompts</a> &bull;
  <a href="docs/ARCHITECTURE.md">Architecture</a> &bull;
  <a href="docs/PLAYBOOK.md">AML Investigation Playbook</a> &bull;
  <a href="docs/TESTING.md">Test Matrix</a> &bull;
  <a href="docs/PRODUCTION-READINESS.md">Production Readiness</a>
</p>

---

## Table of Contents

- [Problem Statement](#problem-statement)
- [What is TrueTrace?](#what-is-truetrace)
- [System Architecture (Alibaba Cloud)](#system-architecture)
- [System Design & Data Flow](#system-design--data-flow)
- [Demo Scenario: End-to-End Pipeline](#demo-scenario-end-to-end-compliance-pipeline)
- [AML Detection Patterns](#aml-detection-patterns)
- [Powered by Alibaba Cloud AI & Infrastructure](#powered-by-alibaba-cloud-ai--infrastructure)
- [Built with Qoder: Spec-Driven Development](#built-with-qoder-spec-driven-development)
- [Project Structure](#project-directory-structure)
- [Quick Start](#quick-start)
- [Testing & CI/CD](#testing--cicd)
- [Security & Compliance Safeguards](#security--compliance-safeguards)
- [Hackathon Submission](#hackathon-submission)
- [Team](#team)
- [License](#license)

---

## Problem Statement

Financial institutions in Vietnam and across Southeast Asia face three critical compliance challenges that traditional manual processes cannot scale to address:

| Challenge | Current Pain Point | Impact |
|---|---|---|
| **Deepfake Identity Fraud** | Fraudsters use AI-generated faces and altered Citizen Identity Card documents to bypass KYC onboarding | Banks unknowingly onboard criminal accounts, exposing them to regulatory penalties and financial loss |
| **Money Laundering Networks** | Sophisticated layering via mule accounts, structuring (smurfing), and circular flows evade rule-based detection | Billions of VND laundered through complex transaction webs that manual review cannot catch in real-time |
| **Regulatory Reporting Burden** | Each Suspicious Transaction Report (STR) requires **2-4 hours** of manual drafting by compliance officers | Backlogs grow, regulatory deadlines are missed, and banks face fines from the State Bank of Vietnam |

> These are not hypothetical problems. Vietnam's banking sector processes millions of digital transactions daily, and the State Bank of Vietnam (SBV) requires all financial institutions to file STRs for suspicious activity under Circular 09/2023/TT-NHNN. Manual compliance workflows are no longer sustainable.

---

## What is TrueTrace?

TrueTrace is a **production-grade, autonomous compliance platform** that deploys three specialized AI agents working as a collaborative team to automate the entire compliance lifecycle -- from identity verification to real-time fraud containment to regulatory reporting:

| Agent | Problem Solved | How It Works | Performance |
|-------|---------------|-------------|:-----------:|
| **Deepfake Inspector** | Fraudulent identity during eKYC onboarding | Alibaba Qwen-VL Vision AI analyzes selfies & Citizen Identity Card documents, detects deepfakes, validates face-match and liveness | **< 10 seconds** |
| **Money-Trail Explorer** | Money laundering via mule accounts, structuring, circular flows | Builds real-time transaction graph with sliding window, detects **6 AML patterns** simultaneously, **auto-freezes** accounts when risk > threshold | **Real-time** |
| **AML Report Generator** | Manual STR report writing (2-4 hours per report) | Qwen LLM generates bilingual (English/Vietnamese) Suspicious Transaction Reports from evidence packages | **< 1 minute** |

> **Production guardrail**: TrueTrace is an AI decision-support system. AI findings are not legal conclusions. STR submission always requires an authorized human reviewer. Account freezing is a reversible internal action.

### Key Differentiators

- **Multi-Agent Collaboration**: Three autonomous agents share context through an event-driven pipeline, not isolated tools
- **Real-Time Containment**: Auto-freeze happens within the same transaction processing window -- not hours later
- **Human-in-the-Loop by Design**: Every AI decision is auditable and requires human approval before regulatory action
- **Bilingual Compliance**: STR reports generated in both English and Vietnamese for SBV regulatory requirements
- **Full-Stack Production Architecture**: Not a prototype -- 11-container microservices platform with CI/CD, monitoring, and infrastructure-as-code

---

## System Architecture

TrueTrace is deployed on **Alibaba Cloud** using a multi-zone, event-driven enterprise architecture powered by Apache Kafka, Redis, ApsaraDB for PostgreSQL, and Alibaba Cloud AI services (Qwen-VL, DashScope LLM API):

![TrueTrace System Architecture Deployment on Alibaba Cloud](docs/images/system-architecture.jpg)

### Multi-Tier Architecture Breakdown

| Tier | Components | Purpose |
|------|-----------|---------|
| **Client Applications** | Next.js Web Portal, React Admin Dashboard (SOC Console), Flutter Mobile Banking App | Multi-channel customer and officer interfaces |
| **API Gateway & Backends** | Alibaba Cloud DNS > SLB > WAF > Nginx > Spring Boot (Java 17) + Go Dashboard Proxy | Secure routing, load balancing, and core banking API |
| **Event Streaming** | Apache Kafka (6 topics) + Redis state cache | Real-time event bus for KYC/transaction/alert/STR feeds |
| **Multi-Agent AI Engine** | Python AsyncIO orchestrator with 3 autonomous agents | Deepfake detection, graph analytics, and STR generation |
| **Data & Storage** | ApsaraDB for PostgreSQL (Primary + Standby) + Alibaba Cloud OSS | Compliance data persistence and evidence storage |
| **Observability** | RAM (IAM), KMS, ActionTrail, Cloud Firewall, ARMS, SLS, Prometheus, Grafana | Security, monitoring, and audit trail |

---

## System Design & Data Flow

The platform executes an end-to-end data processing and multi-agent orchestration workflow:

![TrueTrace System Design Workflow](docs/images/system-design.jpg)

### Pipeline Stages

1. **User Interaction**: Users onboarding via Web/Mobile apps submit eKYC photo/video or initiate financial transactions.
2. **Event Streaming & Ingestion**: Kafka Producers publish KYC & Transaction events into the Kafka Cluster, supported by Schema Registry and cold topic storage for retention & replay.
3. **Parallel Multi-Agent AI Processing**:
   - **Event Dispatcher & Feature Store**: Routes real-time features and event feeds to the Orchestrator & Workflow Engine.
   - **3 Autonomous Agents**: Deepfake Inspector (Qwen-VL), Money-Trail Explorer (Graph analytics), and AML Reporter (LLM narrative).
   - **Result Aggregator & Risk Engine**: Normalizes results, performs risk scoring, deduplication, and threshold evaluation.
4. **Compliance Dashboard & Human Guardrail**: React Compliance Console for human-in-the-loop review, case decisions, manual overrides, and authorized STR regulatory submissions.
5. **Alibaba Cloud Infrastructure**: Integrated with ApsaraDB for PostgreSQL, AnalyticDB, OSS, Redis, PAI-EAS, Model Studio (Qwen LLM API), ARMS, SLS, and WAF security.

---

## Demo Scenario: End-to-End Compliance Pipeline

TrueTrace orchestrates a seamless, multi-agent compliance workflow from onboarding identity protection to real-time asset containment and automated regulatory filing:

```mermaid
sequenceDiagram
    autonumber
    actor Customer as Customer (Web/Mobile)
    participant CoreAPI as Spring Backend API
    participant Kafka as Kafka Event Bus
    participant Agent1 as Agent 1: Deepfake Inspector
    participant Agent2 as Agent 2: Money-Trail Explorer
    participant Agent3 as Agent 3: AML STR Reporter
    actor Officer as Compliance Officer (SOC Console)

    note over Customer, Agent1: Act 1: Identity Onboarding & Deepfake Verification
    Customer->>CoreAPI: Submit KYC (Selfie & Citizen ID Document)
    CoreAPI->>Kafka: Publish Event (truetrace.kyc.submissions)
    Kafka->>Agent1: Consume KYC Event
    Agent1->>Agent1: Qwen-VL Vision AI (Face Match & Liveness <10s)
    Agent1-->>CoreAPI: Return Verification Result (APPROVED)

    note over Customer, Agent2: Act 2: Real-Time Fraud & Money Laundering Containment
    Customer->>CoreAPI: Initiate Rapid Transactions (VND 1B Structuring)
    CoreAPI->>Kafka: Publish Event (truetrace.transactions)
    Kafka->>Agent2: Consume Transaction Feed
    Agent2->>Agent2: Real-Time Graph Analytics (Sliding Window 60s)
    alt Risk Score >= 7.0 (Mule Account / Rapid Fan-Out)
        Agent2-->>CoreAPI: Trigger AUTO-FREEZE Account & Create Alert
        Agent2->>Kafka: Publish Alert (truetrace.alerts)
    end

    note over Agent3, Officer: Act 3: Autonomous Regulatory Reporting & Human Guardrail
    Kafka->>Agent3: Consume High-Risk Alert & Evidence Package
    Agent3->>Agent3: Qwen LLM Auto-Generates Bilingual STR Narrative (<1 min)
    Agent3-->>CoreAPI: Save STR Draft (Status: DRAFT)
    CoreAPI-->>Officer: Display Case Alert on SOC Dashboard
    Officer->>CoreAPI: Human Review & Official Regulator Filing
```

### Detailed Pipeline Breakdown

| Act | Stage & Focus | Trigger & Input | AI & Engine Action | Automated Result | Latency |
|:---:|:---|:---|:---|:---|:---:|
| **Act 1** | **Identity Onboarding**<br/>*(Deepfake Inspector)* | Selfie photo & Citizen Identity Card upload | **Qwen-VL Vision AI** evaluates face match, document integrity, and liveness scoring. | Auto **`APPROVE`**, **`REJECT`**, or escalate to **`MANUAL_REVIEW`** | **< 10s** |
| **Act 2** | **AML Containment**<br/>*(Money-Trail Explorer)* | High-velocity transactions (e.g. 1B VND to 20 targets in 60s) | **Graph Analytics Engine** tracks 6 laundering patterns (Mule dispersion, Structuring, Circular flow). | **Auto-Freeze Account** + Generate high-priority AML alert (Score >= 7.0) | **Real-Time** |
| **Act 3** | **Regulatory Reporting**<br/>*(AML STR Reporter)* | High-risk alert & complete evidence payload | **Qwen LLM Narrative Generator** compiles bilingual (EN/VI) Suspicious Transaction Report. | Save **`DRAFT` STR** on SOC Dashboard ready for human review & submission | **< 1 min** |

> **Autonomous Compliance Transformation**: Traditional banking compliance requires **2 to 4 hours** per manual STR report. TrueTrace reduces the end-to-end cycle -- from fraud containment to regulatory filing -- to just **seconds** with strict **Human-in-the-Loop** review boundaries.

---

## AML Detection Patterns

The Money-Trail Explorer Agent implements six concurrent pattern-detection algorithms against a real-time sliding-window transaction graph:

| Pattern | Risk Points | Description | Detection Method |
|---------|:-----------:|-------------|-----------------|
| Fan-out | +4 | One sender distributes to many recipients rapidly | Graph outdegree analysis within time window |
| Fan-in | +4 | Many senders converge funds to one recipient | Graph indegree analysis within time window |
| Circular Flow | +6 | Cycle detection up to 5 hops (A > B > C > A) | DFS-based cycle detection on transaction graph |
| Velocity Anomaly | +3 | High transaction volume within 1 hour | Statistical deviation from historical baseline |
| Structuring | +5 | Amounts just beneath reporting limits (e.g. VND 200M threshold) | Near-threshold accumulation within sliding window |
| Rapid Mule Dispersion | +8 | Immediate forwarding of received funds (>80% within 60s) | Inflow/outflow ratio analysis with time constraint |

**Freeze threshold**: When cumulative risk score reaches **7/10**, the agent automatically freezes the account, creates an AML alert, and triggers STR generation.

All thresholds are configurable via environment variables. Banks must calibrate using historical data and their own regulatory processes.

---

## Powered by Alibaba Cloud AI & Infrastructure

TrueTrace is built natively on **Alibaba Cloud**'s enterprise AI and cloud-native infrastructure, leveraging Model Studio and DashScope APIs for multi-modal and large language model inference:

| Component | Alibaba Cloud Service | How TrueTrace Uses It |
|-----------|----------------------|-----------------------|
| **Vision AI** | **Qwen-VL (Model Studio)** | eKYC selfie & Citizen Identity Card inspection, facial landmark verification, GAN deepfake artifact detection, and document liveness in <10s |
| **Language Model** | **Qwen-Plus (DashScope)** | Autonomous generation of formal, bilingual (English/Vietnamese) Suspicious Transaction Reports (STR) compliant with SBV regulations |
| **Database** | **ApsaraDB for PostgreSQL** | Production relational persistence for core banking ledgers, KYC verification sessions, AML alerts, and STR report drafts |
| **Message Streaming** | **ApsaraMQ for Kafka** | Distributed high-throughput event streaming bus coordinating 6 real-time compliance topics across 3 autonomous AI agents |
| **Caching & State** | **ApsaraDB for Redis** | High-speed sliding-window transaction graph state cache and rate-limiting session store |
| **Load Balancing** | **Server Load Balancer (SLB)** | High-availability traffic routing and TLS termination for banking web clients and compliance dashboard |
| **Evidence Storage** | **Object Storage Service (OSS)** | Secure storage for encrypted KYC biometrics, deepfake analysis artifacts, and generated STR PDF reports |
| **Observability** | **Simple Log Service (SLS)** | Centralized audit trail logging, security event tracking, and regulatory compliance monitoring |
| **Infrastructure as Code** | **Terraform (alicloud provider)** | Automated 5-tier VPC network isolation, security group policies, and pay-as-you-go cloud resource provisioning |

---

## Built with Qoder: Spec-Driven Development

TrueTrace was built using **[Qoder](https://qoder.com)**'s AI-Powered Spec-Driven Development methodology. Rather than writing boilerplate code manually, we focused on defining clear specifications and letting Qoder generate the implementations.

### How We Used Qoder

| Qoder Feature | How We Used It | Impact |
|--------------|---------------|--------|
| **Spec-Driven Workflow** | Wrote detailed specifications for every module (entities, business logic, API contracts) before code generation | Consistent architecture across 11 microservices |
| **Quest Mode** | Bootstrapped entire services by defining multi-step quests (e.g., "Build Agent Engine: Kafka Consumers > Async Loop > State Management") | Autonomous multi-file code generation maintaining context |
| **Expert Mode & CLI** | Targeted specific files for rapid iteration (e.g., adding sliding-window graph algorithms, optimizing PostgreSQL indexing) | Fast iteration without context-switching |
| **AI Pair Programming** | Debugged async timeout issues, refactored Kafka consumer logic, optimized database queries | Expert-level code review and optimization |

### Development Velocity

| Metric | Value |
|--------|-------|
| Development Time | **2 weeks** (estimated traditional: 3-4 months) |
| Code Generated by Qoder | **~85%** of boilerplate (15,000+ lines) |
| Time Saved | **~80 hours** of manual coding |
| Focus Areas | AI prompt engineering, graph analysis logic, and compliance domain expertise |

### Example Qoder Prompts Used

All prompts used to build TrueTrace are documented in [`SPEC.md`](SPEC.md), including:

1. **Spring Boot Banking Backend** -- Core banking APIs, JPA entities, Kafka producers
2. **Python Agent Engine** -- AsyncIO + aiokafka orchestrator for multi-agent dispatch
3. **Deepfake Inspector Agent** -- Qwen-VL integration with structured JSON output and exponential backoff
4. **Money-Trail Analyzer** -- Sliding-window graph analysis for 6 concurrent AML patterns
5. **AML Reporter Agent** -- Bilingual STR generation with Qwen LLM
6. **React Compliance Dashboard** -- Real-time monitoring console with KPI cards and alert feeds
7. **Next.js Customer Portal** -- Multi-step registration, KYC upload, and transfer forms
8. **Docker Compose Orchestration** -- 11-container microservices with networking and health checks
9. **Terraform Alibaba Cloud Infrastructure** -- VPC, ApsaraDB, Redis, ApsaraMQ Kafka, ECS deployment

---

## Project Directory Structure

TrueTrace is organized as a Git superproject with 10 submodules, each maintained in its own repository under the [Little-Boy-s-TrueTrace](https://github.com/Little-Boy-s-TrueTrace) GitHub organization:

| Directory | Language / Tech Stack | Description |
|---|---|---|
| **[truetrace-backend](https://github.com/Little-Boy-s-TrueTrace/truetrace-backend)** | Java 17 / Spring Boot 3 / Maven | Core banking ledger, accounts, transactions, KYC/AML/STR API endpoints, and Kafka event publisher |
| **[truetrace-agent-engine](https://github.com/Little-Boy-s-TrueTrace/truetrace-agent-engine)** | Python 3.12 / AsyncIO / Kafka / Redis | Multi-Agent orchestrator coordinating 3 AI agents consuming Kafka compliance events |
| **[agent-deepfake-inspector](https://github.com/Little-Boy-s-TrueTrace/agent-deepfake-inspector)** | Python / Qwen-VL Vision | Prompt configuration, validation schemas, and policy documentation for the Deepfake KYC agent |
| **[agent-money-trail](https://github.com/Little-Boy-s-TrueTrace/agent-money-trail)** | Python / Graph Analytics | Prompt configuration and heuristics for tracking structuring, velocity, mule-account, and circular flows |
| **[agent-aml-reporter](https://github.com/Little-Boy-s-TrueTrace/agent-aml-reporter)** | Python / Qwen LLM | Prompt configuration for bilingual STR narrative generation compliant with SBV regulations |
| **[truetrace-dashboard](https://github.com/Little-Boy-s-TrueTrace/truetrace-dashboard)** | React 19 / TypeScript / Go | Compliance admin console (SOC) for bank officers to audit KYC sessions, AML alerts, and STR reports |
| **[truetrace-web-client](https://github.com/Little-Boy-s-TrueTrace/truetrace-web-client)** | Next.js 16 / Node 22 | Customer portal enabling users to register, log in, transfer money, and submit KYC documents |
| **[truetrace-mobile-app](https://github.com/Little-Boy-s-TrueTrace/truetrace-mobile-app)** | Flutter 3 / Dart | Customer mobile banking application with transactions, transfer forms, and balance views |
| **[truetrace-deployment](https://github.com/Little-Boy-s-TrueTrace/truetrace-deployment)** | Docker Compose / Helm / K8s | Container orchestration, Nginx gateway, Kafka init scripts, and deployment configurations |
| **[truetrace-terraform](https://github.com/Little-Boy-s-TrueTrace/truetrace-terraform)** | HCL / Terraform | Cloud infrastructure definitions targeting Alibaba Cloud (ECS, ApsaraDB, ApsaraMQ Kafka, Redis, SLB, OSS) |

---

## Quick Start

### Prerequisites

- **Docker Desktop** (v4.x+) installed and running
- **Git** installed
- At least **8 GB RAM** allocated to Docker (Settings > Resources)

### 1. Clone the repository (with all submodules)

```bash
git clone --recursive https://github.com/Little-Boy-s-TrueTrace/truetrace.git
cd truetrace
```

> If you already cloned without `--recursive`, run:
> ```bash
> git submodule update --init --recursive
> ```

### 2. Configure environment and start

```bash
cd truetrace-deployment
cp .env.example .env
docker compose up --build -d --wait
```

First build takes 5-10 minutes. Wait for all containers to be healthy:

```bash
docker compose ps
```

You should see 10 long-running containers healthy. `kafka-init` is a one-shot container and should show `Exited (0)` after creating the six Kafka topics.

### 3. Access the platform

All services are accessed through the Nginx gateway on port 80:

| Service | URL | Credentials |
|---------|-----|-------------|
| **Customer Portal** | http://localhost | Register a new account |
| **Compliance Dashboard** | http://localhost/soc/ | OTP login with operator UID `10001` |
| **Banking API** | http://localhost/api-bank/ | -- |
| **Dashboard API** | http://localhost/api/ | -- |
| **Kafka UI (Kafdrop)** | http://localhost:9000 | -- |

### 4. Run the automated demo verifier

```bash
cd ..
python truetrace-deployment/scripts/full_stack_smoke.py
```

This deterministic verifier validates the entire pipeline end-to-end:
- Creates run-specific accounts and verifies both KYC decision branches
- Enforces KYC approval before every source account can transfer
- Verifies Kafka offset increases on all six topics and PostgreSQL row persistence
- Executes VND 1 billion inflow with 20-target fan-out triggering rapid mule dispersion detection
- Validates account freeze, AML alert creation, and linked DRAFT STR generation
- Tests structuring detection with near-threshold VND 190M transfers
- Verifies STR review/submission human-in-the-loop workflow


### Troubleshooting

```bash
# View logs for a specific service
docker compose logs -f backend
docker compose logs -f agent-engine

# Restart everything
docker compose down -v
docker compose up --build -d

# Check if backend is healthy
curl http://localhost/api-bank/health
```

---

## Testing & CI/CD

### Automated Test Suite

TrueTrace maintains **87+ automated tests** across all three tiers:

| Tier | Framework | Tests | Coverage Areas |
|------|-----------|:-----:|----------------|
| **Agent Engine** (Python) | pytest | 24 | Deepfake detection, graph analysis (6 AML patterns), STR generation, multi-agent pipeline E2E |
| **Backend** (Java) | JUnit 5 + Spring Boot Test | 36 | Account CRUD, transaction posting, KYC workflow, AML freeze/unfreeze, STR lifecycle, Kafka publishing |
| **Dashboard Frontend** (React) | Vitest + React Testing Library | 27 | Alert rendering, search filtering, freeze/unfreeze actions, KYC verification, compliance overview |

Run all tests:

```bash
# Agent Engine
cd truetrace-agent-engine && python -m pytest -q

# Spring Backend
cd truetrace-backend && mvn test

# Dashboard API (Go)
cd truetrace-dashboard/backend && go test ./...

# Dashboard UI (React)
cd truetrace-dashboard/frontend && npm ci && npm test

# Customer Portal (Next.js)
cd truetrace-web-client && npm ci && npm test
```

### CI/CD Pipeline

Automated CI/CD runs on every push to `main` and `develop` via GitHub Actions:

```
agent-tests ──┐
backend-tests ─┤
dashboard-tests┼──> docker-e2e (Full-stack smoke test)
web-client-tests┘
```

- **Unit gates**: Python pytest, Java Maven, Go test, Vitest, Next.js Jest
- **Lint & build gates**: ESLint, TypeScript strict mode, Terraform validate
- **Integration gate**: Docker Compose full-stack boot + deterministic KYC/AML/STR smoke test
- **Security scanning**: Trivy container vulnerability scanning on Docker images
- **Docker Hub**: Automated image push on `main` branch merges

See [`.github/workflows/full-stack-ci.yml`](.github/workflows/full-stack-ci.yml) for the complete pipeline definition.

---

## Security & Compliance Safeguards

| Safeguard | Description |
|-----------|-------------|
| **Internal Token Sync** | All microservice-to-microservice communications authenticated via `TRUETRACE_SECURITY_SYNC_TOKEN` |
| **Log Sanitization** | Automatically strips credit card numbers, passwords, and JWT secrets from logs (PCI-DSS compliance) |
| **Mule Account Containment** | Auto-freeze triggered at risk score >= 7.0 with reversible internal action |
| **Human-in-the-Loop STR** | STRs always saved as `DRAFT`; submission requires named reviewer approval |
| **Biometric Data Protection** | Raw biometrics only in short-lived processing events; database stores evidence references only |
| **Secret Management** | All secrets via environment variables / secret manager; never committed to Git |
| **Production Hardening** | Agent Engine refuses to start in `production` mode with demo/default configuration |

See [`docs/PRODUCTION-READINESS.md`](docs/PRODUCTION-READINESS.md) for the complete production deployment checklist.

---

## Hackathon Submission

### Alibaba Cloud & Qoder Hackathon HCMC -- FSI (Financial Services Industry)

| Item | Details |
|------|---------|
| **Event** | [Alibaba Cloud & Qoder Hackathon HCMC 2026](https://lu.ma/h8u9qz3b) |
| **Track** | BUILD (Developer Track) |
| **Team** | Little Boy's |
| **AI Provider** | Alibaba Cloud Model Studio -- Qwen-VL (Vision), Qwen-Plus (LLM) via DashScope API |
| **Dev Tool** | [Qoder](https://qoder.com) -- Spec-Driven AI Development (Quest Mode, Expert Mode, CLI) |
| **Source Code** | [github.com/Little-Boy-s-TrueTrace](https://github.com/Little-Boy-s-TrueTrace) (10 repositories) |

---

## Team

**Little Boy's** -- Alibaba Cloud & Qoder Hackathon HCMC 2026

We are a team of developers passionate about applying AI to solve real-world problems in the financial services industry. TrueTrace represents our vision of how multi-agent AI systems can transform regulatory compliance from a manual burden into an autonomous, real-time defense layer.

---

## License

This project is developed for the Alibaba Cloud & Qoder Hackathon HCMC 2026. The source code belongs entirely to the team; Alibaba and Qoder do not require submission of source code.
