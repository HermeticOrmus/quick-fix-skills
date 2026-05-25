# CLAUDE.md

A fast diagnostic pass for common, obvious issues. Most problems are routine and do not need a full investigation. This file is the lightweight triage layer; when it fails to resolve the issue, escalate to a real debugging protocol. Merge with project-specific instructions as needed.

**The lightweight counterpart to [hypothesis-debugging-skills](https://github.com/HermeticOrmus/hypothesis-debugging-skills)**: hypothesis-debugging is the rigorous six-phase root-cause protocol; this is the fast first pass for when that protocol is overkill.

**Tradeoff**: This file biases toward speed over rigor. It is for common, well-understood failure classes. The moment the issue stops being routine, stop and escalate.

## 1. What is broken

Pin the problem to one sentence before touching anything.

- What is not working? Name the command, file, route, or behavior.
- What did you expect? What actually happened?
- Get the exact error text. Not a paraphrase. The literal message and the line it points at.
- If there is no error, the problem is observability, not the bug. Add a log or a print and reproduce.

A vague problem statement produces a vague fix. One precise sentence is the whole first step.

## 2. What changed

Most things that break were working a moment ago. The fastest path is the diff.

- Did it ever work? If no, it was never wired correctly; this is a setup problem, not a regression.
- If yes, what changed since? A dependency bump, a config edit, an environment switch, a new branch, a pulled update.
- Check `git diff` and `git log` for the recent surface area.
- Revert the suspected change in isolation and retest. If the failure clears, you have the cause.

The question "what changed" resolves a large share of routine breakage on its own.

## 3. Most common causes for this class

Match the symptom to the usual suspect before theorizing. These cover the bulk of routine failures.

- **Dependency or version error**: wrong installed version, missing install, lockfile drift, stale `node_modules` or virtualenv. Reinstall clean and check the version actually loaded.
- **Config typo or wrong value**: misspelled key, wrong path, wrong port, value in the wrong environment file. Read the config the program actually loads, not the one you think it loads.
- **Environment variable missing or unset**: the var is set in one shell or one environment and absent in the one that runs the code. Print it from inside the failing process.
- **Path or working directory**: relative path resolved from an unexpected cwd, file not where the code expects.
- **Stale state or cache**: build cache, dist output, browser cache, old process still bound to the port. Clear it and rebuild.
- **Permissions**: file mode, ownership, missing sudo, unreadable path.

## 4. The smallest fix

Once the cause is identified, change the least possible.

- Fix the one thing. Correct the typo, set the var, bump the version, clear the cache.
- Do not add abstractions, validators, or error handling the issue did not require.
- Do not refactor adjacent code while you are in there.
- Reproduce the original failure path after the fix and confirm it now passes. A fix you did not re-run against the repro is a guess.

If the smallest fix does not resolve it, the issue is not in this class. Stop.

## 5. When to stop and escalate

Quick-fix is triage, not investigation. Escalate to a real debugging protocol when:

- Two or three quick fixes have not resolved it.
- The cause is not one of the common classes above.
- The failure is intermittent, timing-dependent, or only reproduces sometimes.
- The fix would require changing code you do not understand.
- "What changed" turns up nothing and the symptom is genuinely novel.

Escalate to [hypothesis-debugging-skills](https://github.com/HermeticOrmus/hypothesis-debugging-skills), the six-phase root-cause protocol (reproduce, isolate, hypothesize, instrument, fix, verify). Quick-fix front-loads the cheap checks; hypothesis-debugging handles the cases the cheap checks cannot.

---

## Use this triage when

- The symptom matches a common class (dependency, config, env var, path, stale state, permissions)
- The issue is fresh and "what changed" has an obvious answer
- A full root-cause investigation would be more ceremony than the bug warrants

## Escalate when

- The fast pass fails twice or three times → switch to hypothesis-debugging
- The bug is intermittent or its cause is not in the common-class list → switch to hypothesis-debugging
- The fix touches code whose behavior is unclear → switch to hypothesis-debugging

---

**License**: MIT. Use it, fork it, merge it into your own CLAUDE.md.
