# prompt-anatomy

A [Claude Code](https://claude.com/claude-code) skill that turns rough task
requests into focused, execution-ready prompts — using only the context,
guardrails, and evidence checks the task actually needs. Laid out by an
11-part prompt anatomy in three tiers:

> **Core:** Task · Context · Done · Scope · Evidence
> **Dials:** Effort · Autonomy · Report
> **Extras:** Skill · Delegate · Memory

You describe what you want in a sentence; the skill returns a copy-paste-ready
prompt with each relevant section filled in, and marks any decision that's
yours to make with an inline `[?]`. Core sections are always in; dials appear
only to override the recipient's defaults; extras only when they apply.
Sections that don't earn their place are dropped, not padded.

## Why this one

Unlike generic prompt-structuring tools that apply academic frameworks
(CO-STAR, RTF, RISEN…), `prompt-anatomy` encodes the **behavior-oriented**
sections that actually matter for Claude's agentic work — *Done* (the test the
output must pass), *Evidence* (audit claims before reporting — against Done),
*Scope* (simplest thing that works), *Autonomy* (act on enough information,
pause only when it must), *Report* (outcome first). It frames the goal and the
*why*, then gets out of the model's way — and it knows that a prompt aimed at
Claude Code shouldn't restate the harness's own defaults back to it.

The anatomy is adapted from Ruben Hassid's *"The Anatomy of a Claude 5 prompt"*
infographic (["How to AI"](https://how-to-ai.guide)).

## Install

Drop the folder into your global skills directory:

```bash
git clone https://github.com/huguryildiz/prompt-anatomy.git
mkdir -p ~/.claude/skills/prompt-anatomy
cp prompt-anatomy/SKILL.md ~/.claude/skills/prompt-anatomy/SKILL.md
```

Now in any Claude Code session, ask something like:

- "help me write a prompt to have Claude clean up the logging in my API"
- "how should I prompt this: X"
- "make this prompt better: …"

and the skill drafts the structured prompt for you. It triggers on
prompt-*authoring* requests — not when you actually want the task done, not
when "the prompt" is a string in your application code, and not for system
prompts or agent personas.

## Examples

### 1. A bug fix

**Input:** *"write a prompt: my React app's dark-mode toggle doesn't persist, it resets on page refresh"*

```
Task: I'm building a React web app; the dark-mode toggle works during a session but resets to light on every page refresh. With that in mind: make the theme choice persist across reloads.

Context: use the existing theme setup in this repo (the toggle component and wherever theme state lives) as the source of truth — don't pull in a new theming library.

Done: the theme survives a page refresh, and the saved theme is read before first paint — no flash of the wrong theme on load.

Scope: the simplest thing that works — persist and restore the theme, nothing more. No settings page, no new abstraction.

Evidence: before saying it's fixed, actually reload the app and confirm the theme holds against Done above. If you couldn't run it, say so.

Autonomy: pick the idiomatic storage mechanism yourself and tell me why; pause only if fixing this means restructuring how theme state is managed app-wide.
```

*Dropped: Effort and Report (Claude Code defaults cover them), Skill, Delegate, Memory.*

### 2. A data analysis

**Input:** *"help me write a prompt for Claude to analyze our churn — I've got a customers.csv"*

```
Task: I run a subscription product and I'm trying to understand why customers cancel, so I can prioritize what to fix next quarter. With that in mind: analyze churn in my data and tell me what actually drives it.

Context: the data is in customers.csv [?] (which columns are the churn flag and the signup/cancel dates?). Use only what's in the file — don't assume fields that aren't there.

Done: drivers ranked by effect size, each backed by a number computed from this file — a finding I could defend in a roadmap discussion.

Scope: just the analysis and the write-up — no dashboard, no automated pipeline.

Evidence: every claim ("X segment churns 2x more") must trace to a number you computed from this file. Show the check. If a result is noisy or the sample is small, say so.

Effort: hard in one place — a correct, defensible finding matters more than a fast one. Spend the effort on validity (confounders, sample size), not on breadth of charts.

Delegate: if distinct cohorts (plan tier, signup channel) need separate digging, split that across subagents and bring back one synthesis rather than five partial views.

Report: open with the top 2-3 churn drivers and what you'd do about each, then the supporting numbers.
```

*`[?]` flags the fact only you can supply. Dropped: Autonomy (default), Skill, Memory.*

### 3. A writing task

**Input:** *"draft a prompt to get Claude to write release notes from my git log"*

```
Task: I'm shipping a release and need human-readable release notes that our users — not developers — will actually read. With that in mind: turn my git log into release notes.

Context: the range is [?] (paste the log or give me the tag range). Treat it as the source — don't invent features that aren't in the commits.

Done: a two-line highlights section up top, then grouped bullets in plain language — every note traceable to an actual commit in the range.

Scope: just the notes — no changelog tooling, no template system. Leave internal-only commits (refactor, chore) out; group and prioritize yourself rather than asking me to categorize each commit.

Evidence: before finishing, check the notes against Done above — nothing invented, nothing user-facing missed.

Report: finished notes in Markdown, headline first. No commit hashes or internal jargon.
```

*`[?]` marks the input you'll paste. Dropped: Effort, Autonomy (defaults), Skill, Delegate, Memory.*

## The 11 sections

| Section | Tier | Intent |
| --- | --- | --- |
| **Task** | core | The goal + the *why* + who it's for, not the steps |
| **Context** | core | Point at where the truth lives; forbid guessing |
| **Done** | core | The test the output must pass — exemplar, invariant, or observable check |
| **Scope** | core | The simplest thing that works; anti-goals |
| **Evidence** | core | Audit claims against real results — and against Done |
| **Effort** | dial | One honest level, with the consequence named |
| **Autonomy** | dial | Act on enough information; pause only when it truly must |
| **Report** | dial | Outcome first + the deliverable's format |
| **Skill** | extra | Invoke a saved skill by name |
| **Delegate** | extra | Hand independent subtasks to subagents |
| **Memory** | extra | Surface durable lessons at the end |

Dials are a Claude Code recipient's defaults — they appear in a prompt only to
override one. For claude.ai/API recipients, they earn their place.

## License

MIT — see [LICENSE](LICENSE).

## Credits

Anatomy adapted from Ruben Hassid, *"The Anatomy of a Claude 5 prompt"*
([how-to-ai.guide](https://how-to-ai.guide)).
