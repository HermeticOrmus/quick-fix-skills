# Examples

Three short walkthroughs. Two resolve fast on the triage pass. The third does not, and shows the handoff to a real debugging protocol.

---

## 1. A dependency or version error, resolved fast

**Scenario**: `npm run build` fails with `SyntaxError: Unexpected token '??='` pointing at a file inside `node_modules/some-lib`.

**Triage**:

1. **What is broken**: the build, with a syntax error inside a dependency, on the nullish-assignment operator.
2. **What changed**: it built yesterday. `git log` shows a `package.json` bump of `some-lib` from `2.3.0` to `3.0.0` pulled this morning.
3. **Common cause for this class**: a version error. The `??=` operator needs a newer Node than the one running the build. Check the actually-loaded version.

```bash
node --version
# v14.18.0
cat some-lib/package.json | grep '"engines"'
# "engines": { "node": ">=16" }
```

The newly bumped `some-lib@3.0.0` requires Node 16+; the build runs on Node 14.

4. **The smallest fix**: switch the running Node, not the library. The version bump was intended.

```bash
nvm use 18
npm run build
```

Build passes. One question ("what changed") plus one version check found it. No investigation needed.

---

## 2. A config typo, resolved fast

**Scenario**: the app boots but every request to the database times out. No code changed.

**Triage**:

1. **What is broken**: all DB queries time out at runtime. The app starts fine.
2. **What changed**: a teammate edited `.env` to rotate the database host.
3. **Common cause for this class**: a config typo. Read the value the program actually loads.

```bash
grep DB_HOST .env
# DB_HOST=db.internal.prod.example.cmo
```

`.cmo` should be `.com`. The host never resolves, so the connection hangs until timeout.

4. **The smallest fix**: correct the one character. Do not add retry logic or a connection validator; the issue did not call for it.

```bash
# .env
# DB_HOST=db.internal.prod.example.com
```

Restart, reproduce the original request path, queries return. The smallest possible change against a precisely located typo.

---

## 3. When the fast pass fails, escalate

**Scenario**: the checkout endpoint returns a 500 roughly one request in twenty. The rest succeed. No deploy, no config change.

**Triage attempt**:

1. **What is broken**: `POST /checkout` returns 500 intermittently, about 5 percent of calls.
2. **What changed**: `git log` shows nothing relevant; it has behaved this way across the last several deploys.
3. **Common causes for this class**: none fit cleanly. It is not a version error (most calls succeed), not a config typo (config is static and the failure is sporadic), not an unset env var (the var would fail every call, not one in twenty).

The triage signals all point the same way: the failure is intermittent, the cause is not in the common-class list, and "what changed" turned up nothing. These are the explicit escalation triggers.

**Escalate** to [hypothesis-debugging-skills](https://github.com/HermeticOrmus/hypothesis-debugging-skills). The six-phase protocol takes it from here: reliably reproduce the intermittent 500 (likely a race or a pool-exhaustion condition under concurrency), isolate the failing path, form and instrument a hypothesis, fix the root cause, and verify the intermittency is gone.

Quick-fix did its job by failing fast and handing off cleanly. The wrong move would have been to keep guessing at a sporadic failure with cheap checks that were never going to catch it.

---

## A note on scope

Quick-fix front-loads the cheap checks that resolve the bulk of routine breakage. Its discipline is twofold: reach for the usual suspect before theorizing, and recognize quickly when the issue is not routine. A fast pass that knows when to stop is worth more than a fast pass that keeps guessing.

## Further reading

- [`hypothesis-debugging-skills`](https://github.com/HermeticOrmus/hypothesis-debugging-skills): the rigorous six-phase root-cause protocol that quick-fix escalates to.
