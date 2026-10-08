# Sherwani's CLAUDE.md

![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![Format: AGENTS.md](https://img.shields.io/badge/format-AGENTS.md%20%2B%20CLAUDE.md-informational.svg)

A drop-in instruction file that stops AI coding agents from making the mistakes that cost you the most review time: guessing instead of asking, over-building, editing code they weren't asked to touch, and declaring work done without checking it.

One file (~100 lines). No dependencies, no install step, no build.

## Table of contents

- [Why this exists](#why-this-exists)
- [What it enforces](#what-it-enforces)
- [Quick start](#quick-start)
- [Section 0: tell the agent if you're in production](#section-0-tell-the-agent-if-youre-in-production)
- [Compatibility](#compatibility)
- [Customizing](#customizing)
- [FAQ](#faq)
- [Credits](#credits)
- [License](#license)

## Why this exists

Coding agents are fast, and they fail in predictable ways:

| Failure | What it looks like in review |
|---|---|
| Silent assumptions | The agent picks one reading of an ambiguous request and builds on it. |
| Over-engineering | 300 lines and a new dependency where 10 lines of existing code would do. |
| Scope creep | Drive-by refactors, reformatted files, deleted comments unrelated to the task. |
| Unverified "done" | No test reproduced the bug, so nobody knows the fix works. |
| Wrong risk posture | Careful backward-compatibility shims in a project with no users, or breaking changes in one with many. |
| Leaked secrets | Tokens in logs, fixtures, bundles or the agent's own replies. |

This file gives the agent a short, explicit rule for each of these, so you review the change you asked for.

## What it enforces

| Section | Rule |
|---|---|
| **0. Project Status** | You state whether the project has real users. Pre-production: fix root causes with a proper refactor and drop old shims. Production: small, backward-compatible, reversible changes, with anything risky flagged first. Unset: the agent asks. |
| **1. Think Before Coding** | State assumptions, present alternatives instead of picking silently, push back, and stop to ask when something is unclear. |
| **2. Simplicity First** | Climb a ladder before writing code: does it need to exist, is it already in the codebase, does the standard library or the platform cover it, is a dependency already installed. Only then write the minimum. |
| **3. Surgical Changes** | Every changed line traces to the request. Match existing style. Clean up only what your own change orphaned; mention unrelated dead code instead of deleting it. |
| **4. Goal-Driven Execution** | Turn tasks into checkable goals: reproduce a bug with a test, then make it pass. Multi-step work gets a short plan with a verify step per item. |
| **5. Project Hygiene** | DRY and SOLID (with a tie-breaker: simplicity wins), library-first, an `AGENTS.md` and `docs/` per folder, secrets only in environment variables, atomic commits. |

The full text is in [`AGENTS.md`](AGENTS.md).

## Quick start

1. Copy [`AGENTS.md`](AGENTS.md) into the root of your project.
2. Fill in section 0 (see below).
3. If you use Claude Code, create a `CLAUDE.md` that imports it:

   ```bash
   echo "@AGENTS.md" > CLAUDE.md
   ```

   Already have a `CLAUDE.md`? Add `@AGENTS.md` as a line in it instead.

To fetch the file directly:

```bash
curl -O https://raw.githubusercontent.com/Abdullah-Khan-Sherwani/sherwanis-claude-md/main/AGENTS.md
```

## Section 0: tell the agent if you're in production

Open `AGENTS.md` and edit this line:

```markdown
- **Production with real users:** yes / no
```

Replace `yes / no` with `yes` or `no`. This one setting changes how the agent fixes problems:

| Setting | Agent behavior |
|---|---|
| `no` | Nothing to stay compatible with, so it fixes the cause with a complete refactor, and removes old interfaces, fallbacks and migration paths. |
| `yes` | Keeps changes backward compatible, prefers small reversible fixes, and warns you before anything that could break users or data. |
| left as `yes / no` | The agent asks. It does not guess. |

Leaving the line unedited is safe, but you will be asked the question. Set it once and revisit it when you launch.

## Compatibility

| Tool | How to use |
|---|---|
| Claude Code | Create `CLAUDE.md` containing `@AGENTS.md`. |
| Tools that read `AGENTS.md` natively | Copy `AGENTS.md` to the project root. See [agents.md](https://agents.md) for the format and supported tools. |
| Tools with their own filename (for example `GEMINI.md`) | Copy or symlink `AGENTS.md` to that name, or use the tool's setting for an alternate instruction file. |

## Customizing

Add your stack, architecture and hard constraints under **Project-specific rules** at the bottom of `AGENTS.md`. Keep that section short. The file is loaded into the agent's context on every turn, so each extra line costs tokens and dilutes the rules around it.

Section 5 asks for a scoped `AGENTS.md` in each folder. Scoped files add to the root file for their folder and do not replace it.

## FAQ

**Will this slow the agent down on small tasks?**
The rules favor caution over speed, and the file says so: for trivial changes such as a typo or an obvious one-liner, the agent is told to use judgment instead of running the full checklist.

**Why does the file say "AGENTS.md" but the repo says "CLAUDE.md"?**
Claude Code reads `CLAUDE.md`, and many other tools read `AGENTS.md`. The rules live in `AGENTS.md` once, and `CLAUDE.md` imports them, so there is a single copy to maintain.

**Do the rules conflict with my existing instructions?**
Direct instructions from you take precedence, and scoped `AGENTS.md` files extend the root file for their folder. If a rule in this file doesn't fit your project, edit or delete it.

**Why is simplicity ranked above SOLID?**
Because SOLID applied mechanically produces interfaces with one implementation and layers nobody needs. Section 5 says simplicity wins ties.

## Credits

Sections 1-4 are adapted from [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills), which distills [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls. The decision ladder in section 2 is inspired by [Ponytail](https://github.com/DietrichGebert/ponytail) by Dietrich Gebert. Sections 0 and 5 and the edits are by Abdullah Khan Sherwani. Full attribution and upstream license text are in [NOTICE](NOTICE).

## License

[MIT](LICENSE) © 2026 Abdullah Khan Sherwani
