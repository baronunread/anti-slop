# Bun and local conventions

Fork-only additions. They replace the matching commands in `SKILL.md` and `update.md`; every other step still applies.

## Commands

- Run the installer with Bun, both for a fresh install and for the staging step of an update: `bun <skill-directory>/scripts/install.mjs [dest]`. This machine has no global `node` on Homebrew's path; Bun runs the script unchanged.
- Versions: `bun pm view oxlint version` and `bun pm view @oxlint/plugins version`.
- Install: `bun add -d -E oxlint@<v> @oxlint/plugins@<v>`, same version for both. Use the repository's package manager instead when it is not Bun.

## Formatter ignores with bun fmt

1. Run `bun fmt --doctor`. Its first line names the config bun fmt reads (`.oxfmtrc.json`, a Prettier config, or `bunfig.toml`). Put the ignore patterns from `SKILL.md` there; with no config, add `[fmt]` `ignorePatterns = [...]` to `bunfig.toml`. Keep one list per repository.
2. Prove it: `bun fmt --check <plugin path>/index.ts` must exit 2 with "All matched files may have been excluded by ignore rules". Exit 0 or 1 means the plugin is still being formatted.
3. A hook running `bun fmt {staged_files}` needs the same paths in its `exclude`: bun fmt exits 2 when every file it is given is ignored.

## require-readable-spacing and bun fmt

After `oxlint --fix`, run `bun fmt`, then `oxlint` and `bun fmt --check` again. A second fix and format pass must change nothing. If the rule and bun fmt keep rewriting each other, stop and report the snippet; do not weaken the rule or the formatter options.

## Copies installed before 2026-10-22

Copies installed from the upstream skill bundle of 2026-08-14 come from `dmmulroy/anti-slop@446268e5d15baa968eaec669ff65358d36ae6259`. Use it as the known base for the three-way merge in `update.md`:

```bash
git clone https://github.com/dmmulroy/anti-slop "$stage/up"
mkdir "$stage/base"
git -C "$stage/up" archive 446268e skills/install-anti-slop/assets/anti-slop | tar -x -C "$stage/base" --strip-components=3
```

Run `diff -rw "$stage/base/anti-slop" <local copy>` first. If only whitespace differs, a formatter rewrote the copy: fix the formatter ignores, treat the copy as pristine, and say so in the report. Record the base in `UPSTREAM.md`.

## Agent instructions

If the repository has an `AGENTS.md`, add one short paragraph: what anti-slop refuses, and what to do when it fires (parse the value at the boundary, name the type, or write a `SAFETY:` comment above the assertion).

## Report

Add to the report: the formatter config that received the ignores, and the proof from step 2 above.
