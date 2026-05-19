# Experiments in AI Organizational Transformation: Notes from a Small Team

Earlier this year, as AI shifted from conversational companion to actual work executor, a wave of "new paradigm" discussions emerged. Both the engineering exploration of Agent Harness and various AI organizational transformation experiments have been feeling their way forward in this direction.

This year's practices from teams like Codex and PeterPang's "AIFirst" approach seem to have given the new paradigm tangible shape: don't try to optimize every process with AI. Instead, start from the assumption that AI should do this thing, then decide which parts humans support. The human is no longer the center of the workflow. The Agent is. Radical — but it works.

Observations from startup teams and one-person companies on X have also been clarifying this direction: flat, high-autonomy small teams will be the basic organizational unit of the AI era. The organizational unit is no longer a job title — it's an "Agent + human" hybrid.

This resonated with something from a long profile I read on a founder, which reminded me of a model from Chinese e-commerce a decade ago: three-person product groups (designer, buyer, visual) each owning a category end-to-end, with full decision rights from selection to pricing to production to sales. Hundreds of autonomous small teams enabling "small batch, fast return" in a rapidly changing market. More recently, certain consumer hardware companies have used a similar "incubator + BU" model — splitting product lines into independent BUs with P&L ownership, independent hiring, and internal competition.

Putting these examples together, they point to the same fact: **organizational structure itself has become the real productivity bottleneck of the AI era.** The hierarchical model that industrial-era companies inherited — the decision-making mechanisms built on information asymmetry — is losing its effectiveness. If you treat AI as an add-on tool bolted onto existing processes and org structures, productivity gets capped by the old organization. You're still at the "steam engine attached to a water wheel" stage.

I've also come to feel that AI organizational transformation is fundamentally harness engineering unfolding simultaneously at the code layer and the organizational layer. Harness does system design for agents while also needing to accommodate the "Agent + human" hybrid working mode. Both layers are doing the same thing in different languages.

---

## 1. The Minimal Operating Unit Experiment

Our team runs satellite store operations — a sub-business unit under Meituan's mature delivery business, focused on supply-side exploration. High uncertainty, high ambiguity, rapid iteration. We constantly absorb knowledge from the industry and validate through practice. AI fit our situation perfectly, giving us soil to experiment with "Agent + small team" structures. The team gradually formed a pattern: for anything ambiguous, unfamiliar, or lacking precedent, we adopt the OPC model — let the Agent lead, with humans asking questions and converting them into coding inputs.

**The context:** satellite stores are delivery-only outlets opened by established restaurant brands. These brands are good at managing physical dining experiences, but delivery traffic operations — precisely the weakest link — require a different skillset they often don't have.

We'd never quite believed this problem was real from our platform perspective. Surely a major brand knows how to run ads? Then we ran stores ourselves and discovered it was genuinely true.

Looking back, we'd been using a "modular division of labor" approach: platform operations sliced by geography, each person owning a region, improvements transmitted through a three-layer chain: headquarters → sales → merchant. Every additional layer meant another round of information loss, another dilution of goals and incentives. We were stuck in the trap of "the more precise the work, the narrower the coverage."

In December last year, a new project required our team to handle online operations directly. Facing something we'd never done before, we started by learning as we went. As the project scaled from one person to two, then three, we noticed something was wrong — we were stuck in a trap of doing something we weren't good at and couldn't do better than the merchants themselves.

That's when we decided to hand it to AI. *It doesn't have to be better than us — just not worse.* We hand-coded an automated ad-placement script. At first it only did one thing: repeat existing campaigns. But in the process, our thinking shifted. We gradually wanted the agent to handle placement performance, strategy, analysis, and post-mortems too (humans are lazy). We kept throwing questions at AI coding to produce solutions.

From a single automation script, we slowly built an intelligent placement agent that could automatically scan store traffic levels, diagnose problems, generate strategies, execute placements, collect data, and readjust. Eventually we found the agent's strategy density and reaction speed had exceeded what any one person could invest — and we didn't need that person on placements anymore. They just needed to keep building the agent.

Beyond efficiency gains, the more important shift was that **the job itself changed.** The "store operations" role became an Agent "coach." From several people responsible for one thing, to one coach running an Agent team — and the coach was only part-time.

---

## 2. Context Is the Organizational Foundation

Treating an Agent as a team member is far more complex than "installing a tool." An agent that genuinely participates in team thinking needs access to the same information as every team member — otherwise, no matter how smart the model, it's reasoning inside an information blackbox. The outputs won't be trusted, and you can't actually build an organization around it.

Starting in April, we rolled out something we internally called the "World Model" — a context engineering initiative. The goal: make the information accessible to the agent as close as possible to what every team member has access to. We divided inputs into three categories:

**Data.** The easiest for agents to obtain, with the clearest permission rules. Structured base tables connect relatively directly, with permissions piggybacking on existing employee access.

**Text.** Building a knowledge base the agent can actually read (the prompt foundation). This was initially messy — historical documents scattered across spaces with no governing rules. We selected what already existed, created new rules for additions, and consolidated everything into one agent-readable knowledge base.

**Skills.** What employees know and don't know — what the team is capable of.

**Interactions.** Daily discussions and meetings — the most overlooked category, and the highest-density information. This is the team's actual "thinking space." If the agent could use it, the leverage would be greatest: the complaints and suggestions in client visits, the hypotheses thrown out in casual discussions, the dissenting views in reviews. These signals carry important threads of insight that have always disappeared into memory.

Our method was blunt but effective: all frontline-attended meetings, discussions, and client visits get transcribed to text and added to the knowledge base for the agent to read.

When the agent's information density approaches the team's, it can use the model's reasoning capacity to make deeper judgments for the team. This is the foundation for "Agent-centric, human-as-support" — and AI-native org design becomes possible.

Looking back, our earliest placement agent was our first rough attempt at harness: clear role definition, tools (placement management, data diagnostics), guardrails (budget caps, ROI constraints), error handling (monitoring alerts), and a human-agent collaboration protocol (humans set goals, agent sets strategy and executes). We didn't know at the time we were doing harness engineering. In retrospect, the hard part wasn't writing the script — it was gradually building the whole apparatus around the agent as the primary working entity.

Context, I've come to understand, is just the "information input" layer of harness engineering. It sets the ceiling on the agent's thinking. But harness is much more than that. At the organizational layer, much harder work lies ahead. We're only just starting to dig in.

---

## 3. The "Agent + Human" Project Team Model

After the placement agent was running, nobody managed store placement anymore. After context was up, the agent was contributing project direction. Something became gradually clear to the team: **the agent isn't just a tool. Treat it as a team member.**

The three-layer Agent Harness structure as I understand it maps directly to organizational structure:

- **Foundation layer:** the model's and tools' execution capacity — maps to each project's "Agent + human" base capability combination
- **Middle layer:** context engineering's skills, MCP, etc. — maps to making agents and humans share real-world information, matching agent skills
- **Top layer:** orchestration and protocol mechanisms — maps to team architecture design and "Agent + human" rule orchestration, building an org system suited to agents

The top orchestration layer is the hardest, and it's what current startup teams generally can't crack. When we discussed using project-based structure for team organization with our HRBP, that sentence sounded light at the time. It got heavier at every decision point afterward.

With HRBP support, we extended the agent-centric model across the whole team's working mode. From a previous "modular division of labor," we shifted to 3-5 person small teams. The people didn't change much, but the working philosophy and expectations inside the team did. We stopped counting by headcount and started counting by "Agent + human" hybrid units. The biggest difference from previous small teams: the mechanism requires everyone to build around the agent as the center.

In this process, we continuously filtered for team members with genuine AI aptitude — and pulled along lighter AI users by necessity (because there was more work, more ambiguity, and no escape).

Concretely, several things changed:

**Job boundaries dissolved.** Previously divided between sales, operations, and product, with some people skilled in data and others in strategy. Now everyone is aligned. Org is organized by direction, everyone assigned by project. The question shifted from "can you do X" to "can you use an Agent to do X."

**Meetings dropped dramatically.** The team's large multi-person meetings are now countable on one hand. After context started running, we basically eliminated "all-hands" meetings. Everything compressed into small meetings each person runs for their own projects, or human-agent interactions. Context broke the information gap across the team; our team agent shares information with everyone daily; AI maintains progress in real time.

**Decision chains shortened.** From the old model where one project drags a crowd along, to each person owning a specific project end-to-end. This forces independent decision-making across the full cycle: research, pilot, iteration, kill or scale. The organization only sets goals, provides resources, asks for results, and assigns accountability.

---

## Unexpected Findings

**The project owner role is a magnifying glass.** Under the old modular structure, one person typically showed only partial ability, with strong boundary instincts. Whether someone had full-chain judgment was rarely visible. Project-based structure compresses direction, orchestration, judgment, and results onto one person. Capability maps get fully exposed. Either you fail or you find a way to do it with the agent — and once you do it with the agent, you enter the positive flywheel: more tasks → more context → clearer specs → better agent output → more tasks. The people we've seen perform best in these months would never have been visible in their original roles over several years. I didn't anticipate this as a side effect. I've started calling it talent.

**The more AI you use, the higher the bar for what humans need to be.** When most execution is handled by agents, human marginal value concentrates at a small number of judgment nodes. Any deviation in judgment gets amplified by the agent across the entire chain. Leaner org, fewer people — doesn't mean easier. People carry agents; their capability boundaries expand; it's easy to fall into the illusion that agents can do anything. It means each person's "capability density" has to be higher. I sometimes wonder whether people have actually changed — the most counterintuitive thing about the AI era might not be that AI makes people's work easier. It's that **AI makes people more expensive.**

Did organizational transformation drive changes in people, or did changes in individual workflows drive organizational transformation? I don't have a clear answer. Replaying what I've observed, it looks more like "workflow changed first" — the sharpest people on the team started using AI to rewrite their own working methods, and the organization followed, passively then actively. But I can't say whether this is a universal pattern or specific to exploratory businesses like ours. Maybe organizations that transform structurally first achieve higher overall efficiency.

---

## 4. What I Haven't Figured Out

I have to be honest here: the biggest blocker in our approach right now is that we haven't established mature harness engineering around the agents — specifically the middle-layer problems described above, and all of the third layer.

Harness for organizational transformation isn't just a metaphor. It's more like a guiding philosophy. Building an org around agents and building harness around agents are the same project — they're just being done in different languages. In pure coding-type work, these two are easily seen as one. But for work involving more physical-world inputs and outputs, what should the human part of "Agent + human" actually look like? The former we've just gotten working. The latter we're still getting wrong.

Three principles from the industry I've found worth using as reference coordinates:

**Agents need JDs.** PeterPang's team repeatedly emphasizes: every agent must have a clear role, goals, skills, and permission boundaries. An agent without boundaries isn't trustworthy. This is isomorphic to writing job descriptions for employees — except now you're writing a JD for an "Agent + human" hybrid, and the requirements are stricter. Humans have socialized common sense as a backstop. Agents don't. Everything that goes without saying has to be said.

**Agent + human collaboration runs on protocol, not tacit understanding.** Getting one agent working is one thing. Getting ten agents working together is another. Getting ten agents and humans working together is something else entirely. The hardest part of Harness engineering is orchestration — who goes first, who overrides whom, who arbitrates conflicts. This maps almost exactly to multi-team coordination in physical organizations. In other words, the governance challenge of building agent-centric teams is fundamentally an extension of organizational design challenges into the code world — and that step still hasn't been solved.

**Observability is a prerequisite for governance.** Invisible agents can't be governed — just as the "tacit agreements" and "relationships" hidden beneath the surface of human teams can't be codified into rules. "Agent + human" systems must have explicit, visible rules, incentives, and accountability. As agent count grows from 1 to 100, the real constraint on organizational ceiling isn't agent quantity — it's sufficient predictability.

---

## Where We Are

Based on our immature current practice, I think there are three thresholds for organizations entering the AI era:

**First threshold:** getting the first agent working. We just cleared this.

**Second threshold:** agents and humans sharing a thinking process. We're on this path.

**Third threshold:** coordinating the tenth, twentieth agent. This is where I expect we'll spend most of our effort in the months ahead.

Writing this, I have no desire to summarize or draw conclusions. I just notice that once change starts, many things feel like they're pushing you forward rather than the other way around. Past methodologies sought to eliminate uncertainty. Under a new worldview, chaos is irreducible. I don't know what comes next. The only orientation that makes sense is to stay as close as possible to the frontier.

---

*By Yibo Zhao — these are field notes from ongoing experiments in AI-native team design at Meituan's satellite store business unit. All mistakes are mine and the experiments continue.*
