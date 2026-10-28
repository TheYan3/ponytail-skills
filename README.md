> **Archived.** This was an early repackaging of [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) and is no longer maintained. Use the original, which has since moved to v5.

# Ponytail 🐴

**Your coding agent writes 1,000 lines where 100 would do. Ponytail makes it stop.**

Two skills for [Claude Code](https://www.anthropic.com/claude-code) that enforce
the laziest solution that actually works — plus a drop-in `CLAUDE.md` snippet if
you'd rather not install anything.

> Lazy means efficient, not careless. The best code is the code never written.

## The Problem

Coding agents love to over-build: an interface with one implementation, a config
option nobody will ever set, a hand-rolled cache class where a decorator would do,
error handling for scenarios that cannot happen. Every one of those lines is code
you now own, review, and debug at 3am.

## The Solution

### `/ponytail` — build less

Before writing any code, the agent climbs this ladder and stops at the **first
rung that holds**:

1. **Does this need to exist at all?** Speculative need = skip it. (YAGNI)
2. **Already in this codebase?** Reuse it. Look before you write.
3. **Stdlib does it?** Use it.
4. **Native platform feature covers it?** `<input type="date">` over a picker lib, CSS over JS, DB constraint over app code.
5. **Already-installed dependency solves it?** Never add a new one for what a few lines can do.
6. **Can it be one line?** One line.
7. **Only then:** the minimum code that works.

Three intensity levels. Same request — *"Add a cache for these API responses"* —
three answers:

| Level | Response |
|-------|----------|
| `lite` | "Done, cache added. FYI: `functools.lru_cache` covers this in one line if you'd rather not own a cache class." |
| `full` (default) | "`@lru_cache(maxsize=1000)` on the fetch function. Skipped custom cache class, add when lru_cache measurably falls short." |
| `ultra` | "No cache until a profiler says so. When it does: `@lru_cache`. A hand-rolled TTL cache class is a bug farm with a hit rate." |

Deliberate shortcuts are marked in the code, with the ceiling and the upgrade path
named — so "simple" reads as intent, not ignorance:

```python
# ponytail: global lock, per-account locks if throughput matters
```

### `/ponytail-review` — delete more

A review pass that hunts **only** over-engineering. One line per finding: location,
what to cut, what replaces it.

```
L12-38: stdlib: 27-line validator class. "@" in email, 1 line, real validation is the confirmation mail.
L4:     native: moment.js imported for one format call. Intl.DateTimeFormat, 0 deps.
repo.py:L88: yagni: AbstractRepository with one implementation. Inline it until a second one exists.
L52-71: delete: retry wrapper around an idempotent local call. Nothing replaces it.
L30-44: shrink: manual loop builds dict. dict(zip(keys, values)), 1 line.

net: -87 lines possible.
```

If there's nothing to cut: `Lean already. Ship.` — and it stops.

Correctness, security, and performance are explicitly out of scope: run your
normal review for those. This pass answers one question only: *what can we delete?*

## What Ponytail Never Simplifies Away

Lazy has hard limits. Never cut: input validation at trust boundaries, error
handling that prevents data loss, security measures, accessibility basics, or
anything you explicitly asked for. And non-trivial logic still leaves one runnable
check behind — lazy code without its check is unfinished.

## Install

**Option A: Claude Code plugin (recommended)**

```
/plugin marketplace add TheYan3/ponytail-skills
/plugin install ponytail@ponytail-skills
```

Then trigger with `/ponytail` (or `lazy mode`, `yagni`, or just complain about
bloat) and `/ponytail-review` (or ask *"what can we delete?"*).

**Option B: CLAUDE.md snippet (no plugin, always on)**

```bash
curl https://raw.githubusercontent.com/TheYan3/ponytail-skills/main/CLAUDE.md >> CLAUDE.md
```

This is the lightweight always-on version of the same philosophy; the skills are
the enforced version with levels and the review pass.

## How to Know It's Working

- Diffs get shorter, PRs stop growing "flexibility" nobody asked for.
- New dependencies stop appearing for things the stdlib ships.
- Reviews end with a number: `net: -N lines possible.`
- `ponytail:` comments tell you which shortcuts were chosen on purpose, and when to upgrade them.

## Escape Hatches

`stop ponytail` / `normal mode` reverts everything. Insist on the full version of
anything and it gets built — no re-arguing. The level you set persists until you
change it or the session ends.

## License

MIT
