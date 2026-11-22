# Project Brief: CSE Research Assistant

**Status:** Draft v1 | **Type:** Final-year software engineering project

## 1. Problem

Retail investors in Sri Lanka lack tools to quickly analyze Colombo Stock Exchange (CSE) companies. Market data, announcements and annual reports are scattered across sources, and there is no easy way to ask "what is going on with this company?" and get an answer backed by evidence. Global tools focus on US and European markets and do not cover the CSE.

## 2. Users

| Priority | User | Need |
|---|---|---|
| Primary | Retail investors (beginner to intermediate) | Quick, understandable summaries of a company's recent behaviour |
| Secondary | Finance and IT students | A transparent tool for learning and research on a local market |
| Tertiary | Independent analysts | Fast first-pass screening across many listed companies |

## 3. Core question

> **"What is unusual about company X this month, and why?"**

Related questions the system should also handle:

- How has company X's price and volume behaved compared with its sector and the ASPI?
- Which companies showed abnormal moves this week?
- What do recent announcements say about company X?

## 4. Solution overview

A multi-agent system that combines trained ML models with retrieval over company documents:

- **Data layer:** collects, cleans and stores CSE price, market and company data in PostgreSQL.
- **ML service:** time-series forecasting, anomaly detection, and sentiment analysis on announcements and news.
- **Research agent:** a LangGraph agent that calls the ML service and data as tools, retrieves relevant documents (RAG over pgvector), and writes a cited summary.
- **Interface:** a web dashboard with a chat view.

## 5. Scope

### In scope

- Daily price and volume data for CSE-listed companies
- Forecasting and anomaly detection models, trained and evaluated by me
- RAG over announcements and annual reports
- Agent that produces cited research summaries
- Evaluation suite, CI/CD and a deployed demo

### Out of scope

- No buy/sell or investment advice
- No live or real-time trading
- No real money, brokerage integration or portfolio management
- No intraday or tick-level modelling

All outputs are research and analysis only, and the interface will carry a clear disclaimer saying so.

## 6. Success metrics

Targets are initial and will be revised once baselines are measured. Each is checked on data the models never saw during training.

| Component | Metric | Target |
|---|---|---|
| Forecasting | Error (MAE/RMSE) vs. a naive "tomorrow equals today" baseline, on a held-out period | Beat the baseline by at least 10% |
| Forecasting | Directional accuracy on held-out data | Above 55% |
| Anomaly detection | Precision and recall on labelled events (e.g. announcement-driven spikes) | Precision at least 0.70, recall at least 0.60 |
| Sentiment | Accuracy / macro-F1 on a hand-labelled sample of announcements | Macro-F1 at least 0.70 |
| Agent grounding | Share of claims in an answer traceable to a cited source | At least 90% |
| Agent reliability | Task success on a fixed evaluation question set | At least 80% |
| System | Typical answer latency | Under 20 seconds |

## 7. Tech stack

| Layer | Choice |
|---|---|
| Frontend | Next.js, TypeScript, Tailwind |
| Backend | FastAPI (Python) |
| Agents | LangGraph, MCP for tool integration |
| LLM / RAG | Claude API, an open-source model via Ollama, pgvector |
| ML | scikit-learn, XGBoost, PyTorch, Hugging Face, MLflow |
| Data | PostgreSQL, Redis |
| DevOps | Docker, GitHub Actions, cloud deployment |
| Quality | pytest, ruff, agent evals, Langfuse or LangSmith |

## 8. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Limited historical depth or gaps in CSE data | Check coverage early; define a minimum usable history; document gaps |
| Markets are hard to predict, so models may barely beat baseline | Treat honest evaluation and clear reporting as the goal, not high accuracy |
| LLM hallucination in summaries | Force citations, check claims against sources, add refusal when evidence is missing |
| Data source terms or access changes | Respect source terms, cache raw data, keep collectors modular |
| Scope creep | Follow this brief; new ideas go on a backlog, not into the build |

## 9. Milestones (high level)

1. Foundation: repo, environment, database, CI
2. Data schema and ingestion pipeline
3. Exploratory analysis and baseline models
4. ML models: forecasting, anomaly detection, sentiment
5. RAG layer and research agent
6. Evaluation suite and guardrails
7. Frontend, deployment and demo
8. Write-up and viva preparation
