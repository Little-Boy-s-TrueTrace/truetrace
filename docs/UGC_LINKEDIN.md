# LinkedIn Post -- TrueTrace by Little Boy's

A compliance officer can spend up to 4 hours investigating and drafting a single Suspicious Transaction Report. What if three autonomous AI agents could detect, contain, and report financial crime in seconds?

Financial institutions face three critical compliance challenges today:
- Fake identities bypassing eKYC via deepfakes and altered documents
- Hidden money laundering networks evading traditional rule-based systems
- Time-consuming compliance reporting creating massive operational backlogs

That inspired team Little Boy's to build **TrueTrace** for the #QoderHackathon -- an Autonomous Multi-Agent AI Compliance Platform built to revolutionize how financial institutions handle AML and eKYC.

Instead of relying on a monolithic AI model or manual reviews, TrueTrace orchestrates three autonomous AI agents that collaborate in real-time like an experienced compliance team:

- Deepfake Inspector: Analyzes KYC selfies and Citizen Identity Card images using Alibaba Cloud Qwen-VL Vision AI to detect deepfakes, document tampering, and liveness in <10s.
- Money-Trail Explorer: Tracks real-time transaction graph streams to identify 6 complex money laundering patterns (mule accounts, structuring, circular flows) and trigger automated risk containment.
- AML STR Reporter: Generates bilingual (English/Vietnamese) Suspicious Transaction Reports using Qwen LLM to save compliance officers hours of manual paperwork.

Building a full-stack architecture across 11 microservices in just a few days was a massive endeavor. Qoder became far more than just a coding assistant -- it served as our AI-powered Spec-Driven Development engine.

Qoder helped us:
- Break complex compliance requirements into executable specifications (`SPEC.md`)
- Parallelize multi-repo development using Quest & Expert Mode with autonomous AI subagents
- Rapidly iterate and debug real-time streaming pipelines across 11 repositories
- Verify generated code with automated unit and full-stack integration tests

We wired it all together using an enterprise tech stack: Spring Boot 3, Python AsyncIO, React 19, Next.js, Apache Kafka, ApsaraDB for PostgreSQL, and Alibaba Cloud AI services.

We cannot wait to present this to the judges and community!

How do you see multi-agent AI systems reshaping regulatory compliance in banking? We would love to hear your thoughts!

-- Team Little Boy's

@Qoder @Alibaba Cloud #QoderHackathon #QoderVietnam #AML #KYC #FinTech #AI #AlibabaCloud #Qwen
