# Bug Assessment: Property Value rejects strings

- **Slug**: property-string-value (auto-generated for supervised non-interactive task)
- **Created**: 2026-10-26
- **Source**: https://github.com/leejongyoung/egovframe-vscode-initializr/issues/13
- **URL policy**: github.com, allowlisted, fetched via gh issue view; treated as data.
- **Verdict**: valid
- **Severity**: medium — ordinary configuration generation is blocked; no data loss observed.

## Report (summarized)
Property Value is restricted to numbers although XML and JavaConfig templates consume strings. External File also validates an unused hidden Value.

## Symptom
A running browser served from fresh upstream main displays “The following fields must contain only numeric values: Value” for `greeting=hello`; no selectOutputFolder message is emitted.

## Reproduction
1. `npm ci` and `npm ci --prefix webview-ui` in the separate pristine upstream submission checkout.
2. `npm --prefix webview-ui run dev -- --host 127.0.0.1`; open actual application at http://127.0.0.1:25463 in Orca browser.
3. Deliver synthetic configTemplates host message for the real Property template, choose Configuration / Property / New Property / Configure.
4. Fill Key `greeting`, Value `hello`, submit using DOM form.requestSubmit() (the first Orca click did not trigger submission).
5. Observe numeric error and zero folder requests. See evidence/before-browser.json and before-screenshot.png. No tests/build/lint ran before this reproduction.

## Suspected Code Paths
- `webview-ui/src/components/egov/forms/PropertyForm.tsx:141`: unconditional validateNumber call.
- `webview-ui/src/utils/codeUtils.ts`: shared numeric validator correctly rejects nonnumeric strings.
- `templates/config/property/property.hbs` / `property-java.hbs`: existing string consumers.

## Root Cause Hypothesis
High confidence: the form applies a numeric-only validator to a string property, unconditionally across modes. Current upstream main is `3365d1a7fc2786bc11c3cd314f81de844f44d2a5`, freshly fetched; affected form matches the work-side source.

## Proposed Remediation
**Preferred**: remove only PropertyForm's validateNumber import and Value validation block. Preserve shared numeric validation, existing required checks, templates, messages and optional empty Value behavior.

**Files likely to change**:
- `webview-ui/src/components/egov/forms/PropertyForm.tsx`
- `webview-ui/src/components/egov/forms/__tests__/PropertyForm.spec.tsx`

**Tests to add or update**: render real form controls, submit strings/numeric-looking/empty values for XML and JavaConfig through folder response; switch to External File with hidden hello; verify missing external filename remains rejected. Mock only extension messaging boundary, not validators or UI controls.

## Risks & Considerations
No dependencies or schema changes. Arbitrary literal escaping is not solved by this validation removal. Final full root/Webview suites, production package and actual browser smoke are required; Extension Host verification remains a distinct requirement.

## Deduplication
Read actual fork issue #13 and all fork/upstream PRs with gh, including open and closed (upstream 39 PRs, zero open; fork one merged PR). No Property Value fix is present. [Upstream #31](https://github.com/eGovFramework/egovframe-vscode-initializr/pull/31) changes path safety, not PropertyForm; [#38](https://github.com/eGovFramework/egovframe-vscode-initializr/pull/38) changes settings lookup only. Existing bare upstream-submit branch equals fresh upstream and has no PR; preserve and use it. Application submission is never derived from work/tooling history.

## Open Questions
No blocking policy questions. Actual Extension Host evidence must be obtained or explicitly reported as missing. No submission push or PR until parent review.
