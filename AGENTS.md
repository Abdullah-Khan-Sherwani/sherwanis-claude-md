# AGENTS.md

Behavioral guidelines to reduce common LLM coding-agent mistakes. Works with any agent that reads
`AGENTS.md`; for agents that read their own file, symlink or copy it (`CLAUDE.md`, `GEMINI.md`, …).
Scoped `AGENTS.md` files in subfolders add to this one for their folder. Direct user instructions
take precedence. These rules favor caution over speed; for trivial tasks, use judgment.

## 0. Project Status (fill this in first)
- **Production with real users:** yes / no  <!-- edit this line; agents must read it before changing code -->
- **No:** there is nothing to stay compatible with. Don't patch around errors and bugs; fix them
  with a complete, proper refactor that leaves the code more readable and maintainable. Drop old
  interfaces, shims, fallbacks and migration paths instead of keeping them for compatibility.
- **Yes:** keep changes backward compatible, prefer small reversible fixes, and flag anything that
  could break existing users or data before making it.
- **Not recorded:** ask. Don't guess.

## 1. Think Before Coding
**Don't assume. Don't hide confusion. Surface tradeoffs.**
- State assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First
**Minimum code that solves the problem. Nothing speculative.**
- No features beyond what was asked. Replacing existing code is fine when the result is smaller and easier to maintain; remove what it supersedes.
- No abstractions for single-use code, no unrequested "flexibility", no error handling for impossible scenarios.
- Before writing anything, stop at the first answer that works: Does this need to exist at all?
  Does the codebase already have it? Does the standard library? A built-in platform feature? A
  dependency that's already installed? Only then write the minimum new code.
- Boring beats clever. Fewer files, shorter diffs, and deleting code beats adding it.
- Understand the problem fully before shrinking the solution. A small diff in the wrong place is a second bug.
- If 200 lines could be 50, rewrite. Ask: would a senior engineer call this overcomplicated?

## 3. Surgical Changes
**Touch only what you must. Clean up only your own mess.**
- Don't "improve" adjacent code, comments or formatting; match existing style.
- Mention unrelated dead code, don't delete it. Remove imports/variables/functions YOUR change orphaned.
- Every changed line traces to the request. (A pre-production refactor under section 0 counts as
  part of the request when it removes the cause of the bug being fixed.)

## 4. Goal-Driven Execution
**Define success criteria. Loop until verified.**
- "Fix the bug" = write a test that reproduces it, then make it pass. "Refactor X" = tests pass before and after.
- For multi-step tasks state a brief plan with a verify step per item.

## 5. Project Hygiene

### Code quality: DRY and SOLID
- **DRY:** every piece of knowledge (a rule, constant, query or format) lives in one place.
  Before writing logic, search for an existing version and reuse or extend it. Merge duplicates
  when you touch them. Don't abstract on the first repeat: wait for the third, or until the copies
  must change together.
- **Single responsibility:** a module, class or function has one reason to change. If describing it needs "and", split it.
- **Open/closed:** add behavior by adding a new implementation (adapter, handler, strategy), not by
  growing `if/elif` chains in shared code. Only do this once a second case actually exists.
- **Liskov substitution:** every implementation of an interface honors its full contract, with
  no surprise exceptions, no ignored arguments and no narrower inputs.
- **Interface segregation:** small, focused interfaces; callers never depend on methods they don't use.
- **Dependency inversion:** core logic depends on interfaces, not on concrete services (databases,
  APIs, file systems). Pass dependencies in so they can be swapped and faked in tests.
- No god-objects, no leaky abstractions (callers must not need to know what's underneath).
- Isolate features/modules so they can be added, changed or removed independently; no parallel structures.
- SOLID serves simplicity, not the other way round: an interface with one implementation and no
  second one coming is ceremony. Section 2 wins ties.
- Comments are a line or two on *why*; never narrate the bug, sample or topic that prompted a change.

### Library-first
- Never reinvent what a maintained library already solves; search first. Prefer well-maintained, widely-used dependencies, including for auth, queues and adapters.

### Documentation
- Every folder gets its own `AGENTS.md` (max 100 lines, this root one too) and a `docs/` subfolder; root `docs/` holds only project-wide detail.
- Docs stay concise and are updated in the same change that makes them stale.

### Reusable workflows and delegation
- If your agent has a skill, command or saved workflow for the task, use it.
- Delegate isolated, well-scoped chunks (research, parallel implementation, review) to subagents where your agent supports them.

### Secrets and environment variables
- Secrets live in environment variables only (a git-ignored `.env` locally, the host's secret store in production); never in committed files.
- Flag any possible leak immediately, even outside your task: secrets in committed files, logs,
  error messages, stack traces, test fixtures, build output, client/browser bundles, URLs or query
  strings, CI output, or text sent to third-party services, including the agent's own tool calls and
  replies. Never print, echo or paste a secret value to show that it leaked; name the variable and
  where it appears.

### Git
- Atomic commits: one commit = one change.

## Project-specific rules
<!-- Add your stack, architecture and constraints here. Keep them short. -->

---

## Credits
- Sections 1–4 are adapted from [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) (MIT),
  which is based on [Andrej Karpathy's observations on LLM coding pitfalls](https://x.com/karpathy/status/2015883857489522876).
- The "simple code" rules in section 2 take inspiration from [Ponytail](https://github.com/DietrichGebert/ponytail) by Dietrich Gebert (MIT).
- Sections 0 and 5 and the edits are by Abdullah Khan Sherwani.
