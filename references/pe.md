# PE and grading

## Reporting rules

[Current PE instructions](https://nus-cs2103-ay2627-s1.github.io/website/admin/tp-pe.html#tp-practical-exam-pe):

* Scope: released JAR behavior and rendered UG/DG pages. Code quality and unrelated website/source Markdown are excluded; code inspection can guide testing.
* Reports must result from your own testing. Tools, including AI, are allowed. Product testing precedes documentation testing.
* One issue per independent bug; put the complete report in its body, with exactly one `type.*` and one `severity.*`. Avoid inter-issue references. Record clear, reproducible findings first, then select up to six reports by confidence and justified impact; a large candidate count is not a goal.

| Type | Basis |
| --- | --- |
| `type.FunctionalityBug` | Behavior contradicts specified or reasonable expectations. |
| `type.FeatureFlaw` | Design or missing functionality impairs normal target-user work. |
| `type.DocumentationBug` | Documentation is incorrect, incomplete, or hinders readers. |

| Severity | Impact |
| --- | --- |
| `severity.VeryLow` | Purely cosmetic; usage unaffected. |
| `severity.Low` | Rare, minor inconvenience. |
| `severity.Medium` | Occasional inconvenience; usage remains possible. |
| `severity.High` | Almost unusable for most users. |

Use the lower severity when evenly balanced. Known issues and inherited AB3 defects are not automatically exempt. Recheck the live triaging rules for duplicates and `NotInScope`; recheck phase-specific instructions before drafting developer/tester responses.

## Scope checks for exploratory tests

The [official bug-triaging guidelines](https://nus-cs2103-ay2627-s1.github.io/website/admin/tp-pe.html#guidelines-for-bug-triaging), checked on 28 October 2026, qualify the community test ideas in the [testing checklist](course-notes.md#alpha-beta-and-pe-testing-checklist):

* Unspecified behavior needs a reasonable target-user expectation and a scope assessment. The multiple-instance example mentioned in issue #720 is not a general recommendation to report unsupported concurrency; check the [course clarification in issue #719](https://github.com/NUS-CS2103-AY2627-S1/forum/issues/719) before relying on it.
* Distinguish plausible user mistakes from deliberate sabotage when testing extreme values or overflow. Judge long-input display problems by their actual effect: cosmetic clipping and a workflow blocked by unreadable output have different severities. Reasonable size limits are allowed.
* Manual data-file editing must provide at least AB3's baseline: correctly edited files work, while incorrect edits may lose data. Report support below that baseline, an unfulfilled stronger UG promise, or a format that violates the human-editable-file constraint. A crash or data loss after deliberately corrupting JSON is not automatically a valid bug. Use disposable copies and preserve a baseline for comparison.
* Console exceptions and saved-file contents can establish the cause or persistence of a product defect. Terminal output alone is generally outside PE scope unless it unnecessarily attracts and alarms the user; identify the user-visible or stored-data consequence when reporting it.

## Grading lenses

[tP grading](https://nus-cs2103-ay2627-s1.github.io/website/admin/tp-grading.html): review product fit/cohesion, implementation quality, QA, documentation, and project management when within scope. Check meaningful logging, exceptions, assertions, defensive coding, SLAP, duplication, and standards beyond Checkstyle. Assess UG/DG explanations, navigation, examples, visuals, links, UML, requirements, and glossary against their audience. Tie each concern to a concrete obstacle; avoid demanding more content merely for volume. Accuracy of classifications and responses matters; do not inflate severity or invent exact penalties. Use the live rubric for marks and administrative details.
