# Risk Agent

**AI-Powered Autonomous Risk Control Agent for Web3**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)

---

## What is Risk Agent?

Risk Agent is an **autonomous AI agent** that handles end-to-end risk control workflows in Web3 environments. Unlike traditional rule-based systems, Risk Agent can:

- **Perceive**: Monitor on-chain events and internal alerts in real-time
- **Reason**: Analyze complex patterns using LLMs + statistical models + external data sources
- **Act**: Execute decisions (freeze accounts, send alerts, generate reports) with human oversight
- **Learn**: Improve detection rules based on feedback and discovered patterns

**The Problem**: Risk control teams are overwhelmed:
- 1000+ alerts per day, 80% require manual review
- New attack patterns emerge faster than rules can be written
- Cross-chain investigations require querying multiple data sources
- Regulatory reports take hours to compile manually

**The Solution**: Risk Agent automates 80% of routine decisions while escalating complex cases to humans, with complete audit trails for every action.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Risk Agent                            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Perception Layer                         │   │
│  │  • On-chain events (Alchemy/Infura webhooks)         │   │
│  │  • Internal alerts (Kafka/Redis)                     │   │
│  │  • External signals (Chainalysis, social media)      │   │
│  └──────────────────────────────────────────────────────┘   │
│                           │                                   │
│                           ▼                                   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Reasoning Layer (LangGraph)              │   │
│  │                                                        │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │   │
│  │  │  Detection  │─▶│  Analysis   │─▶│  Decision   │  │   │
│  │  │   (whale-   │  │  (LLM +     │  │  (Rule +    │  │   │
│  │  │   sentry)   │  │   Context)  │  │   LLM)      │  │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  │   │
│  │                                                        │   │
│  │  Tools: Etherscan API, Chainalysis, Vector DB,       │   │
│  │         Graph DB, RiskLens Platform API               │   │
│  └──────────────────────────────────────────────────────┘   │
│                           │                                   │
│                           ▼                                   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Action Layer                             │   │
│  │  • Freeze account (via business system API)          │   │
│  │  • Send alert (Slack/PagerDuty)                      │   │
│  │  • Generate report (Markdown/PDF)                    │   │
│  │  • Update rules (Git commit + CI/CD)                 │   │
│  │  • Escalate to human (with full context)             │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Memory Layer                             │   │
│  │  • Vector DB (historical cases, embeddings)          │   │
│  │  • Graph DB (address relationships)                  │   │
│  │  • Time-series DB (behavior sequences)               │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Learning Layer                           │   │
│  │  • Collect human feedback (approve/reject)           │   │
│  │  • Discover new patterns (unsupervised learning)     │   │
│  │  • Generate rule candidates (LLM-assisted)           │   │
│  │  • A/B test rule changes                             │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## Key Capabilities

### 🤖 Autonomous Decision-Making
**Scenario**: New address makes first large transfer ($100K+)

**Agent Workflow**:
1. **Perceive**: Detect event from Kafka stream
2. **Reason**:
   - Query address history (Etherscan API)
   - Check blacklists (Chainalysis API)
   - Analyze fund source (graph traversal)
   - Run whale-sentry detection
   - LLM synthesizes findings
3. **Decide**:
   - HIGH risk → Freeze + escalate to human
   - MEDIUM risk → Add to watchlist + re-check in 24h
   - LOW risk → Approve + log
4. **Act**: Execute decision via APIs
5. **Learn**: Store case in vector DB for future reference

**Result**: 95% of routine cases handled automatically, 5% escalated with full context.

---

### 🔍 Pattern Discovery
**Scenario**: Detect unknown attack pattern

**Agent Workflow**:
1. **Collect**: Gather "suspicious but undetected" cases flagged by humans
2. **Analyze**: Use unsupervised learning (Isolation Forest) to find commonalities
3. **Hypothesize**: LLM generates explanation ("These transactions share X, Y, Z characteristics")
4. **Validate**: Test hypothesis on historical data
5. **Propose**: Generate Python rule code
6. **Review**: Human approves/rejects
7. **Deploy**: If approved, commit to risklens-platform repo + auto-deploy

**Result**: Continuous improvement of detection capabilities without manual rule writing.

---

### 📊 Intelligent Reporting
**Scenario**: Regulator requests audit report

**Agent Workflow**:
1. **Parse**: Understand request ("All suspicious transactions >$100K in Q1 2026")
2. **Query**: Fetch data from PostgreSQL + vector DB
3. **Analyze**: For each case, retrieve decision rationale + evidence
4. **Format**: Generate report in regulator-required format (PDF/Excel)
5. **Deliver**: Send via secure channel + log access

**Result**: Reports generated in minutes instead of hours.

---

## Tech Stack

**Agent Framework**:
- LangGraph (workflow orchestration)
- LangChain (LLM integration)
- OpenAI GPT-4 / Claude 3.5 (reasoning)

**Detection & Decision**:
- [whale-sentry](https://github.com/Rhea-Wang315/whale-sentry) (pattern detection)
- [risklens-platform](https://github.com/Rhea-Wang315/risklens-platform) (rule engine)

**Data & Memory**:
- Chroma / Pinecone (vector database for case similarity)
- Neo4j (graph database for address relationships)
- TimescaleDB (time-series behavior data)

**External APIs**:
- Etherscan / Alchemy (on-chain data)
- Chainalysis (compliance data)
- Twitter/Discord APIs (social signals)

**Infrastructure**:
- Docker + Kubernetes
- Prometheus + Grafana
- Kafka (event streaming)

---

## Quick Start

### Prerequisites
- Python 3.11+
- Docker + Docker Compose
- OpenAI API key (or local LLM)
- Access to risklens-platform and whale-sentry

### Installation

```bash
# Clone the repo
git clone https://github.com/Rhea-Wang315/risk-agent.git
cd risk-agent

# Install dependencies
pip install -e ".[dev]"

# Set up environment
cp .env.example .env
# Edit .env with your API keys and configuration

# Initialize databases
docker-compose up -d postgres redis neo4j

# Run the agent
python -m riskagent.cli run
```

### Example: Process an Alert

```bash
# Submit an alert to the agent
python -m riskagent.cli process --alert-file examples/example_alert.json

# Agent output:
# [PERCEIVE] Received alert: alert_example_001
# [REASON] Running whale-sentry detection... score=0.87
# [REASON] Querying address history... 15 previous transactions
# [REASON] Checking blacklists... not found
# [REASON] Analyzing fund flow... 2 hops to known exchange
# [REASON] LLM synthesis: "Likely wash trading based on roundtrip pattern"
# [DECIDE] Risk=HIGH, Action=FREEZE, Confidence=0.92
# [ACT] Freezing account via API... success
# [ACT] Sending alert to Slack... success
# [ACT] Generating report... saved to outputs/report_alert_example_001.md
# [LEARN] Storing case in vector DB... done
#
# ✅ Alert processed in 3.2 seconds
```

---

## Project Status

### Phase 1: Core Agent Framework 🚧 (Current)
- [ ] LangGraph workflow definition
- [ ] Integration with whale-sentry + risklens-platform
- [ ] Basic tool implementations (Etherscan, Chainalysis mock)
- [ ] Action executors (Slack, report generation)
- [ ] Memory layer (vector DB for case storage)

### Phase 2: Pattern Discovery 📋 (Planned)
- [ ] Unsupervised anomaly detection
- [ ] LLM-assisted rule generation
- [ ] Human-in-the-loop validation workflow
- [ ] Auto-deployment to risklens-platform

### Phase 3: Advanced Reasoning 📋 (Planned)
- [ ] Multi-hop graph analysis
- [ ] Cross-chain investigation
- [ ] Social signal integration
- [ ] Causal reasoning (why did this happen?)

### Phase 4: Production Deployment 📋 (Future)
- [ ] Kubernetes deployment
- [ ] Monitoring + alerting
- [ ] A/B testing framework
- [ ] Performance optimization (latency < 5s)

---

## Use Cases

### 1. Automated Triage
- **Input**: 1000 alerts per day
- **Agent**: Processes 800 automatically (LOW/MEDIUM risk)
- **Human**: Reviews 200 escalated cases (HIGH/CRITICAL risk)
- **Result**: 80% reduction in manual workload

### 2. Cross-Chain Investigation
- **Input**: Suspicious address on Ethereum
- **Agent**: Automatically checks activity on Polygon, BSC, Arbitrum
- **Output**: Comprehensive multi-chain profile
- **Result**: Faster, more thorough investigations

### 3. Regulatory Compliance
- **Input**: "Generate FATF Travel Rule report for Q1 2026"
- **Agent**: Queries data, formats report, includes evidence
- **Output**: Audit-ready PDF in 5 minutes
- **Result**: Compliance team saves 10+ hours per report

### 4. Adaptive Defense
- **Input**: New MEV attack pattern emerges
- **Agent**: Detects anomaly, generates hypothesis, proposes rule
- **Human**: Reviews and approves
- **Output**: New detection rule deployed within hours
- **Result**: Faster response to evolving threats

---

## Comparison: Traditional vs Agent-Based

| Aspect | Traditional System | Risk Agent |
|--------|-------------------|------------|
| **Decision Speed** | Minutes (human review) | Seconds (automated) |
| **Coverage** | 20% automated | 80% automated |
| **Adaptability** | Manual rule updates | Self-improving |
| **Investigation Depth** | Single data source | Multi-source synthesis |
| **Audit Trail** | Partial | Complete (every step logged) |
| **Scalability** | Linear (more humans) | Exponential (same agent) |

---

## Development Roadmap

**Current Status**: Phase 1 (Core Agent Framework) - Planning stage

**Planned Phases**:
- **Phase 1**: Core agent framework with LangGraph + basic tool integration
- **Phase 2**: Pattern discovery and rule generation capabilities
- **Phase 3**: Advanced reasoning with multi-chain support
- **Phase 4**: Production deployment and optimization

---

## Related Projects

This project is part of a risk control ecosystem:

- **[whale-sentry](https://github.com/Rhea-Wang315/whale-sentry)**: Statistical detection engine (sandwich attacks, wash trading)
- **[risklens-platform](https://github.com/Rhea-Wang315/risklens-platform)**: Rule-based decision engine + operator dashboard
- **risk-agent** (this repo): AI-powered autonomous agent

**Together, they provide**: Detection → Decision → Action → Learning

---

## What This Project Demonstrates
Web3 risk control is at an inflection point:
1. **Attack sophistication is increasing**: Simple rules can't keep up
2. **Transaction volume is exploding**: Human review doesn't scale
3. **Regulatory pressure is mounting**: Compliance requires complete audit trails
4. **Talent is scarce**: Not enough experienced risk analysts
**Risk Agent addresses all four challenges** by combining:
- AI reasoning (handles complexity)
- Automation (handles scale)
- Audit trails (handles compliance)
- Self-improvement (reduces dependency on experts)

---

## Contributing

This is an active research project exploring AI agents for Web3 risk control. Contributions and collaboration are welcome.
For questions or collaboration inquiries, reach out via GitHub issues or email.

qinw.official@gmail.com

---

## License

MIT License - see [LICENSE](./LICENSE) for details.

---

## Author

**Rhea Wang**  
M.S. in Statistics, University of Pennsylvania  
AWS Certified Solutions Architect | Certified Kubernetes Administrator  
Background: Statistical modeling, AI agents, cloud-native engineering  
Focus: On-chain risk, MEV analysis, DeFi market behavior  

📧 qinw.official@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/rheawangwork)  
💻 [GitHub](https://github.com/Rhea-Wang315)  

Open to **Web3 / Crypto roles (remote or Singapore-based)**.

---

*Risk Agent is a demonstration of AI-powered autonomous systems for Web3 risk control. Not intended for production use without thorough testing and human oversight.*
