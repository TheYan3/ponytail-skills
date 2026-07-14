# Working Philosophy: Lean by Default

<!-- Drop this into your project's CLAUDE.md (or append to it) if you don't
     want the plugin. The skills in this repo are the enforced version;
     this is the always-on lightweight version. -->

- Default is **lean**: shortest path to a working result, YAGNI, no speculative
  abstractions, no mandatory process (no forced TDD, no coverage quotas, no
  planning documents) — unless explicitly requested.
- Before any code, climb the ladder and stop at the first rung that holds:
  1. Does this need to exist at all? Speculative need = skip it, say so in one line.
  2. Already in this codebase? Reuse it. Look before you write.
  3. Stdlib does it? Use it.
  4. Native platform feature covers it? (`<input type="date">` over a picker lib,
     CSS over JS, DB constraint over app code.)
  5. Already-installed dependency solves it? Never add a new one for what a few
     lines can do.
  6. Can it be one line? One line.
  7. Only then: the minimum code that works.
- No unrequested abstractions: no interface with one implementation, no factory
  for one product, no config for a value that never changes.
- Deletion over addition. Boring over clever. Fewest files, shortest working diff.
- Mark deliberate shortcuts with a `// ponytail:` comment naming the ceiling and
  the upgrade path: `# ponytail: global lock, per-account locks if throughput matters`.
- Never simplify away: input validation at trust boundaries, error handling that
  prevents data loss, security measures, accessibility basics, anything
  explicitly requested.
- If a skill, plugin, or imported rule contradicts this philosophy, this file wins.
