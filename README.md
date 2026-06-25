<div align="center">

# GenAI Multi-Agentic Blog Generation

### An autonomous blog generation system powered by a self-correcting multi-agent pipeline

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![LangGraph](https://img.shields.io/badge/LangGraph-Agentic%20Pipelines-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)](https://langchain-ai.github.io/langgraph/)
[![LangChain](https://img.shields.io/badge/LangChain-Tools%20%26%20LLMs-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)](https://langchain.com)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-412991?style=for-the-badge&logo=openai&logoColor=white)](https://openai.com)
[![Tavily](https://img.shields.io/badge/Tavily-Web%20Search-FF6B35?style=for-the-badge)](https://tavily.com)

</div>

---

## What is this?

Most AI writing tools generate content in a **single pass** — one prompt, one output. This system works differently.

**GenAI Multi-Agentic Blog** uses a team of specialized AI agents that collaborate, just like a real editorial team:

-  A **Research Agent** searches the live web for relevant, up-to-date information
-  A **Writing Agent** drafts a full, structured blog post from the research
-  A **Validator Agent** reviews the draft — and if it is not good enough, sends it back for revision **automatically**

No human in the loop. No single-shot prompting. Just agents doing their jobs.

---

## System Architecture+-----------------------------+
                    ¦           START              ¦
                    +-----------------------------+
                                   ¦
               +-------------------+-------------------+
               ¦ domain == ainews  ¦                   ¦ domain == tutorial
               ?                  ¦                   ?
      +-----------------+         ¦        +----------------------+
      ¦   News Agent    ¦         ¦        ¦   Tutorial Agent     ¦
      ¦  - Web search   ¦         ¦        ¦  - Concept research  ¦
      ¦  - Summarize    ¦         ¦        ¦  - Step-by-step doc  ¦
      +-----------------+         ¦        +----------------------+
               ¦                  ¦                   ¦
               +------------------+-------------------+
                                  ?
                      +-----------------------+
                      ¦    Validator Agent     ¦
                      ¦  - Quality check       ¦
                      ¦  - Structure review    ¦
                      ¦  - Factual grounding   ¦
                      +-----------------------+
                                  ¦
                +------------------------------------+
                ¦ APPROVED                  REJECTED  ¦
                ?                                     ?
              END ?              Loop back with feedback ??Built on **LangGraph StateGraph** with conditional routing and **InMemorySaver** checkpointing for state persistence across revision loops.

---

## Key Features

| Feature | Description |
|---------|-------------|
|  **Self-Correcting Loop** | Validator rejects low-quality drafts and routes them back automatically |
|  **Live Web Research** | Agents search the web in real-time via Tavily and Guardian APIs |
|  **Conditional Routing** | Graph dynamically picks the right agent based on content domain |
|  **Modular Agents** | Each agent has its own prompts, tools, and logic — easy to extend |
|  **Stateful Pipeline** | LangGraph checkpointing persists state across the full revision cycle |
|  **Web Frontend** | Generated blogs are published to a clean, browsable web interface |

---

## ??? Tech Stack

| Layer | Technology |
|-------|------------|
| **Agent Orchestration** | LangGraph, LangChain |
| **LLM** | OpenAI GPT-4 (configurable) |
| **Web Search** | Tavily Search API, The Guardian API |
| **State Management** | LangGraph InMemorySaver |
| **Configuration** | Pydantic Settings |
| **Frontend** | HTML, CSS, JavaScript |
| **Package Manager** | uv |

---

##  Project Structureblogboard/

+-- agents/

¦   +-- news_agent/           # Researches live web and writes AI news posts

¦   ¦   +-- agent.py          # Agent logic with 2-step: research then generate

¦   ¦   +-- prompts.py        # News-specific prompt templates

¦   +-- tutorial_agent/       # Writes structured technical tutorials

¦   +-- validator_agent/      # Enforces quality, routes rejections with feedback

¦

+-- graph/

¦   +-- graph.py              # LangGraph StateGraph — routing and compilation

¦   +-- state.py              # Shared BlogState schema across all agents

¦

+-- services/

¦   +-- llm.py                # LLM service abstraction layer

¦   +-- prompt_manager.py     # Centralized prompt loading and management

¦   +-- storage.py            # Blog post persistence to disk

¦

+-- tools/

¦   +-- tavily_search.py      # Tavily real-time web search tool

¦   +-- guardian_search.py    # Guardian News API search tool

¦

+-- web/                      # Static frontend blog viewer

¦   +-- blogs/                # Generated markdown blog posts by category

¦   +-- js / css / html       # Frontend assets

¦

+-- run.py                    # Pipeline entry point

---

##  Getting Started

### Prerequisites
- Python 3.11+
- API keys for OpenAI, Tavily, and Guardian

### 1. Clone the repository
```bash
git clone https://github.com/Thorvi01/GenAI-Multi-Agentic-Blog.git
cd GenAI-Multi-Agentic-Blog
```

### 2. Configure environment
```bash
cp .env.example .env
```
Add your keys to `.env`:
```env
OPENAI_API_KEY=your_key_here
TAVILY_API_KEY=your_key_here
GUARDIAN_API_KEY=your_key_here
```

### 3. Install dependencies
```bash
pip install uv
uv sync
```

### 4. Run the pipeline
```bash
python -m blogboard.run
```

Generated blogs appear in `blogboard/web/blogs/` and are viewable in the frontend.

---

##  Design Decisions

**Why LangGraph over a simple LLM chain?**
> LangGraph supports stateful, cyclic execution — essential for the validator revision loop. A linear chain cannot loop back; LangGraph conditional edges make this natural.

**Why separate agents per domain?**
> News and tutorials require fundamentally different research strategies, tone, and structure. Separate agents keep prompts focused and output quality consistently high.

**Why a validator agent instead of just better prompts?**
> Single-pass generation produces inconsistent quality. A dedicated validator with explicit rubrics enforces structure, tone, and factual grounding on every run — acting as a safety net regardless of input variability.

---

##  Contributing

Contributions are welcome! Open an issue or submit a pull request to:
- Add new agent types (e.g. summarizer, SEO optimizer)
- Support additional LLM providers (Anthropic, Groq, Mistral)
- Improve the web frontend

---

<div align="center">
Built with  LangGraph ·  LangChain ·  Tavily
</div>
