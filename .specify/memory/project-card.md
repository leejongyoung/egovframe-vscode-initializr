# egovframe-vscode-initializr project context

Observed from the `work` source at `38b87c6625598d3d62d180114da6ba3f2d230ec1` on 2026-10-26.
This is a maintenance context card, not a claim that application checks were run during adoption.

## Role and public entry points

VS Code extension for eGovFrame project, DDL-to-CRUD and Spring configuration generation. Entry points are root extension `src/`, React Webview `webview-ui/`, `templates/`, root `package.json`, `README.md` and developer docs.

## Evidence and maintenance boundaries

- `README.md`: project, CRUD, configuration and ERD/preview features.
- `CONTRIBUTING.md`, `.github/PULL_REQUEST_TEMPLATE.md`, and `AGENTS.md`: contribution and host/manual verification requirements, LFS constraints.
- `package.json`: extension engine, public commands/settings and actual scripts.
- `.github/workflows/fork-ci.yml`: Node 20, LFS checkout, both dependency installs/tests and production package.
- Public issues #7, #11 and #13 describe mapping, template-test and configuration work; adoption does not resolve them.

Keep the change scope and workflow policy in the [constitution](constitution.md); concept cards
link there rather than repeating the policy. Author a concept card only for actual planned work.

## Build and verification commands

- Fork CI uses Node 20: `npm ci` then `npm ci --prefix webview-ui`.
- Tests: `npm test` and `npm test --prefix webview-ui`.
- Production: `npm run package` includes type checks, Webview build, lint and extension bundling.
- Other declared checks: `npm run check-types`, `npm run lint`, `npm run format` (check mode). Do not run format:fix during unrelated changes.
- UI/template behavior: Extension Development Host (F5), with screenshots or reproducible observations; ensure LFS archives are materialized. None of these application checks ran for adoption.

For this tooling-only adoption, verify Spec Kit CLI/integration status, installed core/bug commands,
constitution completeness, and template resolution/scaffold presence. Full application build,
lint, tests, formatter, external services, and deployment are not part of that smoke check.

## Upstream and submission baseline

Actual upstream is `eGovFramework/egovframe-vscode-initializr`, default `main`; preserve the mirror and other pre-existing branches. Fork work is integration/tooling, not a clean upstream submission baseline.

Use a separate Orca submission worktree from the fetched upstream default SHA, with `upstream-submit` prepared at that SHA if missing. Preserve existing PRs and the existing disposable `upstream-submit/*` head convention. Do not run the current `scripts/open_upstream_pr.sh` here: it creates a work-derived history and its denylist predates Spec Kit. Transfer only selected reviewed application patches to the clean upstream base and inspect commits and files.
No PR is created by adoption. Do not merge `work` or cherry-pick its adoption commit into a submission.
Preserve existing submission branches and PRs. Inspect changed paths and commits against the exact
base before any later PR; exclude work-only scaffolding without deleting upstream-owned files.

## Working with the pinned tooling

Installed by `uvx --from specify-cli==1.2.3 specify init --here --force --non-interactive
--integration omp --script sh --extension bug` after checking for existing path conflicts.
The CLI's init metadata pins 1.2.3; native agent commands live in `.omp/commands/`, including
`speckit.bug.assess.md`, `speckit.bug.fix.md`, and `speckit.bug.test.md`.
Use official upgrade support for future changes and preserve local customizations.
The new [domain model card template](../templates/model-card-template.md) aligns concept meanings
before specification; it is not an ML performance or benchmark card.

Observed submission base: `3365d1a7fc2786bc11c3cd314f81de844f44d2a5` (eGovFramework/egovframe-vscode-initializr, `main`).
Submission branch status: created-remote-at-upstream-default.
