# LinkedIn Post -- TrueTrace by Little Boy's

A compliance officer can spend up to 4 hours investigating and drafting a single Suspicious Transaction Report. What if three autonomous AI agents could detect, contain, and report financial crime in seconds?

Financial institutions face three critical compliance challenges today:
- Fake identities bypassing eKYC via deepfakes and altered documents
- Hidden money laundering networks evading traditional rule-based systems
- Time-consuming compliance reporting creating massive operational backlogs

That inspired team Little Boy's to build **TrueTrace** for the #QoderHackathon -- an Autonomous Multi-Agent AI Compliance Platform built to revolutionize how financial institutions handle AML and eKYC.

TrueTrace by the numbers:
- 3 autonomous AI agents collaborating in real-time like an experienced compliance team
- <10 seconds eKYC deepfake & document liveness verification
- 6 real-time AML patterns detected (mule accounts, structuring, circular flows)
- 100% automated bilingual (EN/VI) STR report drafting (4 hours reduced to seconds)
- 11 cloud-native microservices deployed on Alibaba Cloud

Instead of relying on a monolithic AI model or manual reviews, TrueTrace orchestrates three specialized autonomous AI agents:

- Deepfake Inspector: Analyzes KYC selfies and Citizen Identity Card images using Alibaba Cloud Qwen-VL Vision AI to detect deepfakes, document tampering, and liveness in <10s.
- Money-Trail Explorer: Tracks real-time transaction graph streams to catch 6 laundering patterns and trigger automated risk containment.
- AML STR Reporter: Generates bilingual (English/Vietnamese) Suspicious Transaction Reports to eliminate manual compliance backlogs.

Powered by the Alibaba Cloud AI Ecosystem:
- Qwen-VL Vision AI for eKYC deepfake inspection, document tampering & liveness analysis
- Qwen-Plus LLM via DashScope for automated bilingual regulatory STR report generation
- ApsaraDB for PostgreSQL & cloud-native infrastructure for real-time compliance persistence

Qoder served as our core AI-powered Spec-Driven Development engine to bring this vision to life. Qoder helped us:
- Break complex compliance requirements into executable specifications (`SPEC.md`)
- Parallelize multi-repo development using Quest & Expert Mode with autonomous AI subagents
- Rapidly iterate and debug real-time streaming pipelines across 11 repositories
- Verify generated code with automated unit and full-stack integration tests

Built on an enterprise-grade cloud-native architecture spanning 11 microservices, Apache Kafka event streaming, Alibaba Cloud AI, and modern Java/Python web services.

We cannot wait to present this to the judges and community!

How do you see multi-agent AI systems reshaping regulatory compliance in banking? We would love to hear your thoughts!

-- Team Little Boy's

@Qoder @Alibaba Cloud #QoderHackathon #QoderVietnam #AML #KYC #FinTech #AI #AlibabaCloud #Qwen
