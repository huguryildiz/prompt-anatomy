---
name: prompt-anatomy
description: >-
  Use when the user wants help WRITING or IMPROVING a prompt or task
  instruction they will hand to Claude or another agent — "help me write a
  prompt", "how should I ask Claude to...", "draft a prompt for...", "make
  this prompt better", "prompt yaz", "buna nasıl prompt yazayım". The
  deliverable is the prompt text itself, never the task's result. Do NOT use
  when the user wants the underlying task performed, when "the prompt" is a
  string inside application code to be edited, or for persistent system
  prompts / agent personas — those are different genres.
---

# Prompt Anatomy

Turn a rough task into a copy-paste-ready prompt, organized by an 11-part
anatomy: five **core** sections every prompt needs, three **dials** that
override the recipient's defaults, and three **extras** for when they apply.
A model with good theory of mind does far more with a well-framed goal than
with a pile of step-by-step commands — so the anatomy carries the *why*, where
the truth lives, what "done" means, and how to prove it.

## When this triggers

The user wants to *author or sharpen a prompt* they will hand to Claude — not
to have the task done. If they actually want the task done, ignore this skill
and just do it.

## Workflow

1. **Read the rough task.** Extract the real goal, who it's for, and the
   **recipient runtime** — Claude Code, claude.ai, or another agent/API. If
   the runtime is unknown and it changes the prompt, that's a `[?]`.
2. **Fill the five core sections** (Task, Context, Done, Scope, Evidence).
   Write from the user's point of view — the prompt is *theirs*, addressed to
   Claude. Draw concrete details from the conversation where they exist.
3. **Add dials only to override a default.** Effort, Autonomy, and Report are
   already a Claude Code recipient's defaults — include one only when the task
   needs a non-default (pause more often, an unusual effort split, a specific
   deliverable format). For claude.ai/API recipients the dials earn their
   place.
4. **Add extras only when they apply.** Skill needs a named skill; Delegate
   needs genuinely independent parallel subtasks; Memory needs a recurring
   workflow. Drop non-applicable sections silently.
5. **Return the prompt in a single fenced block**, `[?]` markers inline, plus
   at most one line of notes after the block.

**The `[?]` rule.** Any decision only the user can make (target file, audience,
effort level, whether subagents are wanted) gets a `[?]` with a short inline
question — *inside the fenced block, in the section it belongs to*. The block
must never read more confident than you are. Max 3 markers; if more decisions
are genuinely open, ask the user first, then draft. Never fabricate context,
file paths, or constraints to look complete.

## The 11 sections

Each section gives its *intent* and a *seed phrasing* to adapt freely — the
seed is a starting point, not a script.

### Core — default in

1. **Task** — the goal + the why + who it's for, not the steps.
   *Seed:* "I'm working on [LARGER GOAL] for [WHO IT'S FOR]. They need [WHAT
   THE OUTPUT ENABLES]. With that in mind: [THE TASK]."

2. **Context** — point at where the truth lives; forbid guessing.
   *Seed:* "Use everything in [this project / these files / CLAUDE.md] first.
   If it isn't there, pull it from [source] — don't guess."

3. **Done** — the test the output must pass: an observable result, an
   exemplar, or an invariant. This is what Evidence audits against.
   *Seed:* "Done means [OBSERVABLE RESULT], checkable by [TEST / CHECK].
   Here's what good looks like: [exemplar]."

4. **Scope** — the simplest thing that works, plus anti-goals.
   *Seed:* "Do the simplest thing that works well. No extra features,
   refactors, abstractions, or error handling for impossible cases. If I'm
   describing a problem, the deliverable is your assessment."

5. **Evidence** — audit claims before reporting, against Done.
   *Seed:* "Before reporting, audit every claim against a real result from
   this session — a file, a search, a run — and against the Done condition
   above. If it's unverified, say so."

### Dials — only to override the recipient's defaults

6. **Effort** — one honest level, with the consequence named. Never a blanket
   "maximum effort": if every task is top-of-range, the dial carries no
   signal, and it fights Scope.
   *Seed:* "This is routine — don't overthink it." / "This is hard in one
   place: [THE SUBTLE PART] is the point — spend the effort there, not on
   breadth."

7. **Autonomy** — when to act, when to pause.
   *Seed:* "When you have enough to act, act — don't re-derive what's
   established or re-litigate my decisions; weighing a choice, give a
   recommendation. Pause only for destructive actions, real scope changes, or
   input only I can give. Don't end your turn on a promise."

8. **Report** — outcome first, plus the deliverable's format.
   *Seed:* "Open with the outcome — the TLDR I'd ask for. Deliver as
   [format / file / language]. Complete sentences; clear beats short."

### Extras — only when they apply

9. **Skill** — invoke a saved skill by name. *(Drop if none exists; an
   exemplar of "good" belongs in Done, not here.)*
   *Seed:* "Apply /[skill-name] fully."

10. **Delegate** — hand independent subtasks to subagents. *(Drop for
    single-context tasks.)*
    *Seed:* "Split independent subtasks across subagents and keep working.
    Verify with a fresh-context subagent against the spec every [interval]."

11. **Memory** — surface durable lessons. *(Drop for one-offs, and for
    recipients with persistent memory of their own — Claude Code already has
    one.)*
    *Seed:* "If you learn something about me or this project that'll matter
    next time, tell me at the end so I can save it."

## Examples

### Minimal — a trivial task keeps only what earns its place

**Input:** "help me write a prompt for claude to fix the typo 'recieve' →
'receive' everywhere in my repo"

````
Task: I'm cleaning up a repo-wide misspelling. Fix every "recieve" to "receive", including case variants (Recieve/RECIEVE).

Scope: only that literal string and its case variants — don't touch other typos or reformat anything you pass over.

Done: a case-insensitive grep for "recieve" returns zero matches.

Evidence: run that grep after the replace and paste the result — report the count of files/occurrences actually changed, not an estimate.

Autonomy: go end to end; pause only if the string appears somewhere machine-facing (an identifier, API field name, URL, config key) where "fixing" it would break something.
````

Dropped: Context (the repo itself is the context), Effort/Report (recipient is
Claude Code — defaults cover them; Autonomy stays because the machine-facing
pause is a non-default), Skill, Delegate, Memory.

### Full — all 11 shown to illustrate the anatomy

**Input:** "help me write a prompt to have claude clean up the logging in my
api"

**Output:** (all 11 sections filled in to illustrate — in a real prompt, drop
whichever don't apply per steps 3–4 above)

````
Task: I'm hardening a Python API service that a small team maintains; noisy, inconsistent logging is making incidents slow to debug. With that in mind: standardize the logging across the service.

Context: use the existing logging setup in this repo (logger config, existing call sites) as the source of truth — don't introduce a new logging framework. [?] Is there a target convention already agreed (e.g. structured JSON), or should Claude infer one from the best current call sites?

Done: consistent message shape across the service, correct levels (no info-level errors), no swallowed exceptions — and the test suite still passes.

Scope: the simplest thing that works — normalize levels and messages, don't refactor the surrounding code or add config knobs I didn't ask for.

Evidence: before saying it's done, run the test suite (or the service) and check the result against Done above. If something's unverified, say so.

Effort: hard in one specific place — the subtle cases (swallowed exceptions, wrong levels) are the point. Spend the effort there, not on breadth.

Autonomy: when you have enough to act, act — if you're weighing a convention, pick one and tell me why rather than asking. Pause only if a change is destructive or genuinely changes scope.

Report: open with what changed and why, then the details.

Skill: apply /[your-logging-skill] if one is saved — otherwise Done above is the bar.

Delegate: if the call sites span many independent modules, split the sweep across subagents by module — verify each module's changes against the convention before merging.

Memory: if you spot a recurring anti-pattern (e.g. a bare `except:` block, a module that logs at the wrong level everywhere), tell me at the end — that's worth fixing at the source, not just patching here.
````

*Note:* one `[?]` to resolve — the target convention.
