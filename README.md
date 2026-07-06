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

## Examples

### 1. A bug fix

**Input:** *"prompt yaz: React uygulamamda dark-mode toggle kalıcı olmuyor, sayfayı yenileyince sıfırlanıyor"*

```
I'm building a React web app; the dark-mode toggle works during a session but
resets to light on every page refresh. With that in mind: make the theme choice
persist across reloads.

Context: use the existing theme setup in this repo (the toggle component and
wherever theme state lives) as the source of truth — don't pull in a new theming
library.

Effort: routine. The fix is probably small, but scope it like it's at the top of
your range so you catch why it isn't persisting rather than papering over it.

Act: when you've found the cause, fix it — don't ask me which storage mechanism
to use, pick the idiomatic one and tell me why.

Scope: the simplest thing that works — persist and restore the theme, nothing
more. No settings page, no new abstraction.

Evidence: before saying it's fixed, actually reload the app and confirm the theme
holds. If you couldn't run it, say so.

Report: open with the root cause and the one-line fix, then the details.
```

*Dropped Skill, Delegate, Memory, Checkpoint — a small single-file fix doesn't need them.*

### 2. A data analysis

**Input:** *"help me write a prompt for Claude to analyze our churn — I've got a customers.csv"*

```
I run a subscription product and I'm trying to understand why customers cancel,
so I can prioritize what to fix next quarter. With that in mind: analyze churn in
my data and tell me what actually drives it.

Context: the data is in customers.csv [?] (which columns are the churn flag and
the signup/cancel dates?). Use only what's in the file — don't assume fields that
aren't there.

Effort: hard. This is the analysis a quarter's roadmap rests on — scope it like
it's at the top of your range. I care more about a correct, defensible finding
than a fast one.

Act: explore the data, form a view, and give me a recommendation — don't hand me
five caveated maybes.

Evidence: every claim ("X segment churns 2x more") must trace to a number you
computed from this file. Show the check. If a result is noisy or the sample is
small, say so.

Report: open with the top 2-3 churn drivers and what you'd do about each, then the
supporting numbers.
```

*`[?]` flags the two facts only you can supply. Dropped Skill, Delegate, Scope, Checkpoint.*

### 3. A writing task

**Input:** *"draft a prompt to get Claude to write release notes from my git log"*

```
I'm shipping a release and need human-readable release notes that our users —
not developers — will actually read. With that in mind: turn my git log into
release notes.

Context: I'll paste the git log [?] (or point you at the tag range). Treat it as
the source — don't invent features that aren't in the commits.

Effort: routine, but the audience is non-technical, so the hard part is
translation, not listing. Scope it like it's at the top of your range there.

Act: group and prioritize the changes yourself — don't ask me to categorize each
commit. If a commit is internal-only (refactor, chore), leave it out.

Scope: just the notes — a short "highlights" section plus grouped changes. No
changelog tooling, no template system.

Report: give me the finished notes in Markdown, headline first. Plain language,
no commit hashes or internal jargon.
```

*Dropped Skill, Delegate, Evidence, Memory, Checkpoint. `[?]` marks the input you'll paste.*

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
