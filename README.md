```
    _   _   _ _____ _   _  ____ _____ ____  ____
   / \ | | | | ____| \ | |/ ___| ____|  _ \/ ___|
  / _ \| | | |  _| |  \| | |  _|  _| | |_) \___ \
 / ___ \ |_| | |___| |\  | |_| | |___|  _ < ___) |
/_/   \_\___/|_____|_| \_|\____|_____|_| \_\____/

              . . . A S S E M B L E D
```

An Avengers roster of [Claude Code](https://claude.com/claude-code) **agent skills** — each one a named hero with a job to do. Together they cover the workflow end to end: **chart** the codebase, **spec** the idea, **build** the feature, **review** the design, and **ship** it to a green PR.

---

## ⚡ The Roster

The table is in the order you'd usually use them — map the code, spec the idea, build it, check the design, ship it.

Every hero also answers to its **role name** — so you can call `/loki` or `/architect`, `/hulk` or `/build`, whichever you'll remember.

| Skill | Role (alias) | What it does |
|-------|------|--------------|
| **[loki](skills/loki/SKILL.md)** | Architect · `/architect` | Explores the codebase and writes `MECHANICS.md` — a living map of how it all works. |
| **[ironman](skills/ironman/SKILL.md)** | Spec Cutter · `/spec` | Turns a rough idea into a clear spec by asking the right questions, then hands it to plan mode. |
| **[hulk](skills/hulk/SKILL.md)** | Builder · `/build` | Builds a planned feature end to end, reviews its own work until it's clean, checks it runs, and opens a green PR. |
| **[widow](skills/widow/SKILL.md)** | Design Reviewer · `/design` | Reviews the UI against the project's `DESIGN.md` and gives a ship / iterate / block verdict. If there's no `DESIGN.md`, it writes one from the real design tokens first. |
| **[thor](skills/thor/SKILL.md)** | Shipper · `/ship` | Takes a branch or PR to green: pushes it, opens the PR, and works through CI failures and review comments. |
| **[assemble](skills/assemble/SKILL.md)** | Orchestrator | **Takes an idea to a merged PR autonomously** — grounds on the docs, specs it, then builds → reviews → verifies → ships the whole thing without step-by-step supervision. |

---

## ⌨️ Usage

Call a hero by name, or just describe the task and Claude Code picks the right one.

```bash
/loki      | /architect    # map the codebase      → MECHANICS.md
/ironman   | /spec         # idea → a clear spec   → plan mode
/hulk      | /build        # build + self-review   → green PR
/widow     | /design       # design-review the UI  → ship / iterate / block
/thor      | /ship         # drive a branch/PR     → green & reviewed
/assemble                  # idea → merged PR      → fully autonomous
```

Each pair is the same skill — call it by hero name or by role. Only `/assemble` has no alias.

---

## 🗺️ A typical mission

```
loki    →  chart the codebase (MECHANICS.md)
ironman →  cut the idea into a spec (.specs/) and hand off to plan mode
hulk    →  build the feature from a plan
widow   →  review the design of the UI changes
thor    →  drive the PR to green and ship it
```

ironman grounds its spec in loki's `MECHANICS.md`, and hulk already calls `widow`, `thor`, and `/verify` internally during its review and ship phases — so for a full feature you can often just chart with `/loki`, spec with `/ironman`, then build with `/hulk <plan>`.

Or hand the whole thing to **`/assemble`** — the one command that takes a raw idea to a merged PR autonomously. It grounds on the existing `MECHANICS.md` / `DESIGN.md` (no slow re-index), cuts the spec with `ironman`, then hands it to `hulk` to build → review (`widow`) → verify → ship (`thor`) — driving the mission end to end on its own:

```
assemble → ground (docs) → ironman (spec) → hulk → widow + thor → merged PR
```

---

## 📦 Installation

These are user-level Claude Code skills. Drop each skill folder into your skills directory:

```bash
# clone, then symlink (or copy) the roster into your Claude Code skills dir
git clone https://github.com/ruzin/agentic-avengers.git
cd agentic-avengers

for skill in loki ironman hulk widow thor assemble architect spec build design ship; do
  ln -s "$PWD/skills/$skill" "$HOME/.claude/skills/$skill"
done
```

Or copy them instead of symlinking:

```bash
cp -R skills/* "$HOME/.claude/skills/"
```

Restart Claude Code (or start a new session) and the roster will be available by name.

---

## 🚀 Why skill abstractions make coding better and faster

Asking an agent — every time you start your dev workflow — to "explore a codebase" or "fix a PR comment" or X, Y, or Z is time-consuming and error-prone. Abstractions are **easier to remember, quicker to invoke, and more consistent**, and you can **chain abstractions together** to form an agentic dev workflow end to end.

And abstractions can be anything you like or enjoy — your roster could be the Straw Hat Pirates, the Ninja Turtles, or the cast of *Friends* — injecting some fun into development. **Fork and make your own roster or characters.** 🦸

---

## 🤝 Make your own roster

The names are just costumes — the *roles* (architect, spec-cutter, builder, reviewer, shipper) are the point. Fork the repo, rename a `skills/` folder and its `SKILL.md` (`name`/`description`/triggers) to a crew you'll enjoy invoking — Straw Hats, Ninja Turtles, the *Friends* gang — then make it your own.

---

<p align="center"><em>“Avengers… assemble.” 🦸</em></p>
