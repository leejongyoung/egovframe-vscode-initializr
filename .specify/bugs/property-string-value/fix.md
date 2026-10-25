# Fix: property-string-value

Slug resolved from assessment context. Removed only PropertyForm validateNumber import/call (nine deleted lines). Shared validator, templates, form contract and required checks are unchanged.

Added real React form regression tests for XML/JavaConfig, values hello/10/empty, External File transition with retained hello, and missing external filename rejection. Tests mock only VS Code messaging and exercise visible controls plus folder response/onSubmit behavior.

No source-text tests or dependencies. Submission worktree uses existing upstream-submit at fresh upstream 3365d1a; work-only artifacts remain on work-side checkout. Before/after hooks: hooks map is empty. Final check results, passing actual browser matrix and host limitation are recorded in test.md. Application commit: 68a81cacb0bbe7cd4b35db563269577b28d0cf5f.
