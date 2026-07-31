# TrueTrace: Spec-Driven AI Development with Qoder

This document outlines the **Spec-Driven AI development workflow** used to build TrueTrace for the **Alibaba Cloud & Qoder Hackathon HCMC -- FSI**. TrueTrace was built by the **Little Boy's** team using [Qoder](https://qoder.com) as a core development partner, resulting in a full-scale microservices architecture delivered in a fraction of the traditional timeframe.

> **Navigation:** [README](README.md) | [Architecture](docs/ARCHITECTURE.md) | [Test Matrix](docs/TESTING.md) | [Production Readiness](docs/PRODUCTION-READINESS.md) | [Demo Runbook](docs/DEMO-RUNBOOK.md)

---

## Table of Contents

- [1. Problem Statement](#1-problem-statement)
- [2. Solution: TrueTrace Multi-Agent Platform](#2-solution-truetrace-multi-agent-platform)
- [3. Alibaba Cloud Integration](#3-alibaba-cloud-integration)
- [4. Qoder Development Workflow](#4-qoder-development-workflow)
- [5. Development Timeline](#5-development-timeline)
- [6. Module Generation Prompts](#6-module-generation-prompts)
- [7. AI-Accelerated Results](#7-ai-accelerated-results)
- [8. Key Technical Decisions Made with Qoder](#8-key-technical-decisions-made-with-qoder)
- [9. Before vs. After: Manual Compliance vs. TrueTrace](#9-before-vs-after-manual-compliance-vs-truetrace)
- [10. Testing & Verification](#10-testing--verification)

---

## 1. Problem Statement

Financial institutions in Vietnam face an escalating compliance crisis. The State Bank of Vietnam (SBV) mandates strict KYC (Know Your Customer) verification under Circular 09/2023/TT-NHNN, and all banks must file Suspicious Transaction Reports (STRs) for suspected money laundering activities. However, the current compliance process is overwhelmingly manual:

| Challenge | Current Reality | Business Impact |
|---|---|---|
| **Deepfake Identity Fraud** | AI-generated faces and altered Citizen ID cards (CCCD) bypass manual KYC review | Criminal accounts are onboarded unknowingly, exposing banks to regulatory fines and fraud losses |
| **Complex Money Laundering** | Sophisticated layering via mule accounts, structuring (smurfing), and circular flows across hundreds of accounts | Traditional rule-based systems detect only simple patterns; complex networks evade detection entirely |
| **STR Reporting Burden** | Each STR requires **2-4 hours** of manual drafting by compliance officers, including evidence gathering, narrative writing, and bilingual translation | Growing backlogs, missed regulatory deadlines, and significant labor costs per report |

**The core question we set out to answer:** *Can autonomous AI agents collaborate to detect, contain, and report financial crime in real-time -- while keeping humans in the loop for final decisions?*

---

## 2. Solution: TrueTrace Multi-Agent Platform

TrueTrace deploys **three specialized AI agents** working as a collaborative team to automate the entire compliance lifecycle:

```
Customer Onboarding          Real-Time Monitoring         Regulatory Filing
       |                            |                           |
  [Agent 1]                    [Agent 2]                   [Agent 3]
  Deepfake Inspector       Money-Trail Explorer        AML STR Reporter
       |                            |                           |
  Qwen-VL Vision AI        Graph Analytics Engine       Qwen LLM Narrative
       |                            |                           |
  APPROVE / REJECT          AUTO-FREEZE Account         DRAFT STR Report
  (< 10 seconds)             (Real-time)                (< 1 minute)
       |                            |                           |
       +------- Human-in-the-Loop Review (SOC Dashboard) ------+
```

**Tech Stack:**
- **Backend:** Spring Boot (Java 17) -- core banking ledger, accounts, transactions, KYC/AML/STR APIs
- **Agent Engine:** Python 3.12 AsyncIO -- multi-agent orchestrator consuming Kafka events
- **Frontend:** React 19 (Compliance Dashboard), Next.js 16 (Customer Portal), Flutter 3 (Mobile App)
- **Infrastructure:** Apache Kafka (event streaming), Redis (state cache), PostgreSQL (compliance data), Docker Compose, Terraform
- **AI Models:** Alibaba Cloud Qwen-VL (Vision) and Qwen-Plus (LLM) via DashScope API
- **Cloud Platform:** Alibaba Cloud (ApsaraDB, OSS, Model Studio, ARMS, SLS, WAF)

---

## 3. Alibaba Cloud Integration

TrueTrace is designed as an **Alibaba Cloud-native** compliance platform. Every AI capability is powered by Alibaba Cloud services:

| Alibaba Cloud Service | TrueTrace Usage | Agent |
|---|---|---|
| **Model Studio / Qwen-VL** | Vision AI for face matching, liveness detection, document integrity analysis on selfie + CCCD photos | Deepfake Inspector (Agent 1) |
| **DashScope / Qwen-Plus** | LLM for bilingual (EN/VI) Suspicious Transaction Report narrative generation from structured evidence | AML STR Reporter (Agent 3) |
| **ApsaraDB for PostgreSQL** | Primary + Standby with replication for compliance data persistence (accounts, transactions, KYC sessions, AML alerts, STR reports) | All Agents |
| **Alibaba Cloud OSS** | Object storage for evidence files (KYC photos, signed STR PDFs, audit logs) | Agent 1, Agent 3 |
| **ARMS (Application Real-Time Monitoring)** | Full-stack observability, distributed tracing, and performance monitoring | Infrastructure |
| **SLS (Simple Log Service)** | Centralized log aggregation, compliance audit trail, and alert-driven log analysis | Infrastructure |
| **Cloud Firewall / WAF** | Web Application Firewall protecting API endpoints against DDoS, SQL injection, and XSS attacks | Infrastructure |
| **RAM / KMS** | Identity and access management (IAM), secret encryption, and key rotation for compliance credentials | Infrastructure |
| **ActionTrail** | API call auditing for all cloud resource operations (regulatory compliance evidence) | Infrastructure |

### Why Alibaba Cloud?

1. **Qwen-VL** provides state-of-the-art vision-language understanding for document verification -- superior to generic OCR for detecting AI-generated/altered ID documents.
2. **Qwen-Plus** delivers high-quality bilingual text generation in both English and Vietnamese -- critical for STR reports submitted to the State Bank of Vietnam.
3. **ApsaraDB for PostgreSQL** offers enterprise-grade managed database with automatic failover, encryption at rest, and point-in-time recovery -- essential for financial compliance data.
4. **Full observability stack** (ARMS + SLS + ActionTrail) provides the audit trail that regulators require.

---

## 4. Qoder Development Workflow

The development of TrueTrace heavily relied on **Qoder's Spec-Driven Development** methodology. Rather than writing boilerplate code manually, we focused on defining clear, robust specifications and letting Qoder generate the implementations.

### Spec-Driven Development

We adopted a "Spec-First" approach. For every module, we wrote a detailed specification in plain English (or pseudo-code) describing the entities, business logic, and API contracts. Qoder ingested these specs and generated the initial scaffolding, database schemas, and API routes.

**Our workflow:**
```
1. Write Spec (entities, APIs, business rules)
       |
2. Qoder Quest Mode (autonomous multi-file generation)
       |
3. Review & Test (run unit tests, manual QA)
       |
4. Iterate with Expert Mode / CLI (refine specific files)
       |
5. Integration Test (Docker Compose full-stack)
       |
6. Ship
```

### Quest Mode

Qoder's **Quest Mode** was pivotal in bootstrapping entire microservices. By defining a "Quest" with multiple sub-tasks (e.g., "Build the Agent Engine: Task 1 - Kafka Consumers, Task 2 - Async Loop, Task 3 - State Management"), Qoder autonomously worked through the steps, maintaining context across multiple files and creating cohesive services.

**Example Quest we used:**
```
Quest: Build the Multi-Agent Engine
  Task 1: Set up Python AsyncIO + aiokafka consumer framework
  Task 2: Implement Deepfake Inspector Agent with Qwen-VL integration
  Task 3: Implement Money-Trail Graph Analyzer with sliding window
  Task 4: Implement AML STR Reporter with Qwen LLM
  Task 5: Wire agent results back to Spring Boot via REST callbacks
  Task 6: Add Redis state management for agent locks and deduplication
```

### CLI Integration

For rapid iteration, we used the **Qoder CLI**. When we needed to modify a specific service (like adding a new sliding-window graph algorithm), we used the CLI to target specific files and provide natural language instructions, bypassing the need to context-switch into the IDE for every small change.

**Example CLI commands:**
```bash
# Add structuring detection to Money-Trail Agent
qoder edit agents/money_trail.py "Add a structuring detection pattern that identifies
transactions just below VND 200M threshold within a 60-second sliding window"

# Fix Kafka consumer offset management
qoder fix agents/main.py "The consumer is not committing offsets after processing,
causing duplicate event handling on restart"
```

### AI Pair Programming

Qoder acted as an expert pair programmer. When integrating Alibaba Cloud's Qwen models, Qoder helped debug async timeout issues in Python, refactored our Kafka consumer logic for higher throughput, and optimized our PostgreSQL indexing strategy for the graph database.

**Key moments where Qoder pair programming saved us:**
1. **Async/Await Deadlock:** Qoder identified a deadlock in our Kafka consumer where `await` calls inside the message loop were blocking the event loop, causing missed heartbeats and consumer group rebalancing.
2. **Base64 Image Encoding:** Qoder caught an issue where our Qwen-VL integration was sending unpadded base64 strings, causing silent API failures.
3. **Transaction Atomicity:** Qoder recommended wrapping account freeze + alert creation + STR generation in a single transactional boundary to prevent partial state on backend failures.

---

## 5. Development Timeline

| Phase | Duration | Qoder Mode | What We Built |
|-------|----------|-----------|---------------|
| **Spec Writing** | Day 1-2 | -- | Problem definition, architecture diagrams, API contracts, entity schemas |
| **Backend Bootstrap** | Day 3-4 | Quest Mode | Spring Boot banking API, JPA entities, Kafka producers, account/transaction/KYC/AML/STR endpoints |
| **Agent Engine** | Day 5-7 | Quest Mode + Expert Mode | Python AsyncIO orchestrator, 3 AI agents, Qwen-VL/Qwen-Plus integration, graph analyzer |
| **Frontend & Dashboard** | Day 8-10 | Quest Mode | React compliance dashboard, Next.js customer portal, Go API proxy |
| **Mobile App** | Day 10-11 | Quest Mode | Flutter mobile banking app with transfers and balance views |
| **Infrastructure** | Day 11-12 | Expert Mode + CLI | Docker Compose (11 containers), Nginx gateway, Kafka topics, Terraform AWS |
| **Testing & Polish** | Day 13-14 | CLI + Expert Mode | 87+ unit tests, CI/CD pipeline, deterministic smoke test, demo runbook |

**Total: 2 weeks** (Estimated traditional development time: 3-4 months with a team of 4-5 developers)

---

## 6. Module Generation Prompts

Below are the actual prompts and specifications used with Qoder to generate the TrueTrace platform. These demonstrate the **Spec-Driven** approach: we describe *what* we want, and Qoder generates *how* to build it.

### Prompt 1: Spring Boot Banking Backend
*Used to bootstrap the core banking APIs.*

```markdown
Generate a Spring Boot (Java 17) microservice for core banking operations.
Include the following:
1. JPA Entities: `Account` (id, user_id, balance, status), `Transaction` (id, sender_id, receiver_id, amount, timestamp, type, status).
2. Repositories for both entities.
3. A Service layer that handles transaction execution with transactional integrity (@Transactional).
4. REST Controllers for:
   - POST /api/v1/transactions (Create transaction)
   - GET /api/v1/accounts/{id}/history (Get transaction history)
5. Include a Kafka Producer that publishes every successful transaction to the `tx-events` topic.
Use Lombok for boilerplate reduction and structure the project using standard controller/service/repository layers.
```

### Prompt 2: Python Agent Engine (Kafka & AsyncIO)
*Used in Quest Mode to build the async orchestrator.*

```markdown
Create a Python async application using `asyncio` and `aiokafka`.
This engine will act as the orchestrator for multiple AI agents.
Requirements:
- Connect to Kafka on `localhost:9092`.
- Consume from topics: `tx-events` and `kyc-events`.
- Implement a router function that checks the event type and asynchronously dispatches it to the appropriate agent handler (e.g., `handle_transaction` or `handle_kyc`).
- Include graceful shutdown handling for the Kafka consumer.
- Add basic logging using the `logging` module.
Provide the complete `main.py` and a `requirements.txt`.
```

### Prompt 3: Deepfake Inspector Agent (Alibaba Cloud Qwen-VL)
*Iterative prompt for Alibaba Cloud vision integration.*

**Initial Prompt:**
```text
Write a Python function that takes a base64 encoded image and sends it to the Alibaba Cloud Qwen-VL API to check if the ID card (CCCD) is a deepfake or altered.
```

**Refinement Prompt (after testing):**
```text
The previous function works, but we need to handle rate limits and add structured output.
Update the function to:
1. Implement exponential backoff for HTTP 429 errors.
2. Force the Qwen-VL prompt to return a strict JSON response with keys: `is_fake` (boolean), `confidence_score` (float 0-1), and `reason` (string).
3. If `is_fake` is true, publish an alert to the Kafka `aml-alerts` topic.
```

### Prompt 4: Money-Trail Agent (Graph Analysis)
*Prompt for complex transaction analysis.*

```markdown
Create a Python class `MoneyTrailAnalyzer` that processes a sliding window of transactions to detect money laundering patterns.
The transactions are provided as a list of dictionaries: `[{tx_id, sender, receiver, amount, timestamp}]`.
Implement detection logic for:
1. **Fan-In:** Multiple small transactions from different accounts to one account within 24 hours.
2. **Fan-Out:** One large deposit immediately split into multiple smaller outgoing transfers.
3. **Circular Flow:** Money moving through 3 or more accounts and returning to the original sender.
4. **Velocity Anomaly:** Transaction volume exceeding 500% of the account's 7-day average.
5. **Structuring (Smurfing):** Multiple transactions just below the VND 200M reporting threshold.
6. **Rapid Mule Dispersion:** Immediate forwarding of >80% received funds within 60 seconds.
Return a list of detected anomalies with a risk score (0-10) for each.
Auto-freeze the account and create an AML alert when cumulative risk >= 7.0.
```

### Prompt 5: AML Reporter Agent (Alibaba Cloud Qwen-Plus)
*Prompt for bilingual STR generation using Alibaba Cloud LLM.*

```markdown
Write a Python script that uses the Alibaba Cloud Qwen-Plus LLM (via DashScope API) to generate a Suspicious Transaction Report (STR).
Input: A JSON object containing user details, transaction history, and detected anomalies from the Money-Trail Agent.
Task:
Prompt the LLM to write a formal, professional STR in both English and Vietnamese.
The report must include:
- Executive Summary
- Subject Information
- Details of Suspicious Activity
- Flow of Funds Analysis
- Conclusion
Ensure the tone is objective and suitable for financial regulators (e.g., State Bank of Vietnam).
Save with DRAFT status -- STR submission requires human reviewer approval.
```

### Prompt 6: React Compliance Dashboard
*Prompt for the frontend monitoring system.*

```markdown
Generate a React 19 dashboard using Tailwind CSS and Recharts.
The dashboard should have:
1. A top row of KPI cards: Total Transactions, Flagged Transactions, Pending KYC, High-Risk Accounts.
2. A main area with a real-time line chart showing transaction volume over the last 24 hours.
3. A side panel displaying a live feed of alerts consumed via a WebSocket connection to `ws://localhost:8080/alerts`.
4. A data table showing detailed Suspicious Transaction Reports (STRs) with columns: ID, Subject, Risk Score, Date, Action (Button to View Report).
Make the design dark mode by default, modern, and professional (FSI style).
```

### Prompt 7: Next.js Customer Portal
*Prompt for the client-facing web app.*

```markdown
Create a Next.js (App Router) client portal.
Needs three main pages:
1. `/register`: A multi-step form for user registration.
2. `/kyc`: A page to upload front/back of ID card (CCCD) and a selfie. Connect this to the backend `/api/v1/kyc/upload` endpoint.
3. `/dashboard`: Shows current balance and a form to initiate a transfer (Account Number, Amount, Description).
Use React Hook Form for validation and standard Tailwind classes for styling.
```

### Prompt 8: Flutter Mobile Banking App
*Prompt for the customer mobile application.*

```markdown
Create a Flutter 3 mobile banking application with the following screens:
1. `/login`: User authentication with username and password.
2. `/home`: Account overview showing current balance, recent transactions list, and quick action buttons.
3. `/transfer`: Transfer form with recipient account number, amount, and description fields with real-time validation.
4. `/transactions`: Full transaction history with date filtering and search.
Use Material Design 3 with a professional banking color scheme.
Connect to the Spring Boot backend at `http://localhost/api-bank/`.
```

### Prompt 9: Docker Compose Orchestration
*Prompt for infrastructure setup.*

```markdown
Write a `docker-compose.yml` file to orchestrate the TrueTrace platform.
Include the following services:
- `kafka` (KRaft mode, no Zookeeper) with 6 pre-created topics
- `postgres` (with an init script to create 'truetrace' db)
- `redis`
- `springboot-backend` (build from ./truetrace-backend)
- `python-agent-engine` (build from ./truetrace-agent-engine)
- `react-dashboard` with Go API proxy (build from ./truetrace-dashboard)
- `nextjs-web-client` (build from ./truetrace-web-client)
- `nginx` reverse proxy routing all services through port 80
- `kafdrop` for Kafka topic monitoring on port 9000
Ensure health checks on all services and proper dependency ordering.
Set up a custom bridge network `truetrace-network`.
Total: 11 containers.
```

### Prompt 10: Terraform AWS / Alibaba Cloud Infrastructure
*Prompt for cloud deployment specs.*

```markdown
Write Terraform configuration to deploy the TrueTrace platform to AWS.
Requirements:
1. AWS VPC with 2 public subnets and 2 private subnets.
2. An RDS PostgreSQL instance in the private subnets.
3. An ElastiCache Redis cluster in the private subnets.
4. An MSK (Managed Streaming for Apache Kafka) cluster.
5. An ECS Fargate cluster with task definitions for the Spring Boot backend and Python Agent Engine.
6. An Application Load Balancer routing traffic to the ECS services.
Output the ALB DNS name and RDS endpoint.
```

---

## 7. AI-Accelerated Results

Using Qoder fundamentally changed our development velocity. What traditionally would require a team of 4-5 developers over several months was accomplished by the Little Boy's team during a hackathon timeline.

| Metric | Value | Details |
|--------|-------|---------|
| **Development Time** | **2 weeks** | Estimated traditional time: 3-4 months |
| **Code Generated by Qoder** | **~85%** | 15,000+ lines of boilerplate (entities, controllers, Kafka configs, UI components) |
| **Time Saved** | **~80 hours** | Manual typing, debugging configs, writing repetitive API integrations |
| **Focus Areas** | **15%** | Complex AI prompt engineering, graph analysis logic, and compliance domain expertise |
| **Repositories** | **10** | Full microservices architecture with CI/CD across all repos |
| **Docker Containers** | **11** | Full-stack platform running locally in one command |
| **Automated Tests** | **87+** | Python pytest, Java JUnit 5, Go test, React Vitest, Next.js Jest |
| **Kafka Topics** | **6** | Real-time event streaming for KYC, transactions, findings, alerts, reports, commands |

### What Qoder Generated vs. What We Wrote

| Component | Qoder Generated | Team Wrote |
|-----------|:-:|:-:|
| Spring Boot entities, repositories, controllers | Generated | -- |
| Kafka producer/consumer boilerplate | Generated | -- |
| React dashboard component scaffolding | Generated | -- |
| Next.js page routing and forms | Generated | -- |
| Flutter screens and navigation | Generated | -- |
| Docker Compose and Nginx config | Generated | -- |
| Terraform infrastructure definitions | Generated | -- |
| CI/CD GitHub Actions workflows | Generated | -- |
| Qwen-VL prompt engineering & output parsing | -- | Written |
| Graph analysis algorithms (6 AML patterns) | -- | Written |
| Sliding-window transaction analyzer | -- | Written |
| STR bilingual prompt templates | -- | Written |
| Business logic (freeze, threshold, evidence) | -- | Written |
| Deterministic full-stack smoke test | -- | Written |

---

## 8. Key Technical Decisions Made with Qoder

Throughout the project, Qoder served as an architectural sounding board:

### Decision 1: Event-Driven Architecture vs. REST

**Context:** Initially, we planned for synchronous REST calls between the Spring Boot backend and the AI agents.

**Qoder's recommendation:** Due to the unpredictable latency of LLM and Vision AI calls (Qwen-VL can take 2-10 seconds, Qwen-Plus 5-30 seconds), an event-driven architecture using Kafka would be more resilient and scalable.

**Outcome:** This led to the adoption of a 6-topic Kafka event bus (`truetrace.kyc.submissions`, `truetrace.transactions`, `truetrace.findings.money_trail`, `truetrace.alerts`, `truetrace.reports.str`, `truetrace.commands.str`), enabling true asynchronous multi-agent processing without blocking the core banking API.

### Decision 2: In-Memory Graph vs. Graph Database

**Context:** When tasked with finding circular money flows and mule account networks, we considered deploying Neo4j.

**Qoder's recommendation:** Implement a sliding window approach in memory using Python dictionaries rather than deploying a heavy Graph Database for the MVP. The transaction volume in the demo scenario is bounded, and the sliding window approach provides O(n) complexity for cycle detection.

**Outcome:** Saved significant setup time and infrastructure overhead while still meeting all 6 AML pattern-detection requirements. The graph analyzer processes real-time Kafka events without any external database dependency.

### Decision 3: Frontend State Management

**Context:** We debated using Redux, Zustand, or MobX for the compliance dashboard's real-time state.

**Qoder's recommendation:** Use standard React context combined with polling for the dashboard MVP. The dashboard is a read-heavy application that fetches compliance data from the backend API.

**Outcome:** Bypassed the complexity of WebSocket integration and state management libraries, keeping the dashboard codebase simple and maintainable while still providing real-time compliance monitoring.

---

## 9. Before vs. After: Manual Compliance vs. TrueTrace

| Process Step | Before (Manual) | After (TrueTrace) | Improvement |
|---|---|---|---|
| **KYC Identity Verification** | Manual document review by officer (15-30 min) | Qwen-VL AI auto-analysis (< 10s) | **100x faster** |
| **Fraud Detection Scope** | Rule-based: amount > threshold only | 6 concurrent pattern detection (graph analytics) | **6x broader** |
| **Time to Containment** | Hours to days (manual investigation) | Real-time auto-freeze at risk >= 7.0 | **From hours to seconds** |
| **STR Report Drafting** | 2-4 hours per report (manual) | < 1 minute (Qwen LLM auto-generation) | **120-240x faster** |
| **Language Support** | English only, manual Vietnamese translation | Bilingual EN/VI in single generation | **Built-in** |
| **Human Oversight** | Scattered across emails and spreadsheets | Centralized SOC Dashboard with audit trail | **Single pane of glass** |
| **Scalability** | Linear (1 officer = 1 case) | Parallel (3 agents, unlimited concurrent events) | **Unbounded** |

---

## 10. Testing & Verification

TrueTrace maintains a comprehensive test suite to ensure correctness across all tiers:

### Automated Test Matrix (87+ tests)

| Tier | Framework | Tests | Key Coverage |
|------|-----------|:-----:|-------------|
| **Agent Engine** (Python) | pytest | 24 | Deepfake detection (Qwen-VL mock), graph analysis (6 AML patterns with sliding window), STR generation (bilingual output), multi-agent pipeline E2E |
| **Backend** (Java) | JUnit 5 + Spring Boot Test | 36 | Account CRUD, transaction posting with Kafka, KYC session workflow, AML freeze/unfreeze, STR lifecycle (DRAFT > READY > SUBMITTED), controller integration |
| **Dashboard Frontend** (React) | Vitest + React Testing Library | 27 | Alert rendering, search filtering, card expansion, freeze/unfreeze button actions, KYC session list, compliance overview stats |

### CI/CD Pipeline

Automated GitHub Actions CI/CD runs on every push to `main` and `develop`:

```
agent-tests (Python pytest)  ──┐
backend-tests (Java Maven)   ──┤
dashboard-tests (Go + Vitest) ─┼──> docker-e2e (Full-stack Docker Compose smoke test)
web-client-tests (Next.js)   ──┘
```

### Full-Stack Deterministic Smoke Test

The `full_stack_smoke.py` verifier validates the entire pipeline end-to-end:

1. Creates run-specific accounts and verifies both KYC decision branches (APPROVED + REJECTED)
2. Enforces KYC approval before every source account can transfer
3. Verifies Kafka offset increases on all 6 topics and PostgreSQL row persistence
4. Executes VND 1 billion inflow with 20-target fan-out triggering rapid mule dispersion
5. Validates account freeze, AML alert creation, and linked DRAFT STR generation
6. Tests two VND 190M structuring transfers producing STRUCTURING alert
7. Verifies STR review/submission human-in-the-loop workflow (reject premature submit, then approve)

### Run All Tests

```bash
# Agent Engine
cd truetrace-agent-engine && python -m pytest -q

# Spring Backend
cd truetrace-backend && mvn test

# Dashboard API (Go) + UI (React)
cd truetrace-dashboard/backend && go test ./...
cd truetrace-dashboard/frontend && npm ci && npm test

# Customer Portal (Next.js)
cd truetrace-web-client && npm ci && npm test

# Full-Stack Integration
cd truetrace-deployment && docker compose up -d --build --wait
cd .. && python truetrace-deployment/scripts/full_stack_smoke.py
```

---

*TrueTrace by Little Boy's -- Built with [Qoder](https://qoder.com) for the Alibaba Cloud & Qoder Hackathon HCMC 2026*
