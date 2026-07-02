<p align="center">
  <img src="assets/banner.svg" alt="Agentic One Piece" width="100%">
</p>

# 🏴‍☠️ Agentic One Piece

A Straw-Hat crew of [Claude Code](https://claude.com/claude-code) **agent skills** — each one a named crewmate with a job to do. Together they cover the workflow end to end: **chart** the codebase, **spec** the idea, **build** the feature, **review** the design, and **ship** it to a green PR.

---

## ⚓ The Crew

The table is in the order you'd usually use them — map the code, spec the idea, build it, check the design, ship it.

| Skill | Role | What it does |
|-------|------|--------------|
| **🎯 [usopp](skills/usopp/SKILL.md)** | Sniper — Architect | Explores the codebase and writes `MECHANICS.md` — a living map of how it all works. |
| **⚔️ [zoro](skills/zoro/SKILL.md)** | Swordsman — Spec Cutter | Turns a rough idea into a clear spec by asking the right questions, then hands it to plan mode. |
| **🧢 [luffy](skills/luffy/SKILL.md)** | Captain — Builder | Builds a planned feature end to end, reviews its own work until it's clean, checks it runs, and opens a green PR. |
| **🍳 [sanji](skills/sanji/SKILL.md)** | Cook — Design Reviewer | Reviews the UI against the project's `DESIGN.md` and gives a ship / iterate / block verdict. If there's no `DESIGN.md`, it writes one from the real design tokens first. |
| **🍊 [nami](skills/nami/SKILL.md)** | Navigator — Shipper | Takes a branch or PR to green: pushes it, opens the PR, and works through CI failures and review comments. |

---

## ⌨️ Usage

Call a crewmate by name, or just describe the task and Claude Code picks the right one.

```bash
/usopp                   # map the codebase      → MECHANICS.md
/zoro  "add CSV export"  # idea → a clear spec   → plan mode
/luffy <plan|spec>       # build + self-review   → green PR
/sanji <pr#|path>        # design-review the UI  → ship / iterate / block
/nami                    # drive a branch/PR     → green & reviewed
```

---

## 🗺️ A typical voyage

```
usopp   →  chart the codebase (MECHANICS.md)
zoro    →  cut the idea into a spec (.specs/) and hand off to plan mode
luffy   →  build the feature from a plan
sanji   →  review the design of the UI changes
nami    →  drive the PR to green and ship it
```

zoro grounds his spec in usopp's `MECHANICS.md`, and luffy already calls `sanji`, `nami`, and `/verify` internally during its review and ship phases — so for a full feature you can often just chart with `/usopp`, spec with `/zoro`, then set sail with `/luffy <plan>`.

---

## 📦 Installation

These are user-level Claude Code skills. Drop each skill folder into your skills directory:

```bash
# clone, then symlink (or copy) the crew into your Claude Code skills dir
git clone https://github.com/<you>/agentic-onepiece.git
cd agentic-onepiece

for skill in luffy nami sanji usopp zoro; do
  ln -s "$PWD/skills/$skill" "$HOME/.claude/skills/$skill"
done
```

Or copy them instead of symlinking:

```bash
cp -R skills/* "$HOME/.claude/skills/"
```

Restart Claude Code (or start a new session) and the crew will be available by name.

---

## 🚀 Why skill abstractions make coding better and faster

Asking an agent — every time you start your dev workflow — to "explore a codebase" or "fix a PR comment" or X, Y, or Z is time-consuming and error-prone. Abstractions are **easier to remember, quicker to invoke, and more consistent**, and you can **chain abstractions together** to form an agentic dev workflow end to end.

And abstractions can be anything you like or enjoy — your crew could be the Ninja Turtles, Marvel characters, or the cast of *Friends* — injecting some fun into development. **Fork and make your own crew or characters, then PR to link it in [Make your own crew](#-make-your-own-crew).** 🏴‍☠️

---

## 🤝 Make your own crew

The names are just costumes — the *roles* (architect, spec-cutter, builder, reviewer, shipper) are the point. Fork the repo, rename a `skills/` folder and its `SKILL.md` (`name`/`description`/triggers) to a crew you'll enjoy invoking — Ninja Turtles, Avengers, the *Friends* gang — then open a PR to add it below.

### Community crews

| Crew | Theme | Author |
|------|-------|--------|
| 🏴‍☠️ **[Straw Hats](#-the-crew)** | One Piece | this repo |
| _your crew here_ | — | [open a PR](https://github.com/ruzin/agentic-onepiece/pulls) |

---

<p align="center"><em>“I'm gonna be King of the Pirates!” 🏴‍☠️</em></p>
