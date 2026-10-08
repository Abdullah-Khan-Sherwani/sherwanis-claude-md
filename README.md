<div align="center">

# Sherwani's CLAUDE.md

**One small file that teaches your AI coding agent some manners.**

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#come-build-this-with-me)
[![Format: AGENTS.md](https://img.shields.io/badge/format-AGENTS.md%20%2B%20CLAUDE.md-informational.svg)](https://agents.md)
[![GitHub stars](https://img.shields.io/github/stars/Abdullah-Khan-Sherwani/sherwanis-claude-md?logo=github)](https://github.com/Abdullah-Khan-Sherwani/sherwanis-claude-md/stargazers)

<sub>About 100 lines · no dependencies · no install · no build</sub>

</div>

---

## Hello!

Your coding agent is quick, cheerful and eager to please. It is also the colleague who "just tidied up" 40 files you never mentioned, pulled in a new dependency for a one-liner, and announced "all done!" without running anything.

This file has a word with it. Drop it into your project and the agent asks when it's unsure, builds what you requested, leaves unrelated code alone, and proves the work before calling it finished. You review the change you asked for, and that's it. :)

## The usual suspects

Coding agents fail in predictable ways. Here's the lineup:

| The habit | What lands in your review queue |
|---|---|
| Silent assumptions | Your request had two readings. The agent picked one and never mentioned the other. |
| Over-engineering | 300 lines and a new dependency where 10 lines of existing code would do. |
| Scope creep | Drive-by refactors, reformatted files, and comments deleted from code nobody asked about. |
| Unverified "done" | No test reproduced the bug, so nobody knows the fix works. |
| Wrong risk posture | Careful compatibility shims in a project with no users, or breaking changes in one with many. |
| Leaked secrets | Tokens in logs, fixtures, bundles or the agent's own replies. |

Each one gets a short, explicit rule.

## The rules, at a glance

| Section | The gist |
|---|---|
| **0. Project Status** | You say whether the project has real users. Pre-production: fix root causes properly and drop old shims. Production: small, backward-compatible, reversible changes, with anything risky flagged first. Unset: the agent asks. |
| **1. Think Before Coding** | State assumptions, lay out alternatives instead of picking silently, push back, and stop to ask when something is unclear. |
| **2. Simplicity First** | Climb a ladder before writing code. Does it need to exist? Is it already in the codebase? Does the standard library or the platform cover it? Is a dependency already installed? Only then write the minimum. |
| **3. Surgical Changes** | Every changed line traces back to the request. Match the existing style. Clean up only what your own change orphaned, and mention unrelated dead code instead of deleting it. |
| **4. Goal-Driven Execution** | Turn tasks into checkable goals: reproduce the bug with a test, then make it pass. Multi-step work gets a short plan with a verify step per item. |
| **5. Project Hygiene** | DRY and SOLID (simplicity wins ties), library-first, an `AGENTS.md` and `docs/` per folder, secrets only in environment variables, atomic commits. |

The full text lives in [`AGENTS.md`](AGENTS.md). It takes about two minutes to read.

## Quick start

1. Copy [`AGENTS.md`](AGENTS.md) into the root of your project.
2. Fill in section 0 (details just below).
3. Using Claude Code? Make a `CLAUDE.md` that imports it:

   ```bash
   echo "@AGENTS.md" > CLAUDE.md
   ```

   Already have a `CLAUDE.md`? Add `@AGENTS.md` as a line in it instead.

That's the whole install. If you'd rather grab the file from the terminal:

```bash
curl -O https://raw.githubusercontent.com/Abdullah-Khan-Sherwani/sherwanis-claude-md/main/AGENTS.md
```

## Section 0: tell the agent if you're in production

> [!IMPORTANT]
> This is the one setting worth your attention. It changes how the agent fixes things.

Open `AGENTS.md` and edit this line:

```markdown
- **Production with real users:** yes / no
```

Replace `yes / no` with `yes` or `no`:

| Setting | What the agent does |
|---|---|
| `no` | Nothing to stay compatible with, so it fixes the cause with a complete refactor and removes old interfaces, fallbacks and migration paths. |
| `yes` | Keeps changes backward compatible, prefers small reversible fixes, and warns you before anything that could break users or data. |
| left as `yes / no` | It asks. It does not guess. |

Leaving it unedited is safe, you'll just get asked. Set it once, and revisit it the day you launch.

## Works with

| Tool | How to use it |
|---|---|
| Claude Code | Create `CLAUDE.md` containing `@AGENTS.md`. |
| Tools that read `AGENTS.md` natively | Copy `AGENTS.md` to the project root. The [agents.md](https://agents.md) site lists the format and supported tools. |
| Tools with their own filename (for example `GEMINI.md`) | Copy or symlink `AGENTS.md` to that name, or point the tool's setting at it. |

## Make it yours

Add your stack, architecture and hard constraints under **Project-specific rules** at the bottom of `AGENTS.md`. Keep that part short. The file is loaded into the agent's context on every turn, so each extra line costs tokens and dilutes the rules around it.

Section 5 asks for a scoped `AGENTS.md` in each folder. Scoped files add to the root file for their folder and never replace it.

## Come build this with me

This repo is small on purpose, which makes it a friendly place for a first open source contribution. There's no toolchain to set up and no test suite to wrestle. You need a text editor and an opinion about how agents should behave.

**Ways to jump in**

- **Tell me where a rule failed.** If an agent ignored a rule, misread it or got annoying about it, open an issue with what you asked, what the agent did, and which tool and model you used. Reports like this are the most useful thing anyone can send.
- **Share a rule that saved your afternoon.** Propose it in an issue or a PR, and tell me what mistake it prevents.
- **Add your tool.** Using something that isn't in the table above? Describe how you wired it up and add a row.
- **Make the words better.** Clearer wording, a sharper example or a typo fix are all welcome. Small PRs are my favorite.

**House rules**

1. `AGENTS.md` stays under 100 lines, and it's close to that cap today. A new rule usually means trimming an old one.
2. Every line is read on every agent turn, so make each one earn its place.
3. One change per PR, with a sentence on the mistake it prevents.
4. Match the file's voice: short, direct, imperative.
5. If you change a rule, update this README in the same PR.

**How to send a PR**

1. Fork the repo and clone your fork.
2. Make a branch: `git switch -c my-tweak`
3. Edit, commit, push.
4. Open a pull request and say hello.

Everyone is still figuring out how to work with agents, so disagreement is welcome and rudeness isn't. Be kind, and we'll get along fine.

Enjoying the file? A star on the repo helps other people find it ;)

## FAQ

**Will this slow the agent down on small tasks?**
Hardly. The rules favor caution over speed, and the file says so. For trivial changes like a typo or an obvious one-liner, the agent is told to use judgment instead of running the full checklist.

**Why does the file say "AGENTS.md" but the repo says "CLAUDE.md"?**
Claude Code reads `CLAUDE.md`, and many other tools read `AGENTS.md`. The rules live once in `AGENTS.md` and `CLAUDE.md` imports them, so there's a single copy to maintain.

**Do the rules clash with my existing instructions?**
Your direct instructions win, and scoped `AGENTS.md` files extend the root file for their folder. If a rule doesn't fit your project, edit it or delete it. It's your file now.

**Why is simplicity ranked above SOLID?**
SOLID applied mechanically gives you interfaces with one implementation and layers nobody needs. Section 5 says simplicity wins ties.

## Credits

Standing on friendly shoulders:

- Sections 1-4 are adapted from [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills), which distills [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls.
- The decision ladder in section 2 is inspired by [Ponytail](https://github.com/DietrichGebert/ponytail) by Dietrich Gebert.
- Sections 0 and 5, plus the edits, are by Abdullah Khan Sherwani.

Full attribution and the upstream license text are in [NOTICE](NOTICE).

## License

[MIT](LICENSE) © 2026 Abdullah Khan Sherwani

<div align="center">

Happy shipping! :D

</div>
