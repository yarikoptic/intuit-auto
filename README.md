# Test branch for intuit/auto#1294 (`spawnSync /bin/sh E2BIG`)

This orphan branch has nothing to do with the rest of this fork; it only
exists to reproduce https://github.com/intuit/auto/issues/1294 and to check
the fix proposed in https://github.com/yarikoptic/intuit-auto/pull/1.

- `.autorc` uses the `exec` plugin with an `afterRelease` hook.
- The `v0.0.0` tag marks the root commit; on top of it come 150 commits
  with long messages, so the JSON of `afterRelease`'s argument (`ARG_0`)
  exceeds Linux's 128KiB limit for a single environment string
  (`MAX_ARG_STRLEN`).
- `.github/workflows/e2big.yml` (run on every push to this branch) runs
  `auto release --from v0.0.0` (as a GitHub prerelease) with
  - `upstream`: the latest released auto binary -- expected to fail with E2BIG;
  - `patched`: auto built from the `claude/stoic-cerf-o8sk1n` branch -- expected
    to pass, with the hook reading the argument from `$ARG_0_FILE`.
