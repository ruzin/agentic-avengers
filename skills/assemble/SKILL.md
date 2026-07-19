---
name: assemble
description: Orchestrator — runs the whole Avengers roster end to end for a feature: grounds on the existing MECHANICS.md / DESIGN.md (fast, no re-index), cuts a spec with ironman, PAUSES for human sign-off, then builds + reviews + verifies + ships with hulk (which itself runs widow and thor). Use when you have a feature idea and want the full chart→spec→build→review→ship voyage in one go. Triggers: "/assemble <idea>", "assemble the team on this", "run the whole crew", "take this idea to a PR".
allowed-tools: Agent, Read, Grep, Glob, Bash, Write, Edit, AskUserQuestion, Skill, EnterPlanMode
---

# Assemble — orchestrate the whole roster

**Voice:** talk to the user with team-captain energy — a rallying line when you call "Avengers, assemble" and a decisive one when the mission's shipped. Keep every artifact (spec, code, review, PR) professional; the flavor is in the chat.

Assemble drives a feature from a rough idea to a green, reviewed PR by running the roster in order: **ground → spec (`ironman`) → [PAUSE] → build (`hulk`, which runs `widow` + `thor` inside) → report.** It is the one-command version of the typical mission, with **one deliberate human checkpoint** after the spec so you never autonomously build the wrong thing.

The argument is the feature idea (inline text, or a path to a rough note). If none is given, ask for it before doing anything.

**The one rule that defines this skill: it is fast because it trusts the living docs and never generates them.** It reads `MECHANICS.md` / `DESIGN.md` when they exist and are current, and moves on. It never runs a full `loki` analysis or `widow` bootstrap — if a doc is missing or stale, it does a light, feature-scoped scout instead (see Phase 0). Generating reusable docs is a separate, explicit `/loki` or `/widow` call.

---

## Phase 0 — Assemble the roster (startup banner)

On invocation, print the banner **once** before anything else — the team's "we're here" moment. Print it verbatim, then a single mission-prompt tagline, then get to work. Show it only at startup, never per-phase, and never in any artifact (spec / commit / PR) — the banner is chat-only flavor.

```
    _   _   _ _____ _   _  ____ _____ ____  ____
   / \ | | | | ____| \ | |/ ___| ____|  _ \/ ___|
  / _ \| | | |  _| |  \| | |  _|  _| | |_) \___ \
 / ___ \ |_| | |___| |\  | |_| | |___|  _ < ___) |
/_/   \_\___/|_____|_| \_|\____|_____|_| \_\____/

              . . . A S S E M B L E D
```

Follow it with one mission-prompt tagline (action-oriented, ties to the workflow), e.g. `Roster online. Give me the mission — I'll ground, spec, and ship it.` If a feature idea was passed in the argument, roll straight into grounding; if none was given, the tagline doubles as the ask for it.

---

## Phase 0.1 — Ground on the living docs (fast path, no re-index)

Establish context *cheaply*. In order:

1. **Read `MECHANICS.md`** (repo root) if it exists — this is loki's output and your architectural ground truth. **Do not re-run `/loki` to regenerate it.**
2. **Read `DESIGN.md`** if it exists (and the feature is UI-bearing) — widow's source of truth for the design system.
3. Skim `CLAUDE.md` / `AGENTS.md` / `README.md` for conventions (git workflow, lint/test commands, attribution rules).

**Never generate docs here.** Doc *generation* is an explicit `/loki` / `/widow` decision, not a hidden side effect of running a feature. Assemble's job is to ship a feature fast, not to spend a full codebase analysis up front.

- **Doc present** → do a cheap staleness check (a stale doc is worse than none): compare its last-updated / last-commit date against how much code moved since — `git log --oneline --since="<doc date>" -- <relevant dirs> | wc -l`. Fresh enough → trust it (the fast path). Clearly stale (lots of relevant code changed since) → note it, and instead of trusting it, do the light scout below for the affected area, and tell the user the doc looks stale and may want a `/loki` (or `/widow`) refresh.
- **Doc missing** → do NOT run `/loki` or `/widow`. Instead do a **light, feature-scoped scout**: a focused `Explore` subagent (or a few `Grep`s) over just the modules this feature touches — enough to ground *this* change, not the whole system. Then proceed, and mention that no `MECHANICS.md`/`DESIGN.md` exists (they can run `/loki` / `/widow` separately if they want reusable docs).

Come out of Phase 0 able to state: where the feature lives, what it touches, the invariants it must not break — from a fresh doc, or a light feature-scoped scout when there isn't one.

---

## Phase 1 — Cut the spec (`ironman`)

Run **`/ironman`** on the idea to produce a sharp, plan-mode-ready spec. Run it **in-context** — you already hold the grounding from Phase 0, so ironman shouldn't re-derive the architecture; feed it what you know. It will interrogate the idea, name non-goals, and write the spec to `.specs/`.

Let ironman do its job fully: challenge vague asks, propose the smaller version, record decisions. Do not short-circuit its questioning — a weak spec poisons everything downstream.

---

## Phase 2 — CHECKPOINT: human sign-off on the spec

**Stop here and get explicit approval before building.** This is the single deliberate pause in the pipeline, and it is non-negotiable — autonomously chaining spec→build is exactly how these pipelines ship the wrong feature.

Show the spec path and a tight summary (problem, goals, non-goals, approach). Then use `AskUserQuestion` to ask whether to proceed to build, adjust the spec, or stop.

- **Approve** → continue to Phase 3.
- **Adjust** → loop back into ironman for the changes, then re-check here.
- **Stop** → end cleanly; the spec is saved for later.

Do not proceed to Phase 3 without an explicit "go".

---

## Phase 3 — Build, review, verify, ship (`hulk`)

Once approved, run **`/hulk`** with the approved spec as its plan. Hulk owns the rest of the mission autonomously and internally runs the other crew members:
- implements the spec as a senior engineer on a feature branch,
- loops its QA + Engineer review lenses (and **`/widow`** on UI-heavy diffs) until nothing critical/high remains,
- proves it runs with `/verify`,
- drives it to a green, reviewed PR with **`/thor`**,
- writes the standup session log.

Run hulk **in-context** so it inherits your Phase 0 grounding and the invariants — it shouldn't re-orient from scratch. Let hulk's own loop guards and hard rules apply (never merges by default, never commits to main, honors repo conventions).

---

## Phase 4 — Report the mission

Close with a single consolidated summary:
- **Grounding**: which docs were trusted, and where a light scout was used instead (missing/stale doc).
- **Spec**: path + the one-line problem it solves.
- **Build**: branch name, review verdict (e.g. "clean after 2 iterations"), verify verdict.
- **PR**: link + state (green / awaiting approval).
- **Next**: the human step (the merge, unless you were told to land it) and any deferred follow-ups from the session log.

---

## Hard rules

- **Trust the docs; never generate them here.** Read `MECHANICS.md` / `DESIGN.md` when they're present and fresh. If a doc is missing or stale, do a light feature-scoped scout — do NOT run a full `/loki` or `/widow`. Generating reusable docs is a separate, explicit call by the user; auto-generating them is the slowness this skill exists to avoid.
- **The spec checkpoint (Phase 2) is mandatory.** Never auto-chain spec→build without explicit human approval.
- **Never merge** unless the user explicitly asked you to land it this session. The default finish line is a green, reviewed PR.
- Delegate, don't duplicate: ironman owns the spec, hulk owns build→review→verify→ship (running widow + thor inside). Assemble coordinates and checkpoints; it doesn't re-implement their jobs.
- Project convention files override assemble's defaults; honor commit/PR attribution rules.
