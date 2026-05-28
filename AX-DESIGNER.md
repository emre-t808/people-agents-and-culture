# AX Designer

*A draft job description.*

This is more of a manifesto than a job description, for a role that doesn't exist yet but will.

Because software was designed for us. We are creatures with eyes, hands, and somewhat reliable short term and long term memory.

When agents are the users, the user now is none of those things.

The user is a language model running inside a harness, looking at the tool list through a context window, billed by the token, with no peripheral vision, hazy working memory between turns, one vague error message away from giving up silently.

**AX is not UX for AI.**

We need a designer role specializing in designing the agent experience: AX. AX is parallel to UX. Parallel to DX. Distinct from both. AX and UX share certain principles like clarity, consistency, forgiveness, discoverability, progressive disclosure. But they differ in how the experience is perceived. The design choices make or break the experience in both.

AX requires you to think like an LLM, instead of a human.

A short flag (`-r`, `-f`) is a gift to a human power user and a gravestone for an agent that doesn't remember what `-r` did three turns later. A modal dialog pop-up is an accessibility headache for a human and an unrecoverable dead end for an agent. A 3,000-word system prompt is bad UX for a developer but can be vital for the same developer's agent.

The AX role demands its own taste.

This document is the case for the role, and the first job description for it. F\* this document. Seriously, fork it and modify it as you wish.

---

## Why now

The role doesn't exist on most org charts. The first companies that do will compound the advantage for two or three years before anyone else notices.

Three things changed in twelve months. Together, they create the why.

**1. Code is everywhere now.** Code used to be scarce. What's scarce now is human time, attention, and the model's context window. Token billionaires are running roughly a billion output tokens per day without touching an editor in months. More and more code ships through agents.

**2. Agent horizons are growing one order of magnitude per release cycle now.** Anthropic's internal benchmark (the METR meter) shows Sonnet 3.7 at roughly one hour on minimal scaffolding; Opus 4.6, twelve hours; internal harnessed agents, thirty-plus. Between Opus 4.5 and 4.6, the harness simplified even as horizons extended; features were deleted because the model no longer needed them.

This means agent experience changes as models evolve. Every harness primitive maps to a current model weakness. Be it context, planning, or self-judgment. As the model absorbs the weakness, the primitive becomes dead weight, and someone needs to strip it out.

**3. The agents can now access internal tools just as easily and often as people.**

Not at every company, but increasingly at more companies. OpenAI's harness team, the Cursor and Codex platform teams, the Claude Code team itself claim humans no longer drive directly. Agents do. These companies are extreme examples perhaps but not irrelevant. Certainly, the direction we are on.

Also, good AX saves lives (tokens at least). Across that surface (CLIs, MCP servers, error messages, help text, tool descriptions, skill packs, route configs, structural lints) almost nobody is responsible for the agent's experience of the interface. At the moment we ship tools and interfaces that are functionally correct but ergonomically catastrophic to agents. Then we wrap a CLI or MCP around them and hope for the best at runtime.

AX Designer role closes the loop.

Surely, the first companies to hire for it (or code it) will out-compete the rest. (Don't call me Shirley ✈️)

---

## UX vs AX

UX is for humans. AX is for agents.

UX designs for *human intuition*: embodied, visual, click-driven, forgiving because the human has the next move in their head. A button looks pressable because the human has pressed real buttons.

AX designs for *the model's training distribution under attention scarcity*. A Unix subcommand looks usable to a model because the model has seen `git`, `kubectl`, `aws`, and a billion `man` pages. **Familiarity to the model is the affordance system.** That sentence carries most of the discipline.

| Dimension              | UX (human) 👤                          | AX (agent) 🤖                                                  |
| ---------------------- | -------------------------------------- | -------------------------------------------------------------- |
| Sensory modality 👂    | Visual + tactile + embodied            | Token stream, read once per turn                               |
| Affordance 👆          | What looks pressable                   | What looks like the model's training corpus                    |
| Memory 🧠              | Working memory + the screen            | Whatever is in context this turn                               |
| Failure mode ⚠️         | Confusion, abandonment, support ticket | Silent retry, escape, $40 of wasted tokens                     |
| Attention budget ⏱️     | Hours of session time                  | Context window, ~80% before degradation                        |
| Recovery ↩️             | Undo, back button, ask a coworker      | Read your own error message, or stop                           |
| Unit of analysis 🖱️     | The click, the screen, the flow        | The turn, the verb, the response                               |
| What "feels good" 🙂   | Low cognitive friction, fast feedback  | Low token friction, deterministic shape, cacheable prefixes    |
| Documentation 📖       | Onboarding, tooltips, support docs     | `--help`, error text, tool description. There is nothing else. |
| Worst sin 🔒           | Hidden state the user has to discover  | Hidden state the model cannot discover                         |

---

## Core AX Principles

The working canon. Numbered for reference and forking convenience.

1. **Verbs over flags.** `lever candidates list` beats `lever -lc`. Subcommands are how the agent reasons about intent; flags are how a human power user compresses what they already know.

2. **`--help` is a writing surface, not metadata.** It must be sufficient guidance with no other docs loaded. Write it the way a good UX writer writes button labels: every word fights for its place.

3. **Errors are navigation.** Every error contains *what failed* and *what to do next* in the same string.

4. **Surface stderr loudly.** A silent stderr is a ten-retry agent loop waiting to happen. Tokens you can't see, billed back to a specific feature, are tokens you can't optimize.

5. **Binary guard before output.** No raw bytes hit the response stream unannounced. The cost of one unannounced binary dump exceeds the cost of every binary-guard line you'll ever write.

6. **Overflow mode by default.** Long output collapses to a preview, a temp-file path, and a suggested next command (`grep`, `tail`, `jq`). The agent reads the preview, knows where to look, runs the next command. Three turns saved.

7. **Metadata footer on every result.** `[exit:0 | 1.2s | 184 lines]` after every command. Consistency is how the model learns the shape of your system in-context.

8. **Stable prefixes, deterministic ordering.** Vagueness in tool descriptions, hash-randomized ordering, timestamps in default output, and so on. Each one adds thousands of dollars a month at scale in tokens. *Determinism is ergonomics.*

9. **Progressive disclosure of help.** Tool description names the verbs. Verb with no args returns usage. Verb with bad args returns specific parameter help. Don't pre-load a three-thousand-word system prompt because the agent might need one paragraph.

10. **Composition beats enumeration.** A small set of orthogonal verbs that compose beats a sprawling menu of bespoke single-purpose tools. The model knows how to pipe; it doesn't know your bespoke API.

11. **Externalize state to the file system.** The agent forgets between turns. Files don't. Write intermediate state to disk and let the next turn read it; never trust the conversation to carry structured data.

12. **Design for the 80% context-rot threshold, not the 100% limit.** Quality degrades before the window fills. Your tool's verbosity competes with the user's actual task. Be verbose where you teach, terse where you don't.

13. **Bundle skills with the capability.** When you ship a new integration, the skill pack ships alongside it. A capability without its skill pack is a capability that won't be used.

14. **Namespace everything.** `google.calendar.list`, not `list`. Collisions are silent failures.

15. **Instrument the escape.** When an agent gives up, you should see it in your dashboard before you see it on your bill. The agent's silent failure is *your* funnel drop-off: the part of the user journey that doesn't generate a support ticket because the user has no fingers to type one.

But wait, there's more. The sixteenth principle, or the motto:

16. **Recoverable failure beats clever success.** Keep the wrong stuff in. Preserved errors and stack traces help the agent adjust priors. Hiding failure produces an agent that thrashes blindly, looking confident, learning nothing. The Manus context-engineering research is a good example with their *let the agent see what it just broke* rule.

---

## The job description (Draft)

### About the role

You design the interfaces (CLIs, help text, error shapes, tool taxonomies, MCP surfaces, skill packs, command vocabularies) that LLM agents use to drive complex work end to end. Your user is a model running inside Claude Code, Codex, Cursor, Gemini CLI, or an internal harness. Your medium is the token stream. Your craft is the discipline UX practitioners spent thirty years building, ported to a user who reads instead of looks, composes instead of clicks, and forgets everything between turns unless you put it in writing.

This is not prompt engineering. Prompt engineering is the inner loop of one conversation. AX is the *outer loop*: thousands of conversations, hundreds of agents, every level of the stack.

This is also not developer experience. DX is for humans who write code; AX is for models that drive tools. The two look similar and diverge on every concrete decision.

### What you'll do

**Design the agent-facing surface.** Audit every CLI, MCP server, and tool surface for agent-friendliness. The bar: *can a fresh agent session get from zero to useful in one turn-budget?* Build wrappers around capabilities that exist only as raw functions or scripts. Design the verb taxonomy so the surface feels like one coherent system, not a graveyard of one-off helpers.

**Author instruction-grade `--help` text.** Help output is your primary writing surface. It must be sufficient agent guidance with no other documentation loaded. The team will read your `--help` text the way good product teams read marketing copy: as the thing the user actually sees.

**Design error shapes and recovery loops.** Establish the house style for navigational error messages. Build binary-guard, overflow, and metadata-footer patterns into every CLI the platform ships. Make silent failures impossible.

**Own the skill, hook, and route surface.** Design skill packs that bundle with the integrations they document. Co-design hooks that route bare tool calls to enriched skills: enforcement, not opt-in. Maintain context routing so each intent loads the right context budget.

**Make agent behavior measurable.** Define and instrument the AX metric set: KV-cache hit rate, escape rate (the agent gave up), help-text-to-success ratio, mean turns to completion, token-per-task budget adherence. Stand up an eval suite of golden tasks that runs against every integration on every change.

**Read traces.** Trace-reading is the primary debug loop. The transcript is the trace, the divergence is the bug, the prompt change is the fix. Run the pipeline weekly. Promote every reproducible divergence into a fix in the interface, prompt, or test.

**Own the scaffolding-deletion cycle.** Every model release absorbs some workarounds the previous one needed. Harness primitives map to current model weaknesses; as a weakness goes away, the primitive becomes dead weight. The AX Designer looks at the harness on the day a new model ships and asks *what can we delete?* Continuous mandate, not one-time audit.

**Set the standard.** Author the anti-patterns and design-requirements documents that codify the discipline at your company. Run AX reviews on PRs touching agent-callable surfaces, the same way frontend teams run design reviews on UI changes. Teach the team. Most engineers are good at building tools; few have been trained to design for a non-human user.

### What we're looking for

**Agent-driven tool design and agents in production:** Claude Code, Cursor, Aider, Gemini CLI, Codex, an internal harness, a homegrown LangGraph or AutoGen setup. The stack matters less than this: *you have watched an agent fail your interface and gotten opinionated about it.* You have shipped at least one CLI, MCP server, or tool surface where the primary caller was a model.

**Strong CLI sensibility.** Deep familiarity with Unix philosophy, POSIX semantics, exit codes, stderr conventions, progressive disclosure. You have built CLIs people praise without prompting. You can defend your choice of `clap` or `cobra` or `commander`. You know why `git status` is good UX and `git rebase` is not.

**Context engineering literacy.** Practical familiarity with prompt caching, context windows, KV-cache hit rate, system-prompt vs. user-message tradeoffs, lost-in-the-middle, the 80% context-rot threshold. You can explain *why* keeping failed actions in agent context is sometimes correct.

**MCP and tool-use fluency.** You understand MCP's tradeoffs against CLI and raw API; you know when each is right. You have built or extended an MCP server, or hold strong opinions on JSON-RPC tool schemas, structured tool output, and how tool descriptions get tokenized.

**Communication.** You write `--help` text the way good UX writers write button labels. You can explain the same concept to a non-technical executive ("the agent gives up silently and we don't see it") and to a platform engineer ("we need a binary guard before piping cat into the response stream").

### Nice to have

A background in UX, IxD, or DX: the discipline lineage matters even though the specifics diverge. Experience with eval frameworks (Inspect, Braintrust, Promptfoo). Open-source authorship of any tool surface where you watched real users (or agents) drive something you built. Experience designing for multiple agent harnesses; the cross-harness portability problem is real.

### First 90 days

**Week 1–2: listen.** Run realistic tasks through every agent-callable surface, token meter open. Log every stall, retry, junk dump. Produce a one-page AX audit ranking surfaces by friction cost.

**Week 3–4: fix the worst offender.** Pick the most painful surface. Ship a wrapper with instruction-grade `--help`, navigational errors, structured output, and a binary/overflow guard. Write the eval that proves it improved. Use the delta as the case study for the rest of the org.

**Month 2: establish the standard.** Author your company's AX anti-patterns and design-requirements documents. Eight to twelve named anti-patterns with detection commands, in the format of your existing frontend or code-style standards. Wire them into PR review so they catch violations before merge.

**Month 3: make it self-reinforcing.** Stand up the AX eval suite: ten to fifteen golden tasks per critical integration, run nightly, plotted against KV-cache hit rate, escape rate, and mean turns to completion. Wire the results into your LLM-judge or review-agent pipeline so a regressing tool-surface change gets caught before merge.

**Ongoing.** Run AX reviews on PRs touching agent-callable surfaces. Keep a backlog of small friction fixes that compound into hours of agent time saved per week. Audit the harness on every frontier model release for what can be deleted.

---

*This is a draft. Fork it.*
