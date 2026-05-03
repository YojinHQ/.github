# Yojin

[![Website](https://img.shields.io/badge/yojin.ai-website-blue)](https://yojin.ai/)
[![X (Twitter)](https://img.shields.io/badge/@YojinHQ-black?logo=x)](https://x.com/YojinHQ)

A local-first AI agent that connects to your investment accounts, delivers personalized intelligence, monitors your portfolio 24/7, and executes trades — across every platform you use.

|                               |                                                                                                      |
|-------------------------------|------------------------------------------------------------------------------------------------------|
| **Unified portfolio view**    | All of your accounts in one place. Positions, P&L, and intelligence updated in real time.            |
| **Chat**                      | Tell Yojin what you want — analyze a stock, check your portfolio, place a trade.                     |
| **Personalized intelligence** | News, sentiment, technical analysis, and macro events based on your actual positions.                |
| **Explainable finance**       | Before every action, Yojin thinks, explores, reasons, tests, calculates, and asks for your approval. |

## Architecture

Yojin is a multi-agent system built around a central **Orchestrator** that coordinates specialized agents. Each agent has its own role, tool set, and allowed actions — but they share state through a common interoperability layer.

The **Orchestrator** is the entry point for every workflow — whether triggered by a user message, a scheduled digest, or a market event. It decides which agents to invoke, in what order or in parallel, and assembles their outputs into a coherent response or action.

Everything runs on your machine. Your data stays in local files — no database, no cloud sync, no containers.

### Agents

| Agent            | Role                                                                                                                                                                                                                                                                         |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Analyst**      | Ingests signals, runs technical analysis (SMA, RSI, BBANDS), extracts tickers from news. Maintains a self-evolving working memory — past analyses, recommendations, and their actual outcomes are stored and retrieved to inform every future decision. |
| **Strategist**   | Owns the Brain (persona, working memory, emotions). Runs bull/bear debate analysis. Defines strategy — asset allocation, rebalancing rules, entry/exit logic tailored to your goals.                                                                                         |
| **Risk Manager** | Analyzes exposure, concentration, correlation, drawdown. Monitors markets 24/7. Delivers alerts and daily portfolio digests.                                                                                                                                                 |
| **Trader**       | Executes trades on target platforms (Robinhood, Coinbase, IBKR, Schwab, Binance, and more).                                                                                                                                                                                  |

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                              Your Machine                                │
│                                                                          │
│  ┌───────────────┐   ┌─────────────────┐   ┌───────────────┐           │
│  │  Robinhood    │   │  AgentRuntime   │   │   Channels    │           │
│  │  Coinbase     │──▶│  Orchestrator   │──▶│  Web / MCP    │           │
│  │  IBKR/Schwab  │   │  (staged)      │   │  ACP / Slack  │           │
│  │  Binance/...  │   └───────┬────────┘   └───────────────┘           │
│  └───────────────┘           │                                          │
│                              ▼                                          │
│               ┌──────────────────────────────┐                          │
│               │  STAGE 0 — Research Analyst  │                          │
│               │  portfolio + signals + enrich │                         │
│               └──────────────┬───────────────┘                          │
│                              ▼                                          │
│               ┌──────────────────────────────┐                          │
│               │  STAGE 1 — parallel          │                          │
│               │  ┌────────────┐ ┌──────────┐ │                          │
│               │  │  Research  │ │   Risk   │ │                          │
│               │  │  Analyst   │ │ Manager  │ │                          │
│               │  └────────────┘ └──────────┘ │                          │
│               └──────────────┬───────────────┘                          │
│                              ▼                                          │
│               ┌──────────────────────────────┐                          │
│               │  STAGE 2 — Strategist        │                          │
│               │  (Brain + Memory + Emotion)  │                          │
│               │  synthesis → InsightReport   │                          │
│               └──────────────┬───────────────┘                          │
│                              ▼                                          │
│                    ┌───────────────────┐                                 │
│                    │      Jintel       │                                 │
│                    │ (intelligence     │                                 │
│                    │  layer)           │                                 │
│                    └────────┬──────────┘                                 │
│            ┌────────────────┼────────────────┐                          │
│            ▼                ▼                ▼                          │
│     ┌────────────┐  ┌────────────┐  ┌────────────┐                     │
│     │  News &    │  │ Market &   │  │  Custom    │                     │
│     │ Sentiment  │  │ Financials │  │  Sources   │                     │
│     │  Feeds     │  │   APIs     │  │ (Sheets,   │                     │
│     └────────────┘  └────────────┘  │  DBs, ...) │                     │
│                                     └────────────┘                     │
│                                                                          │
│  ┌───────────────────────────────────────────────────────────────────┐  │
│  │  Trust Layer: Vault │ Guard Pipeline │ PII │ Audit Log            │  │
│  └───────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────────┘
```

## Security & Privacy

Your credentials, positions, and account details are stored and processed on your computer — not on our servers, not in the cloud. Sensitive data is scrubbed before it reaches the AI model.

- **Credential Vault** — API keys and credentials are stored in a local encrypted vault. The vault never makes network requests. Credentials are injected at the transport layer and never appear in AI prompts.
- **Guard Pipeline** — Deterministic, code-based security rules with binary outcomes. The AI cannot persuade, interpret, or work around them. Every decision is written to a tamper-evident audit log.
- **PII Redaction** — Chat messages and portfolio snapshots are scrubbed of personally identifiable information before reaching any AI model. Responses are rehydrated before you see them.
- **Approval Gate** — Agents have read access to observe and analyze. Irreversible operations — executing a trade, adding a new connection — require your explicit approval.

## Quick Start

Yojin runs locally on your computer. [Download the desktop app](https://yojin.ai/) to get started.

On first launch, Yojin bootstraps itself: connects an LLM provider and generates a personalized Strategist persona based on your investment style. No manual config files needed.

## Where you can use Yojin

| Channel              | Status      |
|----------------------|-------------|
| Desktop app          | Available   |
| Claude Desktop / Cursor (MCP / ACP) | Available   |
| Slack                | Available   |
| Telegram             | Coming soon |

## Security

Report vulnerabilities to [security@yojin.ai](mailto:security@yojin.ai).
