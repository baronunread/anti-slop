# Repository guidance

- `src/` is the canonical plugin implementation.
- Keep rules generic and suitable for reuse across repositories. Do not add application-specific names, paths, or exceptions.
- Use Oxlint's ESTree API; do not add another production parser.
- Add focused RuleTester coverage for semantic rule changes.
- Run `pnpm sync:skill-assets` after changing production source.
- Run `pnpm check` before committing.

## Fork

- `main` is upstream plus fork-only changes. Fork-only skill guidance lives in `skills/install-anti-slop/references/bun.md`, so syncing touches one line of `SKILL.md`.
- Sync: `git fetch upstream && git rebase upstream/main`, then `git push --force-with-lease`. Changes meant for upstream go on a branch from `upstream/main`.
