<div align="center">

# 🔥 PromptForge

### The open-source Prompt Engineering Toolkit

**Score, optimise, generate, and manage prompts for every major AI model — from one clean, dependency-free engine.**

[![CI](https://github.com/promptforge/promptforge/actions/workflows/ci.yml/badge.svg)](https://github.com/promptforge/promptforge/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![Dependencies: 0](https://img.shields.io/badge/runtime%20deps-0-success.svg)](pyproject.toml)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Code of Conduct](https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg)](CODE_OF_CONDUCT.md)

[Quickstart](#-quickstart) · [Features](#-features) · [CLI](#-cli) · [Python API](#-python-api) · [Roadmap](#-roadmap) · [Contributing](CONTRIBUTING.md)

</div>

---

## ✨ Why PromptForge?

Most prompt repos are unstructured lists you copy-paste. **PromptForge is an
engine.** It *measures* prompt quality across ten dimensions, *rewrites* weak
prompts into structured best-practice form, *generates* prompts from a spec, and
ships a **fully-documented, schema-validated prompt library** — all as clean,
tested, **zero-dependency** Python you can embed anywhere.

```text
$ promptforge optimize "write a python function to parse csv"

Score: 65.5 -> 82.3  (delta +16.8)

Changes applied:
  - Added an explicit role: 'a senior software engineer'.
  - Added a Context section scaffold for background/goal.
  - Added a Constraints section with safe defaults.
```

> **Project status — honest version.** The **core engine, prompt library,
> schema, CLI, a local chat web app, and tests in this repository are complete
> and verified** (81 passing tests, runs on the Python standard library alone).
> The full marketing/blog website, the hosted FastAPI service, and growing the
> library to 1,000+ prompts are the **next milestones** — see the
> [Roadmap](#-roadmap). We document what *is*, not what we wish were true.

---

## 🖱️ One-click chat app

Want a ChatGPT-style experience? **Just double-click `start.bat`** (Windows) or
`start.command` (macOS) — or run `promptforge serve`. Your browser opens a clean,
dark-mode chat where you describe what you need and the assistant hands back a
ready-to-use, optimised prompt. It runs **100% locally**: no API keys, no signup,
nothing leaves your machine.

```bash
promptforge serve          # opens http://127.0.0.1:8765 in your browser
# or, with nothing installed:
python start.py
```

In the chat you can:

| Say this | And the assistant will… |
|----------|--------------------------|
| *"Write a prompt for a cold sales email to founders"* | **generate** a structured, optimised prompt |
| *"optimize: &lt;your prompt&gt;"* | **rewrite** it and show the before→after score |
| *"score: &lt;your prompt&gt;"* | **rate** it across 10 dimensions |
| *"find prompts about SEO"* | **search** the bundled library |
| *"make it shorter"* / *"target Claude"* | **refine** the last prompt |

---

## 🚀 Quickstart

**Requirements:** Python 3.10+ — and nothing else. No Node, no API keys, no
network access required for the core.

```bash
# Clone and install
git clone https://github.com/promptforge/promptforge.git
cd promptforge
pip install -e .

# Score a prompt
promptforge score "write something good about marketing"

# Optimise it
promptforge optimize "write something good about marketing"

# Generate one from a spec
promptforge generate --goal "Explain SQL CTEs to a junior dev" --length short
```

Prefer not to install? The engine runs straight from the source tree:

```bash
PYTHONPATH=src python -m promptforge.cli score "your prompt here"
```

---

## 🧩 Features

| # | Feature | Status | Module |
|---|---------|:------:|--------|
| 1 | **Prompt Library** — schema-validated, fully-documented prompts | ✅ Core (starter set) | [`prompts/`](prompts/) · [`library.py`](src/promptforge/library.py) |
| 2 | **Prompt Optimizer** — rewrites prompts, reports every change | ✅ Done | [`optimizer.py`](src/promptforge/analysis/optimizer.py) |
| 3 | **Scoring Engine** — 10 dimensions, 0–100 + grade | ✅ Done | [`scoring.py`](src/promptforge/analysis/scoring.py) |
| 4 | **Prompt Generator** — build a prompt from a spec | ✅ Done | [`generator.py`](src/promptforge/generator.py) |
| 5 | **AI Model Comparison** — context, cost, capabilities | ✅ Done | [`models.py`](src/promptforge/models.py) |
| 6 | **Prompt Templates** — JSON/MD/YAML/XML/CSV + tool-calling | ✅ Done | [`templates.py`](src/promptforge/templates.py) |
| 7 | **Prompt Engineering Guide** — beginner → expert | ✅ Done | [`docs/guide/`](docs/guide/) |
| 8 | **Prompt Search** — keyword + filters | ✅ Done | [`library.py`](src/promptforge/library.py) |
| 9 | **CLI** — scriptable, `--json` everywhere | ✅ Done | [`cli.py`](src/promptforge/cli.py) |
| 10 | **Local chat web app** — ChatGPT-style, double-click to launch | ✅ Done | [`server.py`](src/promptforge/server.py) · [`assistant.py`](src/promptforge/assistant.py) |
| 11 | **Marketing website / blog / hosted FastAPI service** | 🔜 Next milestone | — |
| 12 | **Marketplace, version control, semantic search, agents** | 🗺️ Planned | — |

### The 10 scoring dimensions

`clarity` · `specificity` · `reasoning` · `chain_of_thought` · `context` ·
`formatting` · `token_efficiency` · `safety` · `reliability` ·
`output_consistency`

Each returns a 0–100 sub-score with concrete *findings* and *suggestions*; the
overall score is a documented weighted average. **It's deterministic** — the same
prompt always scores the same, so you can use it as a quality gate in CI.

---

## 🤖 Supported models

ChatGPT (GPT) · Claude · Gemini · Grok · DeepSeek · Llama · Mistral · Qwen ·
Perplexity — with an extensible registry for future models.

```bash
$ promptforge models compare gpt claude deepseek

  metric                         gpt        claude      deepseek
  coding                           5             5             5
  reasoning                        5             5             5
  context_window              128000        200000        128000
  input_cost_per_mtok            2.5           3.0          0.27
```

> Model specs and prices are **indicative defaults** for comparison and are
> easy to override — always verify against the provider's docs before relying on
> them for billing.

---

## 🖥️ CLI

| Command | What it does |
|---------|--------------|
| `promptforge score <text>` | Score a prompt across 10 dimensions |
| `promptforge optimize <text>` | Rewrite it into a better, structured form |
| `promptforge generate --goal ...` | Generate a prompt from a spec |
| `promptforge search <query>` | Search the library (filters: category/tag/difficulty/model) |
| `promptforge models [list\|compare ...]` | Inspect and compare models |
| `promptforge template <name>` | Print a structured-output / tool-calling scaffold |
| `promptforge validate` | Validate every library prompt against the schema |
| `promptforge serve` | Launch the local ChatGPT-style chat app in your browser |

Add `--json` to any command for machine-readable output.

---

## 🐍 Python API

```python
import promptforge as pf

# 1. Score a prompt
result = pf.score_prompt("summarise this article")
print(result.overall, result.grade)         # e.g. 51.2 F
print(result.suggestions[:3])

# 2. Optimise it
opt = pf.optimize("summarise this article")
print(opt.delta)                             # positive improvement
print(opt.improved)                          # the rewritten prompt

# 3. Generate from a spec
gen = pf.generate(pf.GeneratorSpec(goal="Write a cold outreach email",
                                   industry="saas", tone="friendly"))
print(gen.prompt)

# 4. Search the library
lib = pf.PromptLibrary.load()
for hit in lib.search("sql", category="data-analysis"):
    print(hit.prompt.title)

# 5. Compare models
print(pf.compare(["gpt", "claude", "gemini"]))
```

---

## 🏗️ Architecture

```
promptforge/                 # core engine — pure stdlib, fully typed
├── schema.py                # Prompt data model + validation
├── models.py                # model registry + comparison
├── generator.py             # spec -> optimised prompt
├── templates.py             # multi-format renderers + scaffolds
├── library.py               # load / validate / search prompts
├── cli.py                   # argparse CLI (score/optimize/generate/serve/...)
├── assistant.py             # chat brain: routes a message to the engine
├── server.py                # stdlib HTTP server for the chat app
├── web/index.html           # ChatGPT-style chat UI (single file)
└── analysis/
    ├── text_stats.py        # deterministic text measurements
    ├── metrics.py           # per-dimension analyzers
    ├── scoring.py           # weighted 100-point scoring engine
    └── optimizer.py         # structured prompt rewriter

start.bat / start.command / start.py   # one-click launchers for the chat app

prompts/
├── schema/prompt.schema.json
└── data/<category>/*.json   # the library
```

Design principles: **zero runtime dependencies**, **deterministic & testable**,
**provider-agnostic**, and a **clean separation** between the engine, the data,
and the interfaces (CLI today; web/API next). See
[`docs/architecture.md`](docs/architecture.md).

---

## 🗺️ Roadmap

- [x] Core engine: scoring, optimizer, generator, templates, model registry
- [x] Schema-validated prompt library + keyword search + CLI
- [x] Full test suite + CI + contributor tooling
- [x] **Local ChatGPT-style chat app** with one-click launchers
- [ ] Marketing website + blog (Next.js): landing page, library browser, **AI Playground**
- [ ] Hosted FastAPI backend exposing the engine over HTTP
- [ ] Grow the library to **1,000+** curated prompts (community-driven)
- [ ] Semantic (embedding) search
- [ ] Prompt version control with a visual diff viewer
- [ ] Prompt marketplace (ratings, likes, collections)
- [ ] AI agents (research, coding, marketing, SEO, …)

Track progress and details in [`CHANGELOG.md`](CHANGELOG.md) and the
[Discussions](https://github.com/promptforge/promptforge/discussions).

---

## 🤝 Contributing

We'd love your help — especially **new prompts**, which take five minutes to add.
See the [Contributing Guide](CONTRIBUTING.md) and our
[Prompt Engineering Guide](docs/guide/README.md) for the quality bar. Every PR
runs the test suite and validates the prompt library automatically.

---

## 📜 License

[MIT](LICENSE) — free for personal and commercial use. Built by the community,
for the community.

<div align="center">
<sub>If PromptForge helps you, please ⭐ the repo — it helps others find it.</sub>
</div>
