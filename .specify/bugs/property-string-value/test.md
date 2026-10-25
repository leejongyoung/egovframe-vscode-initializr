# Verification: property-string-value

## Result
Application patch is ready for parent review, with a recorded Extension Development Host limitation. Slug reused from assessment/fix context. All registered bug hooks are empty.

## Before
Fresh upstream main `3365d1a7fc2786bc11c3cd314f81de844f44d2a5`: actual Vite application in Orca browser, XML/Internal Properties, greeting=hello. `form.requestSubmit()` produced “The following fields must contain only numeric values: Value” and zero selectOutputFolder messages. Snapshot and screenshot were captured before editing.

## Final checks performed
- `npm ci` and `npm ci --prefix webview-ui`: exit 0, lockfiles unchanged. Node v24.22.0 (project requires Node 20+; exact CI Node 20 not exercised).
- `npm test`: exit 0, 4 files / 41 tests passed.
- `npm test --prefix webview-ui`: initial exit 1, 4 new tests used an exact label without the accessibility whitespace. Corrected selectors only; final exit 0, 14 files / 156 tests passed, including all 10 new form cases.
- `npm run package`: exit 0; root types, Webview TypeScript + production Vite build, lint and extension bundle completed.
- Scoped final Prettier and git diff --check passed. Existing commit hook also ran Prettier and eslint --fix successfully; committed source patch remains the reviewed nine-line removal.
- No build, lint, formatter or tests ran mid-flight; the actual browser reproduction preceded the source change, and checks started after implementation.

## Actual browser runtime
`npm --prefix webview-ui run dev -- --host 127.0.0.1`, running actual application in Orca browser. Synthetic configTemplates and selectedOutputFolder messages represent the extension boundary; production form/controls/validation/ConfigView ran in the browser. Asserted both requested and generated generationType explicitly, including after form resets:

| Format | Internal hello | Internal 10 | External after hidden hello | Missing external filename |
| --- | --- | --- | --- | --- |
| XML | folder request + generateConfig | folder request + generateConfig | folder request + generateConfig | rejected, no request |
| JavaConfig | folder request + generateConfig | folder request + generateConfig | folder request + generateConfig | rejected, no request |

The regression tests additionally cover optional empty string Value in both formats. This is actual browser behavior, not a claim of filesystem generation or Spring execution.

## Host limitation
Launched built extension with VS Code --extensionDevelopmentPath, then a clean isolated profile under local Git metadata with --disable-extensions. The clean Extension Development Host opened its welcome overlay but accessibility exposed only window chrome, and attempted interactions did not establish verifiable navigation to the form. The earlier ordinary host had unrelated installed extensions. No sign-in or global setting was changed. F5-equivalent host generation / native folder picker / actual output files remain UNVERIFIED; parent review must decide whether browser evidence suffices or obtain manual host evidence before publishing.

## Evidence and isolation
Detailed raw command logs, synthetic browser before/after snapshots, before screenshot and replay script remain local under Git metadata `property-value-evidence/`, with exact local paths in the coordinator's report JSON. Raw desktop state is not published. Work-only assessment/model/fix/test cards are separate from the application submission.

Submission commit `68a81cacb0bbe7cd4b35db563269577b28d0cf5f` has exactly two paths: PropertyForm.tsx and its behavior regression test. Its parent is the fetched upstream main SHA. No source-text/wiring test, dependency, template, public doc or tooling changes. No submission push, issue update or PR creation occurred.
