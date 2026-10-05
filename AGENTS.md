# Fork collaboration: eGovFrame VS Code Initializr

This file governs work in `leejongyoung/egovframe-vscode-initializr`. It is
fork-only and must be excluded from submissions to
`eGovFramework/egovframe-vscode-initializr`.

## Branches and review

- `main` mirrors upstream `main` byte for byte. Never commit to it directly.
  `.github/workflows/sync-upstream-main.yml` fast-forwards it daily and on
  manual dispatch. A non-fast-forward fails without force-pushing.
- `work` is this fork's default integration branch. Fork-only automation and
  guidance live here. Start upstream-bound topics from current `main`; start
  fork-infrastructure topics from `work`. Open fork PRs against `work`, run
  checks, and review before considering an upstream submission.
- Upstream also has `5.0.x`, `develop`, and `version-arch`. Their fork copies
  existed before this setup. Do not change or automate them without first
  establishing their intended upstream roles. This setup mirrors only `main`.

The fork CI runs root and Webview tests, type checking, lint, and the
production build on `work` PRs and pushes using Node 20. It is a code build,
not an Extension Development Host UI test. A UI change still needs manual
F5 verification and screenshots or a reproducible observation.

## Upstream submission

Batch reviewed changes on `work` instead of sending one upstream PR after
every fork PR. From a clean checkout run:

```sh
scripts/open_upstream_pr.sh work main --title "..." --body-file /path/to/body.md
```

The script creates a disposable `upstream-submit/*` branch, removes fork-only
files listed in its `DENYLIST`, and opens an upstream PR against `main`.
Inspect its exact file list and diff against upstream `main` before sending;
update `DENYLIST` whenever fork-only files are added. Never make an upstream
PR directly from `work` or a topic branch used for further development:
GitHub recalculates an open PR from its live head after every push.

`.github/workflows/check-upstream-pr-hygiene.yml` checks all open upstream
PRs from this fork daily. It fails if an upstream PR head does not use
`upstream-submit/*`; the failing run is a continuing alert until the PR is
closed or resubmitted. The schedule runs from default branch `work`, even
when a topic branch lacks workflows. Existing upstream PRs, if any, must be
inspected before changing this rule.

Opening a PR, marking it ready, passing CI, and merging are separate events.
Record version or branch dependencies and required merge order in PR bodies.
Only upstream maintainers can merge there.

## Issues and project

File issues for verified findings with reproduction steps or command output.
Use a `type:*` and `area:*` label, a fitting milestone, and the
[contribution roadmap](https://github.com/users/leejongyoung/projects/4).
The upstream issue templates still apply their default `bug`/`enhancement`
labels, so keep those as well when a template uses them. Check an issue box
only after a specific commit implements it. Link full upstream PR URLs,
identify closed or superseded PRs, and close the issue only after all work
and upstream merges are complete. A fork PR alone does not close an
upstream-bound task.

## Repository-specific verification

- Follow `CONTRIBUTING.md` and `.github/PULL_REQUEST_TEMPLATE.md` for
  upstream contribution format. Current sources specify Node.js 20+.
- Run `npm ci` at the root and in `webview-ui`, then `npm test` in both,
  followed by `npm run package` at the root. The root package script checks
  types, builds the Webview, lints, and bundles the extension.
- Keep Git LFS assets intact, particularly `templates/projects/examples/*.zip`.
  A normal pointer file is not a usable project template.
- This repository is a VS Code extension; there is no Hugo or GitHub Pages
  preview. Use the built extension and Extension Development Host for UI and
  template behavior checks.
