# TrueTrace: Spec-Driven AI Development with Qoder

This document outlines the **Spec-Driven AI development workflow** used to build TrueTrace for the **Alibaba Cloud & Qoder Hackathon HCMC -- FSI**. TrueTrace was built by the **Little Boy's** team using Qoder as a core development partner, resulting in a full-scale microservices architecture delivered in a fraction of the traditional timeframe.

## 1. Project Overview

**TrueTrace** is a Multi-Agent AI Compliance Platform designed for the Financial Services Industry (FSI). It automates and enhances the detection of fraudulent activities, money laundering, and identity spoofing across banking operations.

**Core Problems Solved:**
- **KYC Fraud:** High volume of deepfake and altered ID submissions (CCCD).
- **Complex AML Networks:** Sophisticated money laundering patterns (layering, mule networks) that evade traditional rule-based systems.
- **Reporting Overhead:** The manual, time-consuming process of drafting Suspicious Transaction Reports (STRs) for regulators.

**Tech Stack:**
- **Backend:** Spring Boot (Java 17) for core banking services
- **Agent Engine:** Python AsyncIO for AI orchestration
- **Frontend:** React 19 (Dashboard), Next.js (Web Client), Flutter (Mobile)
- **Infrastructure:** Kafka, Redis, PostgreSQL, Docker Compose, Terraform (AWS)
- **AI Models:** Qwen-VL (Vision), Qwen (LLM) via Alibaba Cloud

---

## 2. Qoder Development Workflow

The development of TrueTrace heavily relied on **Qoder's Spec-Driven Development** methodology. Rather than writing boilerplate code manually, we focused on defining clear, robust specifications and letting Qoder generate the implementations.

### Spec-Driven Development
We adopted a "Spec-First" approach. For every module, we wrote a detailed specification in plain English (or pseudo-code) describing the entities, business logic, and API contracts. Qoder ingested these specs and generated the initial scaffolding, database schemas, and API routes.

### Quest Mode
Qoder's **Quest Mode** was pivotal in bootstrapping entire microservices. By defining a "Quest" with multiple sub-tasks (e.g., "Build the Agent Engine: Task 1 - Kafka Consumers, Task 2 - Async Loop, Task 3 - State Management"), Qoder autonomously worked through the steps, maintaining context across multiple files and creating cohesive services.

### CLI Integration
For rapid iteration, we used the **Qoder CLI**. When we needed to modify a specific service (like adding a new sliding-window graph algorithm), we used the CLI to target specific files and provide natural language instructions, bypassing the need to context-switch into the IDE for every small change.

### AI Pair Programming
Qoder acted as an expert pair programmer. When integrating Alibaba Cloud's Qwen models, Qoder helped debug async timeout issues in Python, refactored our Kafka consumer logic for higher throughput, and optimized our PostgreSQL indexing strategy for the graph database.

---

## 3. Module Generation Prompts

Below are examples of the actual prompts and specifications used with Qoder to generate the TrueTrace platform.

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

### Prompt 3: Deepfake Inspector Agent
*Iterative prompt for vision integration.*

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
5. **Structuring (Smurfing):** Multiple transactions just below the $10,000 reporting threshold.
Return a list of detected anomalies with a risk score (0-100) for each.
```

### Prompt 5: AML Reporter Agent
*Prompt for bilingual STR generation.*

```markdown
Write a Python script that uses the Qwen LLM to generate a Suspicious Transaction Report (STR).
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

### Prompt 8: Docker Compose Orchestration
*Prompt for infrastructure setup.*

```markdown
Write a `docker-compose.yml` file to orchestrate the TrueTrace platform.
Include the following services:
- `zookeeper` and `kafka` (Confluent inc images)
- `postgres` (with an init script to create 'truetrace' db)
- `redis`
- `springboot-backend` (build from ./backend)
- `python-agent-engine` (build from ./agents)
- `react-dashboard` (build from ./dashboard, expose port 3000)
Ensure Kafka is accessible from the host on port 9092 and from other containers.
Set up a custom bridge network `truetrace-network`.
```

### Prompt 9: Terraform AWS Infrastructure
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

## 4. AI-Accelerated Results

Using Qoder fundamentally changed our development velocity. What traditionally would require a team of 4-5 developers over several months was accomplished by the Little Boy's team during a hackathon timeline.

- **Development Speed:** Built a fully functional, event-driven, 11-container microservices platform in **2 weeks** (Estimated traditional time: 3-4 months).
- **Code Generation:** Qoder generated approximately **85%** of the boilerplate code (entities, controllers, Kafka configurations, UI components), amounting to over 15,000 lines of code.
- **Time Saved:** Saved an estimated **80 hours** of manual typing, debugging configuration files, and writing repetitive API integrations, allowing the team to focus entirely on the complex AI prompt engineering and graph analysis logic.

## 5. Key Technical Decisions Made with Qoder

Throughout the project, Qoder served as an architectural sounding board:

1. **Event-Driven Architecture vs. REST:** Initially, we planned for synchronous REST calls between the Spring Boot backend and the AI agents. Qoder suggested that due to the unpredictable latency of LLM and Vision AI calls, an event-driven architecture using Kafka would be more resilient and scalable. This led to the adoption of the Kafka topic structure.
2. **Graph Analysis Approach:** When tasked with finding circular money flows, Qoder recommended implementing a sliding window approach in memory using Python dictionaries rather than deploying a heavy Graph Database (like Neo4j) for the MVP, saving significant setup time and infrastructure overhead while still meeting the hackathon requirements.
3. **Frontend State Management:** Qoder advised using standard React context combined with WebSockets for the live alert feed on the dashboard, bypassing the need for complex state management libraries like Redux for the MVP scope.

---
*TrueTrace by Little Boy's -- Alibaba Cloud & Qoder Hackathon HCMC 2026*
