---
name: prompt-anatomy
description: >-
  Turn a rough, one-line task into a well-structured prompt for Claude, laid out
  by the 11-part prompt anatomy (Task, Context, Skill, Effort, Act, Scope,
  Delegate, Evidence, Memory, Checkpoint, Report). Use this whenever the user
  wants help WRITING or IMPROVING a prompt/instruction they will give to Claude
  (or another agent) — phrases like "help me write a prompt", "how should I ask
  Claude to...", "draft a prompt for...", "make this prompt better", "turn this
  into a good prompt", "prompt yaz", "buna nasıl prompt yazayım". Do NOT use it
  to actually perform the underlying task — only to author the prompt for it.
---

# Prompt Anatomy

Turn a rough task into a copy-paste-ready prompt, organized by an 11-part
anatomy. The point isn't a rigid template — it's to make sure the prompt carries
the things a strong model actually needs: the *why* behind the task, where to
find context, how hard to push, when to stop, and how to report back. A model
with good theory of mind does far more with a well-framed goal than with a pile
of step-by-step commands.

## When this triggers

The user wants to *author or sharpen a prompt* they will hand to Claude — not to
have the task done. If they actually want the task done, ignore this skill and
just do it.

## Workflow

1. **Read the rough task.** Extract the real goal and who it's for. If the task
   is a single verb ("summarize this"), that's fine — you'll fill what you can
   and flag the rest.
2. **Fill the 11 sections below.** Write each from the user's point of view (the
   prompt is *theirs*, addressed to Claude). Draw concrete details from the
   conversation where they exist.
3. **Mark decisions, don't invent them.** Any section that genuinely depends on a
   choice only the user can make (effort level, target file, who the output is
   for, whether subagents are wanted) gets a `[?]` with a short inline question.
   Never fabricate context, file paths, or constraints to look complete.
4. **Drop sections that don't apply.** Not every task needs Delegate or Skill. A
   throwaway one-off doesn't need Memory. Omit a section rather than pad it — but
   say briefly why you dropped it.
5. **Return the drafted prompt in a single fenced block** so it's copy-paste
   ready, followed by a one-line note on any `[?]` the user should resolve.

## The 11 sections

Each section below gives its *intent* (what it's for) and a *seed phrasing* you
can adapt. Adapt freely — the seed is a starting point, not a script.

1. **Task** — the goal + the why, not the steps.
   *Seed:* "I'm working on [LARGER GOAL] for [WHO IT'S FOR]. They need [WHAT THE
   OUTPUT ENABLES]. With that in mind: [THE TASK]."

2. **Context** — point at where the truth lives; forbid guessing.
   *Seed:* "Use everything in [this project / these files / CLAUDE.md] first. If
   it isn't there, pull it from [source] — don't guess."

3. **Skill** — invoke a saved skill, or paste a reference of what "good" is.
   *Seed:* "Apply /[skill-name] fully. If no skill fits, here's what good looks
   like: [example]." *(Drop if no skill or exemplar is relevant.)*

4. **Effort** — set the difficulty so the model doesn't undersell itself.
   *Seed:* "This is a [routine / hard / hardest-unsolved] problem. Scope it like
   it's at the top of your range."

5. **Act** — stop over-planning; act on enough information.
   *Seed:* "When you have enough to act, act. Don't re-derive what's established,
   re-litigate my decisions, or survey options you won't pursue. Weighing a
   choice? Give a recommendation."

6. **Scope** — the simplest thing that works.
   *Seed:* "Do the simplest thing that works well. No extra features, refactors,
   abstractions, or error handling for impossible cases. If I'm describing a
   problem, the deliverable is your assessment."

7. **Delegate** — hand independent subtasks to subagents.
   *Seed:* "Split independent subtasks across subagents and keep working. Verify
   with a fresh-context subagent against the spec every [interval]." *(Drop for
   small single-context tasks.)*

8. **Evidence** — audit claims before reporting.
   *Seed:* "Before reporting, audit every claim against a real result from this
   session — a file, a search, a run. If it's unverified, say so."

9. **Memory** — surface durable lessons at the end.
   *Seed:* "If you learn something about me or this project that'll matter next
   time, tell me at the end so I can save it." *(Drop for pure one-offs.)*

10. **Checkpoint** — pause only when it truly must.
    *Seed:* "Pause only for destructive actions, real scope changes, or input
    only I can give. Otherwise go end to end. Don't end your turn on a promise."

11. **Report** — outcome first, readable prose.
    *Seed:* "Open with the outcome — the TLDR I'd ask for. Complete sentences, no
    arrow chains or shorthand. Clear beats short."

## Example

**Input:** "help me write a prompt to have claude clean up the logging in my api"

**Output:**

````
I'm hardening a Python API service that a small team maintains; noisy,
inconsistent logging is making incidents slow to debug. With that in mind:
standardize the logging across the service.

Context: use the existing logging setup in this repo (logger config, existing
call sites) as the source of truth — don't introduce a new logging framework.

Effort: routine-to-hard. Scope it like it's at the top of your range — I'd
rather you catch the subtle cases (swallowed exceptions, wrong levels).

Act: when you have enough to act, act. If you're weighing a convention, pick one
and tell me why rather than asking.

Scope: the simplest thing that works — normalize levels and messages, don't
refactor the surrounding code or add config knobs I didn't ask for.

Evidence: before saying it's done, run the test suite (or the service) and
report what you actually observed. If something's unverified, say so.

Checkpoint: pause only if a change is destructive or genuinely changes scope.

Report: open with what changed and why, then the details.
````

*Note:* I left "who it's for" implicit and dropped **Skill**, **Delegate**, and
**Memory** — a single-context cleanup doesn't need them. Set **Effort** yourself
if "routine-to-hard" is off. `[?]` Do you want the service actually run, or just
the tests?
