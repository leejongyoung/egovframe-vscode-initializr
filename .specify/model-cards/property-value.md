# Domain model card: Property Value

## One-sentence definition
A developer supplies a string property value in the configuration Webview, which passes that string through folder selection to the XML or JavaConfig property-service template.

## Why it is needed
[Issue #13](https://github.com/leejongyoung/egovframe-vscode-initializr/issues/13) reports that a numeric-only form check blocks ordinary string properties and stale hidden values in External File mode.

## Existing models and terminology
- `webview-ui/src/components/egov/types/templates.ts`: observed `ConfigFormData.txtValue?: string`, with `generationType` XML/JavaConfig and `rdoType` Internal Properties/External File.
- `PropertyForm.tsx`: observed default `pageUnit=20` is a string example, not a numeric schema; Key/Value are optional controls shown only in Internal Properties.
- `templates/config/property/property.hbs`: observed XML map entry string attribute.
- `templates/config/property/property-java.hbs`: observed `Map<String, String>` with quoted values.
- `selectOutputFolder` and `selectedOutputFolder`: observed two-stage submission boundary; no generator or protocol change is needed.

## Synthetic example data
`{generationType: "xml", txtFileName: "context-properties", txtPropertyServiceName: "propertiesService", rdoType: "Internal Properties", txtKey: "greeting", txtValue: "hello"}`. Numeric-looking `"10"` and empty `""` remain strings; no unit applies. External mode uses fictitious `classpath:/example.properties`.

## Successful behavior
- Internal XML/JavaConfig `greeting=hello` or `greeting=10` -> folder request, then unchanged value delivered to onSubmit after folder response. Supported by both existing templates and typed string field.
- Empty optional Value -> remains accepted, preserving current behavior.
- Internal `hello` then External File -> retained hidden Value does not block submission; templates select only external filename/encoding in this mode.

## Rejected or invalid behavior
- Missing external Property File Name -> required-field error, no folder request (existing form validation).
- Invalid filename/class/package or service identifier -> existing validation remains authoritative.
- Numeric fields in other forms continue using shared `validateNumber`; this concept does not redefine them.

## Outside this concept's responsibility
Template escaping, arbitrary Java literal correctness, service runtime loading, multi-property editing, folder picker implementation, and other configuration forms. No new string restrictions or required Value policy.

## Rule sources and assumptions
| Rule | Source and revision/date | Evidence kind | Assumptions or limits |
| --- | --- | --- | --- |
| Value is a string | types/templates.ts and both property templates at upstream `3365d1a7fc2786bc11c3cd314f81de844f44d2a5` | Observed | Not a guarantee for every possible escaped character |
| Hidden Value cannot block external mode | Same templates, mode branches | Observed | Existing Key special-character validation is outside this bug |
| Numeric check causes failure | Browser reproduction on 2026-10-26; evidence/before-browser.json | Observed | Dev browser, host bridge is console fallback |
| Issue acceptance | Public fork issue #13, read 2026-10-26 | Public claim, reproduced for XML hello | Further cases pending final verification |

## Open questions
No blocking policy decision identified. Arbitrary Java escaping is a separate template concern and is not expanded here.

## Next step and evidence of completion
See [assessment](../bugs/property-string-value/assessment.md), followed by fix.md and test.md. Follow the [constitution](../memory/constitution.md). Actual browser failure and passing XML/JavaConfig matrix are recorded in test.md; 41 root and 156 Webview tests and packaging passed. Extension Host generation remains unverified. Detailed evidence stays local under Git metadata; the coordinator report names its paths.
