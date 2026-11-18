# Toolbox — TODO

> Work inside each utility is tracked in that utility's own repository. This file only
> tracks work that belongs to the Toolbox collection itself.

## Active

- [x] `P3` `testing` `@ai` **Code-level readiness audit (promises vs code)** of the three
  utilities. Done 2026-11-18: summary in `docs/audit-2026-11-18.md`; findings are in each utility's own
  `TODO.md`. `unblock_tracker` and `image_tools` not ready, `convert_epub` ready for daily use.
- [ ] `P3` `feature` `@ai` **Decide whether Toolbox needs its own launcher.** The utilities
  are currently reached through Lab Hub or run directly; build a separate launcher only
  if that adds value beyond Lab Hub's Tools tab.
- [ ] `P3` `docs` `@ai` Re-check the README's Layout section whenever an autosync submodule bump changes a member's files or hardcoded paths — the Sep–Nov 2026 bumps went undocumented here.

## Done

- [x] `P1` `infra` `@ai` Keep `unblock_tracker`, `convert_epub`, and `image_tools` as
  independent nested repositories/submodules rather than merging their histories.
- [x] `P2` `docs` `@ai` Document recursive clone/submodule initialization in the README.
- [x] `P2` `docs` `@ai` README Layout section maps every member's files, what each `image_tools` script does, and the paths currently hardcoded in them (Nov 2026).
