# A Harness Engineering Guide for AI Coding Users

To let a coding agent work with less human supervision, you need to build a trust mechanism around its outputs. As someone who has worked with AI-generated code, I've felt this trust problem directly — LLMs are non-deterministic, they don't know your codebase, they don't truly "understand" code, they think in tokens. This article explores a mental model that combines context engineering with the emerging concept of Harness Engineering to build that trust.

"Harness" has become shorthand for everything in an AI agent except the model itself: **Agent = Model + Harness**. That's a broad definition, so it needs to be narrowed for specific agent types. This piece focuses on the *Coding Agent* context. Coding agents come with some harness built in — system prompts, code retrieval mechanisms, orchestration systems. But they also give users significant room to build an "outer harness" for their specific use case and system.

A well-designed outer harness serves two goals: **raising the probability that the agent gets it right the first time**, and **providing a feedback loop so problems self-correct before they reach human eyes**. The result is lighter code review burden, higher system quality, and — as a side benefit — less wasted tokens.

---

## The Two Pillars: Feedforward and Feedback

To effectively harness a coding agent, you need both: anticipating bad outputs before they happen, and deploying sensors to help the agent self-correct after the fact.

**Guides (feedforward control)** — anticipate the agent's behavior and shape it *before* it acts. Guides raise the probability of a good first-pass result.

**Sensors (feedback control)** — observe what the agent did and help it *self-correct*. Sensors are especially powerful when their output is optimized for LLM consumption — for example, a custom linter whose error messages include specific self-correction instructions. This is a form of intentional "prompt injection."

Without either side, the system breaks down: feedback without feedforward means the agent repeats the same mistakes; feedforward without feedback means you've written rules the agent never knows whether it's actually following.

This feedforward + feedback control framework has decades of history in industrial control systems. Birgitta Böckeler brought it into AI agent engineering — and the insight isn't just a labeling exercise. It reveals a fundamental problem: most teams today use coding agents in pure feedforward mode (write a good prompt, hand it to the agent), with no systematic feedback loop. Without feedback, the agent's behavior is entirely constrained by the model's self-discipline — and LLM non-determinism makes that constraint deeply unreliable.

---

## Computational vs. Inferential: Two Execution Types

Both Guides and Sensors split further into two execution types:

**Computational** — high determinism, fast, CPU-executed. Includes tests, linters, type checkers, structural analysis. Runs in milliseconds to seconds. Results are reliable.

**Inferential** — semantic analysis, AI code review, "LLM as judge." Typically GPU or NPU executed. Slower, more expensive, more non-deterministic.

Computational Guides raise the probability of good results through deterministic tools. Computational Sensors are cheap and fast enough to run synchronously with every agent change. Inferential controls are more expensive and uncertain, but allow richer guidance and additional semantic judgment layers. Despite non-determinism, inferential sensors paired with a capable model can still meaningfully improve confidence.

The table below shows the control taxonomy with implementation examples:

| Control content | Direction | Type | Implementation examples |
|----------------|-----------|------|------------------------|
| Agent specification | Feedforward | Inferential | AGENTS.md, Skills |
| New project bootstrapping | Feedforward | Both | Skill with instructions + bootstrap script |
| Code mods | Feedforward | Computational | Tools integrating OpenRewrite recipes |
| Structural tests | Feedback | Computational | Pre-commit hook running ArchUnit tests for module boundaries |
| Code review instructions | Feedback | Inferential | Skills |

The computational vs. inferential distinction solves a common practical mistake: handing constraints to an LLM to "understand and follow" when you should have encoded them as rules. The reliability difference between these two approaches is an order of magnitude. 

A concrete example: you write "don't call private methods directly across modules" in AGENTS.md. The LLM will usually comply — but occasionally forget, or rationalize an exception. If you encode that rule as an ArchUnit structural test, a violation is a compilation failure. No negotiation. **If it can be solved computationally, never rely on inferential.**

---

## Timing: Shift Quality Checks Left

Continuous integration practitioners face a perennial challenge: how to distribute tests, checks, and human review across the development timeline, balancing cost, speed, and importance. The goal is for every commit to be deployable. The principle: **the closer a check is to where the problem occurs, the lower the cost to fix it.**

A practical lifecycle framework:

**Before integration (or even before commit):** Which checks are fast enough to run here? (e.g., linters, fast test suites, basic code review agents)

**In the integration pipeline:** Which more expensive checks only run here, repeating the fast checks? (e.g., mutation testing, broader code review with larger context)

Two categories of continuously running sensors also need dedicated design:

**Continuous drift detection** — runs against the codebase independent of the change lifecycle: dead code detection, test coverage quality analysis, dependency scanners (Dependabot).

**Continuous runtime feedback** — agents monitoring runtime metrics: detecting SLO degradation and suggesting improvements, AI judges continuously sampling response quality and flagging log anomalies.

There's a subtle but important point here: "continuous drift detection" breaks the traditional CI/CD mental model. Traditionally, checks attach to *change events* — no commit, no check. But codebase degradation often isn't caused by any single commit; it's the slow accumulation of technical debt that nobody addressed. Designing a sensor system that runs independently of the change lifecycle is a hallmark of mature harness engineering. OpenAI and Stripe both describe similar "garbage collection" mechanisms in their engineering blogs. That's not a coincidence.

---

## Three Dimensions of Harness

Harness isn't a single-objective system. It can be decomposed into three tuning dimensions:

### 1. Maintainability Harness

The most mature dimension today, because off-the-shelf tools already exist.

Computational sensors can reliably catch structural issues: duplicate code, cyclomatic complexity, missing test coverage, architecture drift, style violations. These checks are cheap, mature, and deterministic.

LLMs can partially address issues requiring semantic judgment — semantic duplication, redundant tests, brute-force fixes, overengineering — but they're expensive and probabilistic, not suitable for every commit.

Some high-impact problems neither can reliably catch: misdiagnosed problems, unnecessary features, misunderstood human instructions. These get caught sometimes, but not reliably enough to reduce supervision needs. **Correctness is what no sensor can guarantee — the prerequisite is that humans specified what they wanted clearly in the first place.**

### 2. Architecture Fitness Harness

A combination of Guides and Sensors that define and check the architectural characteristics of an application — essentially the concept of Architecture Fitness Functions.

Examples:
- A Skill that feedforwards performance requirements, paired with performance tests that feed back improvement or regression to the agent
- A Skill describing better observability standards (e.g., logging standards), paired with debug instructions asking the agent to reflect on the quality of logging it used

### 3. Behaviour Harness

This is the elephant in the room: how do we guide and sense whether the application is functioning as intended?

Current practice in teams giving agents high autonomy:
- **Feedforward:** provide functional specifications (ranging from short prompts to multi-file descriptions)
- **Feedback:** check whether AI-generated test suites pass, whether coverage is reasonable; some teams use mutation testing to monitor test quality, combined with manual testing

**This approach over-trusts AI-generated tests, and it's not good enough yet.** Some colleagues have had success with "approved fixtures" patterns, but that fits specific scenarios rather than being a comprehensive answer to test quality.

Behaviour Harness is the most honest part of this whole picture. The truth is: **for functional correctness, we currently don't have good systematic answers.** Any solution today that claims "high-autonomy coding agents" relies on human testing as a safety net to some degree. This isn't a technical limitation — it's intellectual honesty. Any engineering team that blindly trusts agents at this layer is taking real risk.

---

## Harnessability: Not All Codebases Are Equal

Not all codebases are equally easy to harness.

Strongly typed languages provide type checking as a sensor out of the box. Clearly defined module boundaries provide architectural constraint rules. Frameworks like Spring abstract away details the agent doesn't need to touch, implicitly improving agent success rates. Without these characteristics, corresponding control mechanisms simply can't be built.

Böckeler's colleague Ned Letcher calls these characteristics **"ambient affordances"**: *"the structural features of an environment that make it more readable, navigable, and tractable for agents operating within it."*

This affects greenfield and legacy projects very differently:

**Greenfield projects:** teams can build harnessability in from day one — technology choices and architecture decisions determine how controllable the codebase will be in the future.

**Legacy systems:** especially those with accumulated technical debt, face the hardest version of the problem: harness is most needed where it's hardest to establish.

The "ambient affordances" concept points toward a possible future trend: technology selection criteria will expand beyond "which framework has the highest productivity" or "which language performs best" to include a new dimension — **"which tech stack already has mature harness available."** As escape.tech's San Francisco research report noted: "The moat is in the harness, not the model itself." Teams or tools that first establish a mature harness ecosystem for a given tech stack will hold significant first-mover advantage in the agent era.

---

## Harness Templates: Turning Experience into Infrastructure

Most enterprises have a handful of common service topologies covering 80% of their needs — API-exposing business services, event processing services, data dashboards. In mature engineering organizations, these topologies are often codified by coding agents into "service templates."

In the future, these may evolve into **Harness Templates**: bundled packages of Guides and Sensors that bind coding agents to specific topological structures, standards, and tech stacks. Teams making technology and architecture decisions may partly base their choices on "which harness templates are already available."

This faces challenges similar to service templates: once a team instantiates from a template, they start drifting from upstream improvements. Harness templates will face the same versioning and contribution problems — and non-deterministic Guides and Sensors are harder to test, potentially making this worse.

---

## The Human Role: The Irreplaceable Implicit Harness

As human developers, we bring our skills and experience as an *implicit harness* to every codebase. We've absorbed norms and best practices. We've felt the cognitive pain of complexity. We know our name is on the commit. We carry organizational alignment — knowing what the team is trying to achieve, which technical debt is tolerated for business reasons, what "good" looks like in this particular context. We move in small steps at a human pace, and that pace creates thinking space for experience to be triggered and applied.

Coding agents have none of this. No social accountability. No aesthetic aversion to 300-line functions. No instinct for "we don't do it that way here." No organizational memory. They don't know which conventions are load-bearing walls and which are just habits, or whether the technically correct solution aligns with what the team is actually trying to achieve.

Harness is an attempt to externalize and make explicit what human developers bring — but it can only go so far. Building a coherent set of Guides, Sensors, and self-correcting loops is expensive, so we must prioritize with clear goals: **good harness doesn't need to eliminate human input entirely. It needs to direct human input to where it matters most.**

This is the most philosophically important insight in the whole framework, and the easiest to skip in tool-focused discussions. The goal of harness engineering isn't "an agent that needs no human supervision." It's "an agent that focuses human supervision where it counts." These sound similar but have fundamentally different implications for how you design a harness.

---

## Industry Evidence

OpenAI documented their harness architecture — a layered architecture enforced by custom linters and structural tests, plus a regular "garbage collection" mechanism: scanning for drift and having agents propose fixes. Their conclusion: *"Our current hardest challenges are concentrated on how to design the environment, feedback loops, and control systems."*

Stripe described similar practices in their "minions" project — heuristics-based pre-push hooks, on-demand linters, emphasizing "shifting feedback left," and integrating feedback sensors into agent workflows through "blueprints."

Thoughtworks teams are addressing architecture drift using combinations of computational and inferential sensors — mixing agents and custom linters to improve API quality, or deploying "cleaning crews" to improve code quality.

The common thread across all of these: **the hardest engineering problem is no longer making the model smarter. It's designing the environment in which the model works reliably.**

---

## Open Questions

The framework is honest about what remains unsolved:

- As harness grows, how do we keep Guides and Sensors internally consistent and avoid contradictions?
- When instructions and feedback signals point in different directions, how much can we trust the agent to make reasonable tradeoffs?
- If Sensors never trigger, is that a sign of high quality — or inadequate detection?
- We need tools analogous to code coverage and mutation testing to assess harness coverage and quality.
- Feedforward and feedback controls are currently scattered across delivery steps; there's room for tooling that helps manage them as a unified system.
- Building an outer harness is becoming an ongoing engineering practice, not a one-time configuration.

---

## A Final Note

The fact that this framework was published on Martin Fowler's website isn't coincidental — Fowler's site has historically been the "certification venue" where software engineering concepts reach maturity. Harness Engineering moving from Silicon Valley niche discussion to that platform means it's serious enough for the entire industry to engage with systematically.

The greatest value of Böckeler's framework isn't a set of ready-made answers. It's a set of precise language and analytical tools — letting us discuss agent engineering at a higher level: not "what tools did you use" but "what control system did you design."

That framework itself may be the most important starting point for building your first harness.

---

*By Yibo Zhao — writing on context engineering, harness architecture, and AI-native organizational design.*  
*Based on Birgitta Böckeler's "Harness Engineering," martinfowler.com*
