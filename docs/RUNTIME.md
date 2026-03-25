# Runtime: Where and How Agents Live

You've seen the architecture (SOUL.md, AGENTS.md, memory/, skills/). Now the question is: **where does all this actually run?**

This document covers the operational side — hardware, platforms, models, costs, and security — so you can make an informed decision before you build.

---

## 1. Where Agents Live

An agent needs a place to run. Your options:

| Option | Cost/month | Uptime | Best for |
|--------|-----------|--------|----------|
| **Your laptop** | $0 | Only when open | Experimenting, development |
| **Dedicated home server** (Mac mini, Intel NUC, old laptop) | $5-15 (electricity) | 24/7 if you set it up | Personal agent, full control |
| **VPS** (Hetzner, DigitalOcean, Hostinger) | $5-25 | 24/7, managed | Always-on without hardware |
| **Raspberry Pi** | $3-5 (electricity) | 24/7 | Ultra-low cost, limited power |
| **Cloud VM** (AWS, GCP, Azure) | $15-100+ | 24/7, scalable | Teams, enterprise |

### The case for a dedicated server

A laptop works for experimenting. But a real agent system — one with cron jobs, heartbeat checks, and messaging integrations — needs something that runs when your laptop is closed.

A [Mac mini M4](https://www.apple.com/shop/buy-mac/mac-mini) (from $499) or a used Intel NUC ($100-200) on your desk is the simplest 24/7 setup. No monthly fees beyond electricity. No vendor dependency. You own it.

A VPS is the alternative if you don't want hardware at home. [Hetzner](https://www.hetzner.com/cloud/) (from €4.50/month), [DigitalOcean](https://www.digitalocean.com/pricing) ($6/month), or [Hostinger](https://www.hostinger.com/vps-hosting) ($5/month) give you a Linux server that's always on. As of early 2026, several providers offer one-click agent platform installs.

**Kubernetes and serverless are poor fits.** Agents are long-running and stateful, not ephemeral. A Kubernetes pod that gets rescheduled loses the agent's in-memory context. Serverless functions have timeout limits that conflict with agent tasks that take minutes.

---

## 2. What Keeps Them Running

An agent isn't just a model — it's a system. These are the components:

### Gateway
The gateway routes incoming messages (Telegram, Signal, Discord) to the right agent session, manages approvals, and enforces policies. Think of it as the receptionist.

Platforms like OpenClaw, LangChain's LangServe, or a custom Node.js/Python server fill this role. Without a gateway, your agent is a script you run manually.

### Cron Jobs & Scheduling
Agents do their best work when they run on a schedule:
- **Daily digest** (07:00): Summarize news, check email
- **Heartbeat** (every 2h): Verify system health, check for pending tasks
- **Weekly review** (Monday 09:00): Summarize the week, suggest improvements

On macOS: LaunchAgents. On Linux: crontab or systemd timers. On a VPS: same, or the platform handles it.

### Monitoring
An unmonitored agent is a liability. At minimum:
- **Watchdog:** Is the agent process alive?
- **Cost tracking:** How much API spend this month?
- **Error logs:** What failed and why?

---

## 3. Platform Landscape

You need software to orchestrate your agent. Here's the landscape as of March 2026:

| Platform | Type | Language | Best for | Complexity |
|----------|------|----------|----------|------------|
| **Claude Code** | CLI | TypeScript | Developers on their machine | Low |
| **OpenClaw** | Gateway | Node.js | Always-on personal agents | Medium |
| **LangGraph** | Framework | Python | Complex workflows, enterprises | High |
| **CrewAI** | Framework | Python | Multi-agent teams | Medium |
| **AutoGen** (Microsoft) | Framework | Python | Research, conversational agents | Medium |
| **Semantic Kernel** (Microsoft) | SDK | C#/Python | Enterprise .NET integration | High |
| **No framework** | DIY | Any | Simple setups, full control | Varies |

### How to choose

```
Just experimenting?
  → Claude Code (or any LLM CLI)

Want a personal agent that's always on?
  → OpenClaw or a simple gateway + cron

Building for a team or product?
  → LangGraph (most mature) or CrewAI (faster to start)

Enterprise with compliance requirements?
  → LangGraph or Semantic Kernel
```

**Key insight:** You don't need a framework to run an agent. This starter kit works with any platform — or none. The architecture (SOUL.md, AGENTS.md, memory/) is framework-agnostic by design.

Sources: [SparkCo Framework Comparison (2026)](https://sparkco.ai/blog/ai-agent-frameworks-compared-langchain-autogen-crewai-and-openclaw-in-2026), [DEV.to Agent Framework Guide (2026)](https://dev.to/linou518/the-2026-ai-agent-framework-decision-guide-langgraph-vs-crewai-vs-pydantic-ai-b2h)

---

## 4. Cloud APIs vs. Local Models

This is one of the most important decisions. It's not just about cost — it's about sovereignty, privacy, and capability.

### The Spectrum

| | Cloud API (Claude, GPT, Gemini) | Local Model (Ollama + Qwen, Llama) |
|---|---|---|
| **Quality** | Frontier (best available) | 70-85% of frontier |
| **Cost per request** | $0.01-0.80 per task | $0 (after hardware) |
| **Privacy** | Data leaves your network | Data stays on your machine |
| **Availability** | Depends on provider uptime | Depends on your hardware |
| **Speed** | Fast (but latency to API) | Fast locally, no network latency |
| **Context window** | 128K-1M tokens | 8K-128K (model dependent) |
| **Capability** | Complex reasoning, long analysis | Simple tasks, summarization, chat |

### What local models can actually do (March 2026)

Local AI has matured dramatically. Ollama reached [52 million monthly downloads](https://dev.to/pooyagolchian/local-ai-in-2026-running-production-llms-on-your-own-hardware-with-ollama-54d0) in Q1 2026. Key benchmarks (source: same article, MMLU evaluation):

| Model | Size | MMLU | Hardware needed | Tokens/sec |
|-------|------|------|----------------|------------|
| Qwen 2.5 32B | 32B | 83.2% | Mac M4 Pro (36GB+) | ~15 t/s |
| Qwen 3.5 7B | 7B | 76.8% | Any M-series Mac (16GB) | ~45 t/s |
| Llama 3.1 8B | 8B | 73.0% | 16GB RAM minimum | ~40 t/s |
| DeepSeek-R1 70B | 70B | 85.1% | M4 Max (128GB) | ~12 t/s |
| Phi-4 14B | 14B | 79.3% | 16GB RAM | ~25 t/s |

For comparison: GPT-4 scores ~86.4% MMLU ([OpenAI](https://openai.com/index/gpt-4/)), Claude Opus ~86% MMLU ([Anthropic](https://docs.anthropic.com/en/docs/about-claude/models)).

**The crossover point:** At ~1,000 requests/day, cloud APIs cost $30-45/month. A local setup on existing hardware costs effectively $0 in marginal terms. At 50,000+ daily requests, cloud API costs run $2,000+/month while local inference uses only electricity.

Sources: [DEV.to Local AI Benchmarks (2026)](https://dev.to/pooyagolchian/local-ai-in-2026-running-production-llms-on-your-own-hardware-with-ollama-54d0), [PremAI Self-Hosted Guide (2026)](https://blog.premai.io/self-hosted-llm-guide-setup-tools-cost-comparison-2026/)

### Sovereign AI: When data can't leave your network

Some use cases require that no data leaves your infrastructure:
- **Regulated industries** (healthcare, finance, legal)
- **Personal privacy** (journals, medical notes, financial data)
- **Corporate policy** (internal documents, strategy, IP)

Local models solve this completely. Your prompts and responses never touch an external server. The tradeoff: lower capability on complex reasoning tasks.

### The Hybrid Approach

Most production systems use both:
- **Local models** for routine tasks: summarization, classification, simple chat, embedding generation
- **Cloud APIs** for complex tasks: deep analysis, research synthesis, multi-step reasoning, code generation

This starter kit supports both — the agent routes to the right model based on the task. See `AGENTS.md` for model routing examples.

---

## 5. Security & Isolation

### Why your agent should NOT run on your main machine

An agent has access to:
- Your file system (read and write)
- Shell commands (if permitted)
- Network (web search, API calls)
- Potentially: email, calendar, messaging

If an agent is compromised (via prompt injection, malicious tool input, or a bug), it has the same access as the user account it runs under.

**Real risks:**
- An agent reads `~/.ssh/` and leaks your SSH keys
- An agent runs `rm -rf` on an important directory
- An agent sends an email you didn't authorize
- An agent stores sensitive data in a public Git repo

### Mitigation: Isolation layers

| Layer | What it does | How |
|-------|-------------|-----|
| **Dedicated user** | Separate OS account for the agent | `useradd agent` — agent can't access your files |
| **Restricted paths** | Block sensitive directories | Define in `TOOLS.md` (this kit does this) |
| **Approval gates** | Human confirms destructive actions | Gateway feature (OpenClaw, custom) |
| **Container** | Full filesystem isolation | Docker — agent sees only its own world |
| **Network rules** | Limit outbound connections | Firewall rules, no SSH access |

### Containers: The concept

Docker and similar tools create an isolated environment where the agent can only see and modify files inside the container. If the agent runs `rm -rf /`, it destroys only its own container — your host system is untouched.

You don't need to master Docker to understand the principle: **the less your agent can access, the less it can break.** Start with a dedicated user account and restricted paths (`TOOLS.md`). Add containers when you're ready.

---

## 6. What It Costs

### Hardware (one-time)

| Option | Cost | Amortized/month (36mo) |
|--------|------|----------------------|
| [Raspberry Pi 5](https://www.raspberrypi.com/products/raspberry-pi-5/) (8GB) | ~$80 | ~$2 |
| Used Intel NUC (16GB) | ~$150-250 | ~$5-7 |
| [Mac mini M4](https://www.apple.com/shop/buy-mac/mac-mini) (16GB) | ~$499 | ~$14 |
| [Mac mini M4 Pro](https://www.apple.com/shop/buy-mac/mac-mini) (48GB) | ~$1,599 | ~$44 |
| VPS (no hardware) | $0 | $5-25/mo |

### Running costs (monthly)

| Category | Range | Notes |
|----------|-------|-------|
| **Electricity** (home server) | $3-15 | Mac mini ~15W idle, ~60W load ([source](https://support.apple.com/en-us/111902)) |
| **VPS** (if no home server) | $5-25 | Hetzner, DigitalOcean, Hostinger |
| **Cloud API** (light use) | $5-20 | ~100-500 requests/day, mixed models |
| **Cloud API** (heavy use) | $30-100+ | 1000+ requests/day, Opus-heavy |
| **Cloud API** (automated cron) | $15-60 | Daily digests, reviews, heartbeats |

### The invisible costs

Things people forget to budget:
- **Cron jobs run silently.** A daily digest + weekly review + heartbeat checks can cost $15-45/month without you noticing. Set spending alerts.
- **Opus/GPT-4 is 5-15x more expensive** than Haiku/GPT-4o-mini. Route simple tasks to cheap models.
- **Context window usage scales cost.** An agent that reads 50 files per task uses 10x more tokens than one that reads 5.

Sources: [Alchemic Technology VPS Cost Breakdown (2026)](https://alchemictechnology.com/blog/posts/vps-cost-breakdown-self-hosted-ai.html), [Anthropic Pricing](https://docs.anthropic.com/en/docs/about-claude/models), [OpenAI Pricing](https://openai.com/pricing). As of March 2026.

---

## 7. Making the Case

If you need to explain to your boss, team, or yourself why a dedicated agent setup makes sense:

### The 30-second pitch

> "An AI agent that runs 24/7 costs $20-50/month (server + API). It handles research, monitoring, content drafting, and routine tasks while we sleep. The ROI is one employee-hour saved per day — at a fraction of the cost."

### The risk argument

> "Running agents on employee laptops means they stop when the laptop closes, have access to personal files, and can't be audited. A dedicated server with proper isolation is safer, more reliable, and auditable."

### The comparison

| | Employee | Agent (dedicated server) | Agent (laptop) |
|---|---|---|---|
| **Availability** | 8h/day | 24/7 | When laptop is open |
| **Cost/month** | $5,000+ | $20-50 | $0 (but unreliable) |
| **Audit trail** | Email, Slack | Git commits, logs | None |
| **Security** | Trained | Sandboxed | Same access as user |
| **Consistency** | Variable | Deterministic | Variable |

This is not about replacing people. It's about giving people a reliable async teammate that handles the routine so they can focus on the work that matters.

---

---

## Sources

All prices and benchmarks as of March 2026 — verify before making purchasing decisions.

| # | Source | Type |
|---|--------|------|
| 1 | [DEV.to — Local AI in 2026: Ollama Benchmarks](https://dev.to/pooyagolchian/local-ai-in-2026-running-production-llms-on-your-own-hardware-with-ollama-54d0) | Benchmarks, hardware data |
| 2 | [PremAI — Self-Hosted LLM Guide (2026)](https://blog.premai.io/self-hosted-llm-guide-setup-tools-cost-comparison-2026/) | Cost analysis, setup |
| 3 | [Alchemic Technology — VPS Cost Breakdown](https://alchemictechnology.com/blog/posts/vps-cost-breakdown-self-hosted-ai.html) | VPS pricing, free tier |
| 4 | [DEV.to — Agent Framework Decision Guide (2026)](https://dev.to/linou518/the-2026-ai-agent-framework-decision-guide-langgraph-vs-crewai-vs-pydantic-ai-b2h) | Framework comparison |
| 5 | [Anthropic — Claude Pricing](https://docs.anthropic.com/en/docs/about-claude/models) | API costs |
| 6 | [OpenAI — Pricing](https://openai.com/pricing) | API costs |
| 7 | [Apple — Mac mini](https://www.apple.com/shop/buy-mac/mac-mini) | Hardware pricing |
| 8 | [Apple — Mac mini Power Consumption](https://support.apple.com/en-us/111902) | Electricity estimates |
| 9 | [Raspberry Pi — Products](https://www.raspberrypi.com/products/raspberry-pi-5/) | Hardware pricing |
