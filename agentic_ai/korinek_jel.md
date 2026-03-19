# Korinek (2025): AI Agents for Economic Research

**Citation:** Korinek, Anton. 2025. "AI Agents for Economic Research: August 2025 Update to 'Generative AI for Economic Research: Use Cases and Implications for Economists,' published in the *Journal of Economic Literature* 61(4)."

**Full text:** [AEA website](https://www.aeaweb.org/content/file?id=23290) | **Updates and newsletter:** [genaiforecon.substack.com](https://genaiforecon.substack.com)

---

## Overview

This paper is the August 2025 update to Korinek's landmark 2023 *Journal of Economic Literature* article on generative AI for economists. Where the original article introduced large language models (LLMs) and their use cases for research, this update documents the emergence of a new paradigm: **AI agents** — systems that go beyond responding to prompts to autonomously planning and executing multi-step research tasks.

The paper combines conceptual frameworks with working code examples, making it both a reference for understanding the technology and a hands-on guide for economists who want to build or use agents in their own work.

---

## Three Paradigms of AI Systems

A central contribution of the paper is a taxonomy of AI systems organized into three generations, each building on the last:

### 1. Traditional LLMs (available since November 2022)
Standard chatbots like early ChatGPT — fast, fluent, and useful for language tasks such as writing, editing, and summarization. They operate through single-pass text generation (what Kahneman called "System 1" thinking): no ability to reflect, revise, or access live information. Particularly useful for economists on writing and ideation tasks; limited for coding, math, and data analysis.

### 2. Reasoning Models (introduced September 2024)
Models trained via reinforcement learning to simulate deliberate, step-by-step "System 2" thinking — constructing chains of thought and tree searches before responding. This makes them powerful for complex mathematical derivations, algorithm development, and formal proofs. Korinek reports that by mid-2025, leading reasoning models (GPT-5, Grok-4, Gemini 2.5 Pro, Claude Opus 4.1) were approaching or exceeding PhD-level performance on the GPQA benchmark and had achieved gold-medal performance at the International Mathematical Olympiad.

### 3. Agentic Chatbots (introduced December 2024)
Systems that synthesize fluent language, reasoning, and the ability to autonomously call external tools — web search, code execution, file access, API calls. Unlike the prior two paradigms, agents don't just respond to questions; they actively pursue goals. For economists, this enables end-to-end workflows: download data, clean it, run regressions, produce figures, synthesize results — all from a single natural language instruction.

Korinek provides a useful comparison table (reproduced from the paper) of how each paradigm performs across seven research categories:

| Research Category | Traditional LLMs | Reasoning Models | Agentic Chatbots |
|---|---|---|---|
| Ideation & Feedback | Good for brainstorming | **Best for structured feedback** | Scans literature for novelty |
| Writing | **Excellent for drafting** | Ensures logical flow | Incorporates live web information |
| Background Research | No live web access | Synthesizes provided texts | **Best for literature reviews** |
| Coding | Basic snippets | **Excellent for complex algorithms** | Executes code, tests hypotheses |
| Data Analysis | Cannot execute code | Suggests approaches | **Best for end-to-end analysis** |
| Math | Unreliable | **Solves multi-step problems** | Leverages external compute |
| Promoting Research | Drafts content | Tailors for audiences | **Automates multi-platform posts** |

---

## Key Applications for Economists

### Deep Research Agents
The most powerful near-term application. Deep Research systems (available from Google Gemini, OpenAI, Claude, Perplexity, and Grok) use a multi-agent architecture: a lead orchestrator agent decomposes a research question into subtasks, spawns specialized subagents to search in parallel, and synthesizes their findings into a comprehensive report with citations — in minutes rather than weeks.

Korinek notes important caveats: these systems compile existing knowledge rather than generating novel insights; they struggle to identify the most important papers in emerging literatures; and they can exhibit overconfidence in their conclusions. They work best for well-established topics with abundant existing literature. His recommendation: use them to generate background, then verify with your own judgment.

### Coding Agents (Vibe Coding)
Coding agents — Claude Code (Anthropic, February 2025), Codex CLI (OpenAI, April 2025), Gemini CLI (Google, June 2025) — allow researchers to create software through natural language instructions rather than writing code directly. The paper demonstrates building a complete OLS regression tool (file upload, variable selection, regression table, scatter plot with fitted line) in under 10 minutes through iterative prompting with Claude Code.

This "vibe coding" paradigm democratizes computational work: researchers who previously lacked programming skills can now build data pipelines, implement estimation procedures, and produce interactive tools. However, Korinek cautions that these agents may generate syntactically correct but logically flawed code, particularly when deep domain expertise is required.

### Building Your Own Agents
Section 4 of the paper walks through how to build research agents from scratch using Python and **LangGraph** (a framework for constructing agents as directed state graphs rather than linear scripts). Two worked examples with complete code are provided:

1. **FRED Data Retrieval Agent** — a simple agent that takes a natural language question about the US economy, identifies the appropriate FRED data series, fetches the data, and returns a plain-language answer. Implements the ReAct (Reason-Act-Observe) framework explicitly.

2. **Deep Research Agent** — a more sophisticated multi-agent system that decomposes a research question into subtasks, runs parallel web searches via the Tavily API, analyzes results from multiple angles, and synthesizes a comprehensive report. Built with LangGraph's `StateGraph` and Python's `ThreadPoolExecutor` for parallelism. Cost: approximately $0.01 per report.

Both examples are fully vibe-coded — themselves written using AI coding assistants, illustrating a recursive truth: AI agents can build AI agents.

---

## The AI Landscape as of Mid-2025

The paper surveys the competitive landscape of AI labs and models as of August 2025:

**Proprietary leaders:** Google Gemini 2.5 Pro (2M token context window), OpenAI GPT-5 (smoothest agentic experience), Anthropic Claude Opus 4.1 (coding and nuanced writing), xAI Grok-4 (strong reasoning, real-time X integration)

**Open-source options:** DeepSeek R1, Alibaba Qwen3, Moonshot Kimi-K2 (GPT-4-class performance at ~1/100th the price of leading proprietary models), Meta LLaMA series

**Pricing:** Premium subscriptions from leading labs now reach $200–$300/month, a tenfold increase from a year prior. Korinek raises access equity concerns: cutting-edge AI tools may become increasingly stratified by ability to pay.

**Strategic advice for researchers:** Maintain flexibility across providers rather than committing to a single ecosystem. For intermittent needs, pay-per-use API access may be more economical than multiple monthly subscriptions.

---

## Open Protocols: MCP and A2A

The paper covers two emerging standards that are reshaping how agents connect to data and to each other:

**Model Context Protocol (MCP)** — introduced by Anthropic in November 2024 and now adopted industry-wide. Provides a universal standard for connecting AI agents to external data sources and tools. Analogous to USB-C: instead of custom integrations for each data source, researchers and institutions can build a single MCP server that any AI agent can connect to. As of mid-2025, nearly 10,000 MCP servers were publicly available, including wrappers for FRED and IMF data.

**Agent2Agent Protocol (A2A)** — launched by Google in April 2025 and transferred to the Linux Foundation. Enables agents from different systems to communicate, delegate tasks, and coordinate. An "Agent Card" specifies each agent's capabilities, inputs, and outputs, allowing orchestrators to discover and deploy specialized agents (e.g., a panel regression agent, a data cleaning agent) without knowing their implementation details.

---

## Limitations and Risks

Korinek gives significant attention to current limitations, which are directly relevant to research integrity:

- **Hallucinations** — confident generation of incorrect content, including fabricated citations and statistics
- **Error propagation** — in multi-step agentic workflows, an early mistake can compound silently through downstream steps
- **Prompt sensitivity** — small changes in how you phrase a query can produce dramatically different outputs, creating reproducibility challenges
- **Shallow economic reasoning** — current agents sometimes misapply theoretical frameworks and reproduce common misconceptions from training data rather than applying rigorous economic logic
- **Prompt injection** — malicious instructions hidden in web content or documents can hijack agent behavior

**Recommended approach:** Treat AI agents like a team of research assistants — talented and fast, but requiring planning, oversight during execution, and careful vetting of final outputs. Remember that the mistakes LLMs make differ from human mistakes: they hallucinate with supreme confidence rather than hedging uncertainty.

---

## Broader Implications

The paper closes with a forward-looking discussion of what increasingly autonomous research agents mean for the economics profession. Several points stand out:

- **Democratization** — technical barriers that once excluded researchers without programming skills from computational work are dissolving. Junior researchers can accomplish what once required teams.
- **Competitive dynamics** — those who best learn to deploy sophisticated agents may benefit far more than casual users, potentially widening rather than closing capability gaps.
- **The role of economists** — as research generation becomes more automated, the economist's irreplaceable contributions shift toward defining research questions, evaluating tradeoffs, and ensuring that analysis serves human welfare.
- **Academic integrity** — as AI agents enter peer review and research workflows, new forms of strategic manipulation (e.g., prompt injection attacks on AI reviewers) become possible, raising new governance questions.

The paper's closing analogy: the competition period that Kasparov experienced against chess computers in the 1990s may now be beginning for economic research. Unlike chess, the stakes extend well beyond academic prestige.

---

## Related Resources from Korinek

- **Original JEL paper (December 2023):** Introduction to LLMs for economists and longer-term implications — [AEA](https://www.aeaweb.org/articles?id=10.1257/jel.20231736)
- **December 2024 update:** LLMs learning to reason and collaborate — covers dozens of use cases
- **August 2025 update:** AI agents (summarized above)
- **Newsletter:** [genaiforecon.substack.com](https://genaiforecon.substack.com) — approximately semi-annual updates as the technology evolves
- **Code repository:** Working Python implementations of all agents in the paper available at [GenAI4Econ.org](https://www.GenAI4Econ.org)
