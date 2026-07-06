# prompt-anatomy

A [Claude Code](https://claude.com/claude-code) skill that turns a rough,
one-line task into a **well-structured prompt for Claude** — laid out by an
11-part prompt anatomy:

> **Task · Context · Skill · Effort · Act · Scope · Delegate · Evidence · Memory · Checkpoint · Report**

You describe what you want in a sentence; the skill returns a copy-paste-ready
prompt with each relevant section filled in, and marks any decision that's yours
to make with `[?]`. Sections that don't apply are dropped, not padded.

## Why this one

Unlike generic prompt-structuring tools that apply academic frameworks
(CO-STAR, RTF, RISEN…), `prompt-anatomy` encodes the **behavior-oriented**
sections that actually matter for Claude's agentic work — *Act* (stop
over-planning), *Scope* (simplest thing that works), *Evidence* (audit before
reporting), *Checkpoint* (pause only when it must), *Report* (outcome first).
It frames the goal and the *why*, then gets out of the model's way.

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
- "buna nasıl prompt yazayım: X"
- "make this prompt better: …"

and the skill drafts the structured prompt for you. It triggers on
prompt-*authoring* requests — not when you actually want the task done.

## The 11 sections

| Section | Intent |
|---|---|
| **Task** | The goal + the *why*, not the steps |
| **Context** | Point at where the truth lives; forbid guessing |
| **Skill** | Invoke a saved skill, or paste an exemplar of "good" |
| **Effort** | Set difficulty so the model doesn't undersell itself |
| **Act** | Act on enough information; stop over-planning |
| **Scope** | The simplest thing that works |
| **Delegate** | Hand independent subtasks to subagents |
| **Evidence** | Audit claims against real results before reporting |
| **Memory** | Surface durable lessons at the end |
| **Checkpoint** | Pause only when it truly must |
| **Report** | Outcome first, readable prose |

## License

MIT — see [LICENSE](LICENSE).

## Credits

Anatomy adapted from Ruben Hassid, *"The Anatomy of a Claude 5 prompt"*
([how-to-ai.guide](https://how-to-ai.guide)).
