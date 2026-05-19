# It's Not the Model. It's the Context.

Why do teams using the same LLM produce such wildly different results?

This isn't a new question. But watching Patrick Debois' talk *"Context Is the New Code"* at QCon London in March 2025 gave me a cleaner frame for something I'd been sensing for a while. Debois didn't say anything I'd never thought of — he articulated what I'd only half-formed into words.

Here's my version of that argument, shaped by what I've actually seen building AI-native teams.

---

## The problem: context failures are silent

There's a reason bad context is so dangerous — **you can't see it fail.**

When code breaks, the system tells you. You get a stack trace, an error message, a red CI light. The failure is loud and localized.

When context breaks, the agent doesn't complain. It just quietly produces the wrong output. And here's the insidious part: most people don't think to check the context. They assume it's a model limitation. They file a ticket against the LLM vendor. They switch models.

I've been watching this pattern in team after team, and I'd put it bluntly: **blaming the model without auditing the context is intellectually dishonest.**

Debois calls today's typical context management *Cowboy coding* — AGENTS.md files copy-pasted from blog posts, prompts hand-edited with no version history, no one knowing which rules are still active, which are stale, which contradict each other. It's an accurate description. But I'd add one thing he didn't quite say: cowboy code at least crashes. Bad context doesn't crash. It *drifts* — slowly, silently pulling agent behavior away from your intent while you're looking elsewhere.

I call this the **silent failure problem**, and it's the most underrated issue in enterprise AI adoption right now.

---

## The hidden knowledge problem

Here's something I discovered while building AI-native teams at Meituan (China's leading local services platform, roughly analogous to DoorDash + Yelp + Groupon combined):

**Our teams held enormous amounts of knowledge that had never been written down.**

Example: "Don't use negotiation approach X with client type Y — it backfires." This kind of judgment lives in a BD rep's head, or as distributed tribal knowledge across a team. It was earned through maybe hundreds of real interactions, gradually absorbed through conversations, post-mortems, and the invisible process of experienced people correcting junior ones.

In a human organization, this tacit knowledge propagates through socialization — mentorship, meetings, observation. New people get corrected and learn. The knowledge is alive.

**Agents have none of this.** Every session, they start fresh. They can't be corrected and remember it next time. Their entire "experience" is exactly what you've written in the context. The things you assume everyone knows — the things you think are too obvious to document — simply *do not exist* for an agent.

So the tacit knowledge problem, which in a human organization is a slow-burn onboarding issue, becomes **catastrophically acute** in an AI-native team. The agent doesn't make a mistake, get corrected, and improve. It makes the same mistake, forever, because it has no memory.

Debois' framing here resonated with me: AI gives us the most compelling reason we've ever had to make tacit knowledge explicit. Not as a documentation exercise — as an engineering requirement. If you don't do it, your agent will keep failing in ways you can't diagnose.

My practical response to this: I started requiring our team to narrate everything — **if it happened, transcribe it.** Meetings, decisions, client conversations, post-mortems. Not for archival purposes. To feed the context. It's a blunt instrument, but it's the foundation.

---

## The Context Maturity Ladder

Working with Debois' framework (and extending it from my own observations), I think about context engineering maturity as a five-level progression:

**Level 1 — Ad-hoc prompts**
You write a new prompt for each conversation. Nothing accumulates. The agent's capability is entirely a function of how well you described the task *this time*. Not reproducible. Not scalable.

**Level 2 — Rule files**
You write common instructions into an AGENTS.md that gets auto-injected. This is where most teams are today, and it's the primary form of Cowboy coding. Better than Level 1, but still static, still manually maintained, still no version control or testing.

**Level 3 — Structured skills**
Rule files evolve into packaged skill bundles — reusable units containing scripts, documentation, and tool integrations. Context stops being a text file and becomes an *executable engineering asset*. This is where the first real leverage appears.

**Level 4 — Dynamic context integration**
Rather than relying on static files, you build pipelines that pull context in real time — from meeting transcripts, code review discussions, decision logs, external tools. Context becomes live and observable, not a snapshot from last Tuesday.

**Level 5 — Spec-driven development**
The prompt itself becomes a specification. The agent doesn't just execute instructions — it reads the spec and plans its own execution path. Context becomes a *design language*. This is what I'd call harness-level context engineering. It's hard. Most teams are nowhere near it.

The jump from Level 1–2 to Level 3–4 is where most of the leverage is. The jump from Level 4 to Level 5 is where it gets genuinely interesting — and genuinely difficult.

---

## Context needs to be tested like code

This is the most important practical implication of Debois' framework.

He describes a three-tier testing approach:

1. **Linter-level**: format and syntax validation of context files
2. **LLM-as-judge**: semantic validation — does the context actually convey what you think it conveys?
3. **Unit-test-level evals**: the real thing

Here's a concrete eval example from Debois: a team decides all API paths must start with `/awesome`. They write that rule into the context. They build an eval that checks whether agent-generated API paths include `/awesome/user`. **If the eval fails, you debug the context — not the model.**

This requires a complete mental model shift:

| Old mental model | Eval-driven mental model |
|---|---|
| Agent output is wrong | Agent output is wrong |
| → Model doesn't understand | → Context spec is ambiguous |
| → Swap model or rewrite prompt | → Fix context → re-run eval → verify |

The deeper insight: you can't directly control an agent's output, but you can control the context it receives. Eval-driven development is the discipline of using observable outputs to infer context quality — then systematically improving it.

Every failed eval is a rule you didn't write clearly enough. The method is tedious. The ROI is enormous.

---

## The context flywheel

Here's where I'd extend Debois' framework with something I've been thinking about more broadly.

Most AI strategy discussions focus on model selection. This is the wrong question to optimize for. The frontier models available today are already superhuman on most cognitive tasks. The gaps between models are narrowing. The switching costs are falling.

But the context your team accumulates — the documented decisions, the captured post-mortems, the encoded tribal knowledge, the tested rule systems — **that's private.** It compounds. It can't be replicated by switching to a better model next quarter.

This creates a flywheel:

```
More tasks / projects
    → More context captured (input)
    → Clearer, more tested specs (PDCA)
    → Better agent output (output)
    → More tasks you can take on
```

Compare this to Amazon's business flywheel, which requires scale before it spins. **The context flywheel starts spinning on day one.** The team that starts context engineering today, even imperfectly, builds a structural advantage that compounds over every model generation.

---

## The bottom line

It's not the model. It's the context.

The model is public infrastructure — a commodity that gets better and cheaper every year, available to everyone equally. Your context is the only part of this system that belongs to you: hard-won, proprietary, irreplaceable.

Start accumulating it now. Even clumsily. **Models come and go. Good context compounds forever.**

---

*By Yibo Zhao — writing at the intersection of context engineering, agent architecture, and AI-native organizational design. I run experiments at scale and share findings at 《硅基电报》 (Silicon Dispatch) on WeChat.*

*Inspired by: Patrick Debois, "Context Is the New Code," QCon London, March 2025.*
