# Reconcile Local Main With Origin

Status: Approved
Last updated: 2026-09-04
Owner: Shane Bishop

## Goal

The local `main` contains 43 commits that `git range-diff` identifies as patch-equivalent to commits already on `origin/main`. The remote also contains seven later commits. Four uncommitted file changes remain from deliberate March 30 CLI help-output cleanup; the Yarn `packageManager` metadata was added incidentally by tooling.

Adopt the canonical remote history, retain the intended cleanup, and leave a clean, verified, synchronized `main`.

## Scope / Out of Scope

- In scope: preserve the current local history in a safety branch; adopt `origin/main`; retain the plain help/version output and removal of development-only help; add three README badges; verify and push.
- Out of scope: changing behavior from remote commits, dependency upgrades, live E2B checks requiring credentials, and release publication.

## Dependencies and Constraints

- `origin/main` is canonical because all 43 local-only patches are already present there under different commit IDs.
- The safety branch is retained locally until the user explicitly requests removal.
- Only the three intentional non-`package.json` worktree changes are restored.

## Execution Breakdown

1. Create a local safety branch at the existing `main` tip and reset `main` to `origin/main`, preserving all prior work and incorporating the seven remote-only commits.
2. Reapply the three intentional CLI help-output changes. Do not restore the accidental Yarn `packageManager` entry.
3. Add npm version, CI workflow, and MIT license badges below the README title.
4. Run focused CLI tests, style checks, and the production build. Commit the intentional retained work and README update, then push directly to `main`.

## Validation Evidence Expectations

- `npm run test -- test/cli.bootstrap.test.ts` exits zero and verifies help/version output.
- `npm run check:style` exits zero.
- `npm run build` exits zero and creates the package output.
- `git status --short --branch` reports a clean `main` synchronized with `origin/main` after push.

## Related Docs

- `README.md`
- `docs/launcher-config-reference.md`
