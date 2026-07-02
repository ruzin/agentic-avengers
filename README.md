<p align="center">
  <img src="assets/banner.svg" alt="Agentic One Piece" width="100%">
</p>

# 🏴‍☠️ Agentic One Piece

A Straw-Hat crew of [Claude Code](https://claude.com/claude-code) **agent skills** — each one a named crewmate with a job to do. Hand the crew a plan and they'll build it, navigate it to a green PR, plate the design, and chart the codebase.

Every skill is a self-contained `SKILL.md`. Claude Code loads them automatically and invokes them by name (e.g. `/luffy`, `/nami`) or when a task matches the skill's description.

---

## 🤔 Why skill abstractions make coding better and faster

Asking an agent — every time you start your dev workflow — to "explore a codebase" or "fix a PR comment" or X, Y, or Z is time-consuming and error-prone. Abstractions are **easier to remember, quicker to invoke, and more consistent**, and you can **chain abstractions together** to form an agentic dev workflow end to end.

And abstractions can be anything you like or enjoy — your crew could be the Ninja Turtles, Marvel characters, or the cast of *Friends* — injecting some fun into development. **Fork and make your own crew or characters, then PR to link it in [Make your own crew](#-make-your-own-crew).** 🏴‍☠️

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

### 🎯 usopp — *codebase architect*

usopp explores a codebase to learn how it works — its architecture, key decisions, and moving parts — and writes it all down in **`MECHANICS.md`**, a living map anyone can read to get up to speed fast. The first run creates the doc; later runs read it and update only what has changed.

**Triggers:** `/usopp` · "document the architecture" · "update MECHANICS.md"

### ⚔️ zoro — *spec cutter*

zoro turns a rough idea into a clear, **plan-ready spec**. He asks what you want to build, reads the codebase (using `MECHANICS.md` / `DESIGN.md` when they exist), then asks the right questions in a few short rounds — pushing back on vague asks and pointing out what's out of scope. Once you approve, he saves the spec to a gitignored `.specs/` folder and hands it straight to plan mode. Zoro decides the *what and why*; plan mode works out the *how*.

**Triggers:** `/zoro` · "spec this out" · "help me write a spec" · "turn this into a spec for plan mode"

### 🧢 luffy — *autonomous feature builder*

Hand luffy a plan or spec (a file or a quick description) and he builds the feature like a senior engineer, reviews his own work through a **QA + Design + Engineer** loop until it's clean, proves it actually runs with `/verify`, takes it to a green reviewed PR via `/nami`, and ends with a short session log.

**Triggers:** `/luffy <plan>` · "run luffy on this plan" · "autonomously implement this feature" · "build this and review it until clean"

### 🍳 sanji — *senior design reviewer*

sanji reviews UI work — a PR, a diff, or a component — like a senior product designer. It checks the design against the repo's own **`DESIGN.md`** and returns clear findings with a **ship / iterate / block** verdict. If the repo has no `DESIGN.md`, it writes one first — reverse-engineering the real design tokens into the [Google `DESIGN.md`](https://github.com/google-labs-code/design.md) format (just like usopp owns `MECHANICS.md`) — then reviews against it. Design only; it leaves logic bugs to `/code-review` and luffy.

**Triggers:** `/sanji <pr#|path>` · "review the design" · "sanji review this PR"

### 🍊 nami — *navigator to a green PR*

nami takes the helm on a branch or PR. She pushes it, opens or updates the PR, then works the two things that come back — **CI results** and **review comments** (bots and humans) — fixing what's real and pushing back on what's wrong, until checks are green and nothing blocking is left. luffy also calls her during its ship phase.

**Triggers:** `/nami` · "take this to a green PR" · "get CI green" · "address the PR comments" · "babysit this PR till it's green"

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

## 📁 Repository layout

```
agentic-onepiece/
├── README.md
├── assets/
│   └── banner.svg
└── skills/
    ├── luffy/SKILL.md
    ├── nami/SKILL.md
    ├── sanji/SKILL.md
    ├── usopp/SKILL.md
    └── zoro/SKILL.md
```

---

## 🤝 Make your own crew

The crew is just an abstraction — the *names* are yours to choose. Prefer the Ninja Turtles, the Avengers, or the *Friends* gang? Fork the repo and rename the skills to a crew you'll actually enjoy invoking.

1. **Fork** this repo.
2. **Copy a skill folder** under `skills/` and rename it to your character (e.g. `skills/leonardo/SKILL.md`).
3. **Edit the `SKILL.md`** — update the `name`, `description`, and triggers so Claude Code invokes it by your character's name.
4. **Keep the job, swap the costume** — a skill's *role* (builder, shipper, design reviewer, architect) is what matters; the persona is flavour.
5. **Open a PR** to add your crew to the table below.

### Community crews

| Crew | Theme | Author |
|------|-------|--------|
| 🏴‍☠️ **[Straw Hats](#-the-crew)** | One Piece | this repo |
| _your crew here_ | — | [open a PR](https://github.com/ruzin/agentic-onepiece/pulls) |

---

<p align="center"><em>“I'm gonna be King of the Pirates!” 🏴‍☠️</em></p>
