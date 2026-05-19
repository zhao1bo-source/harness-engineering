# Harness Engineering Notes

**Practical writing on Context Engineering, Agent Architecture, and Human-AI Collaboration Systems** — from real-world experiments building AI-native products and teams.

> *"The model is public infrastructure. Your context is the moat."*

---

## What is Harness Engineering?

A model is only as good as the system around it.

**Harness Engineering** is the discipline of designing everything that sits between a frontier model and a real outcome: context pipelines, tool orchestration, memory architecture, agent loops, skill systems, organizational workflows, and the human-AI collaboration patterns that make it all work.

Most teams obsess over which model to use. The teams that actually win obsess over their harness.

This repository collects my ongoing thinking and experiments at the intersection of:
- **Context Engineering** — how to make implicit knowledge explicit, testable, and evolvable
- **Agent Architecture** — how to design systems where humans and agents form effective working units
- **Organizational Harness** — how to restructure teams around agents as the unit of work, not people

---

## Articles


| Title | Core Idea | Read |
|-------|-----------|------|
| It's Not the Model. It's the Context. | Context failures are silent. Teams blame the model when they should debug the context. Introduces the Context Maturity Ladder (5 levels) and the case for eval-driven context development. | [articles/en/context-not-model.md](https://github.com/zhao1bo-source/harness-engineering/blob/main/articles/en/context-not-model.md) |
| AI Coding's Harness Engineering Guide | *Coming soon* | — |
| Organizational Practice and Reflections by Harness | *Coming soon* | — |

---

## Key Concepts

### The Context Maturity Ladder

Most teams are stuck at Level 1–2. The gap to Level 3+ is where real leverage lives.

```
Level 1  Ad-hoc prompts      No accumulation. Results vary per session.
Level 2  Rule files          AGENTS.md, auto-injected. Most teams are here. Cowboy coding.
Level 3  Structured skills   Rules packaged as executable skill bundles with scripts + docs.
Level 4  Dynamic context     Real-time context pulled from meetings, tools, discussions.
Level 5  Spec-driven dev     Prompts as specifications. Agent plans its own execution path. ≈ Harness.
```

### The Silent Failure Problem

Code crashes loudly. Bad context fails silently — the agent just produces subtly wrong output, and you blame the model. This is the most underrated problem in enterprise AI adoption.

### The Context Flywheel

Unlike Amazon's business flywheel (needs scale to spin), the context flywheel starts on day one:

```
More tasks → More context (input) → Clearer specs (PDCA) → Better agent output → More tasks
```

Models are commodities. Context is the moat.

---

## About

I'm **Yibo Zhao** — a product builder and AI practitioner based in Beijing.

Background: 6 years leading innovation businesses at Meituan (China's leading local services platform) + 5 years as a startup co-founder. For the past two years I've been running hands-on experiments with AI-native team structures, context engineering, and agent-centric organizational design.

I write about these experiments on my WeChat channel **《硅基电报》** (Silicon Dispatch).

- 📬 zhao1bo@gmail.com
- 🐦 [Twitter/X][#](https://x.com/zhao1bo)

---

## Why GitHub?

The best context engineering thinking is currently scattered across blog posts, Discord threads, and conference talks — almost none of it in structured, citable form. I'm building this repo as a living knowledge base: things I've tried, things that worked, frameworks I've stress-tested against real business problems.

PRs, issues, and discussion welcome. If you're building in this space, I want to hear from you.

---

*Language note: Most articles are written in Chinese first, as that's where my primary audience and experimental context (Meituan, Chinese AI ecosystem) lives. English abstracts are included in each article. Full translations planned for pieces that get traction.*
