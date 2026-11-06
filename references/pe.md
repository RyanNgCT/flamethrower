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

Use the lower severity when evenly balanced. Rate documentation by impact on its readers, including developers reading the DG. Consider visible damage to credibility as well as task disruption, without automatically inflating severity. Recheck phase-specific instructions before drafting developer/tester responses.

## What bugs to report (part c)

Sources checked on **6 November 2026**: [PE part c and triaging guidelines](https://nus-cs2103-ay2627-s1.github.io/website/admin/tp-pe.html#tp-practical-exam-pe), and the [grading definitions and UG/DG lists](https://nus-cs2103-ay2627-s1.github.io/website/admin/tp-grading.html). The checks below operationalize those rules; examples are investigation prompts, not findings or mandatory features for every app.

### Functionality bugs

Use `type.FunctionalityBug` for a failure to meet a specified or reasonable behavioral expectation:

* Exercise UG promises, runnable examples, ordinary successful commands, and supported recovery paths. Compare the actual result, affected records, displayed feedback, and saved/reloaded state with the promised outcome.
* Try plausible mistakes: incorrect commands, extra parameters, missing separators, and invalid values. Rejection itself need not be a defect; investigate incorrect processing, harmful state changes, unusable recovery, or feedback that identifies the wrong problem.
* For unspecified behavior, state the normal user expectation and why it is reasonable. An implementation detail or personal preference is insufficient evidence.
* Resolve a UG/product mismatch by asking what needs repair. A correct feature described incorrectly is a documentation bug; broken behavior requires a functionality/design fix. Do not automatically report both sides of the same discrepancy.

### Feature flaws

Use `type.FeatureFlaw` when the delivered feature's design or missing capability undermines normal use by the intended user, including behavior that exactly follows the UG. Check whether the feature actually completes the stated user task, fits the product, supports the target user's typing workflow, is reasonably testable, and complies with applicable project constraints.

Do not submit a wish list. Establish the target user, realistic task, current limitation, and resulting loss of usefulness. Explain why this is a flaw in the current feature rather than simply an attractive future addition. Apply [feature-flaw triage](#triaging-feature-flaw-bugs), especially its two exceptions to `NotInScope`.

### Possible UG bugs

Use `type.DocumentationBug`; inspect the **rendered HTML UG** and explain the user's reading or usage obstacle.

| Lens | Concrete checks |
| --- | --- |
| Audience and purpose | Can the reader identify the intended user, problem, and value? Are unfamiliar terms explained at the audience's level? |
| Instructions and examples | Follow setup and commands with stated prerequisites. Check syntax, parameter constraints, sample inputs/results, and consistency between sections. Too few examples can leave usage unclear; excessive similar examples can obscure differences. |
| Visual explanations | Check whether screenshots/diagrams are needed to understand a step, actually support the adjacent explanation, and avoid repetitive near-identical images. Missing visuals alone do not prove a bug. |
| Amount and clarity | Locate explanations too terse to follow, needlessly long, ambiguous, or repetitive enough to hide important distinctions. Identify the task or interpretation affected. |
| Correctness and presentation | Verify links and anchors, factual claims, screenshots, legibility, alignment, spacing, and wording in the rendered page. Capture the actual defect rather than inferring it solely from Markdown. |
| Navigation | Can a reader locate the relevant command, prerequisite, limitation, and recovery instruction without avoidable searching? Demonstrate the broken route or confusing structure. |

### Possible DG bugs

Apply the UG readability checks to the **developer audience**, then inspect:

| Lens | Concrete checks |
| --- | --- |
| Architecture | Do diagrams and prose convey component responsibilities and relationships at a high level? Investigate unexplained symbols, ambiguous two-way arrows, and excessive internal detail that obscures the architecture. |
| UML | Compare notation with the course textbook, diagram type with its purpose, and interactions/relationships with the explanation and implementation. Check whether complexity prevents understanding; identify the misleading element. If UML would serve equally well, a substitute diagram can be a DG concern under the rubric. |
| Code excerpts | Does the excerpt isolate the point being explained, or bury it in unnecessary code? State what the reader struggles to find or understand. |
| User stories | Check role, goal, and benefit, their consistency, expected format, and important missing requirements. Explain the consequence for future development. |
| Use cases | Check important workflows, meaningful MSS steps/extensions, numbering and flow references, missing or redundant steps, and excessive UI mechanics. Apply the [use-case audit guide](use-cases.md), which distinguishes useful abstraction from actual omissions; not every parser branch needs an extension. |
| NFRs | Check that requirements are genuinely non-functional, sufficiently scoped to judge, feasible, and cover highly relevant quality needs. An unreasonable unmet NFR may be a `severity.Low` documentation bug; failure to meet a reasonable one can instead be a feature flaw. |
| Glossary | Check missing domain terms that developers need and unnecessary entries that obstruct useful definitions. |

DG checklists identify candidates, not automatic violations. Cite the rendered section/figure and explain how it misleads, delays, or impairs a developer; avoid demanding arbitrary diagram detail or document volume. Secondary DG pages such as the team's setup page are outside PE scope.

## Guidelines for determining bugs

### General triage and scope

* Keep validity, type, severity, scope, and duplication separate. A wrong type/severity does not invalidate a real defect. A valid improvement deferred appropriately is `response.NotInScope`; reserve `response.NotApplicable` for genuinely mistaken reports. When an improvement might reasonably be considered in a future version, favor `NotInScope` over `NotApplicable`.
* A scope defence needs a priority/effort argument: correcting the flaw is less important than completed work and would divert effort from it. Documented exclusions/future plans and graceful or easily recoverable failure support that argument; they are not automatic exemptions. Apply the feature-flaw exceptions below.
* Inherited AB3 bugs remain eligible. A known-issue notice or workaround may reduce impact, but does not erase the bug.
* OS-specific defects can be valid on the required supported platforms. Judge affected population and conditions such as scaling or resolution when choosing severity.
* Duplicate reports must describe the same bug, or symptoms of one defect that cannot be fixed independently. Shared wording, a common category, or similar fixes are insufficient. Require a convincing causal explanation; independently correctable occurrences remain separate.
* When clear evidence shows a defect, make a reasonable attempt beyond the supplied steps before choosing `CannotReproduce`. A broken transferred image warrants checking the transfer problem, not automatic rejection. If a report bundles defects, select the highest-severity valid one and identify it in the response.

### Triaging feature-flaw bugs

**A scope defence cannot excuse either (1) a fix necessary for the app to be reasonably useful, or (2) an existing design that could serve the user better without much additional implementation effort.** A UG statement that behavior is intentional does not settle either question.

Use this decision sequence for each candidate:

1. Identify the delivered feature, target user, and ordinary task. Reproduce the limitation and describe its practical cost, including available workarounds.
2. Establish whether the issue is missing functionality or a poor design, even if documented. Separate a defect in an existing promise from a request for an unrelated feature.
3. Assess reasonable usefulness: can the intended user still accomplish the central task adequately? Explain why the limitation is essential if relying on exception (1).
4. Assess a plausible better design and its additional effort if relying on exception (2). Inspect the relevant implementation where available; support an effort claim with evidence, or label it uncertain. Do not equate fewer UI steps with a necessarily cheap implementation.
5. Evaluate any remaining priority/scope defence against completed work, graceful failure, and documented limits. Choose severity independently from acceptance status. Frame the report around present harm; a possible remedy supports the explanation but need not prescribe the implementation.

| Feature-flaw lens | Probe and triaging distinction |
| --- | --- |
| Duplicate detection | Try realistic case and spacing variants of the same identity. Exact comparison can miss likely duplicates; a warning can preserve user choice. Check whether disclosed limitations prevent false confidence. Do not demand automatic merging or assume similar names always denote one person. |
| Overzealous validation | Try legitimate domain values beyond a programmer's preferred format: multiple annotated contact numbers, past events kept as records, or legal names containing delimiters. Ask whether rejection protects an operation or only blocks useful input. Warnings can be preferable; unhandled harmful input can also be a feature flaw. |
| Error specificity | A truthful but vague error can leave users unable to correct input; distinguish this from a message naming the wrong error. Investigate whether the cause can reasonably be pinpointed. The course treats confusion between invalid values and format errors as `Low`; a combined message can be acceptable when distinguishing the causes qualifies as `NotInScope`. |
| Typing efficiency | Exercise routine commands repeatedly. Look for avoidably long keywords, unnecessary case sensitivity, awkward symbols, or cryptic abbreviations. Short and memorable aliases may help, but justify the actual friction and effort rather than assuming every command needs an alias. |
| Real-world case semantics | Check names and search terms across case variants. Case behavior should reflect the represented entity; distinguish identifiers whose real-world meaning is case-sensitive. Documenting an inconvenient rule does not eliminate a design flaw. |
| Search usefulness | Use partly remembered multiword queries. Assess whether case sensitivity or AND-only matching hides useful results; OR matching may better support uncertain recall. This is a course example of a design trade-off, not a universal requirement that every search use OR. Tie the finding to the app's search purpose and target-user task. |

### Triaging documentation bugs

* Reader hindrance includes avoidable effort or delay, not only unreadability. Pure cosmetic defects are normally `VeryLow`; broken links can be `Low` or `Medium` according to the obstacle caused.
* Tool-specific UML decoration and omitted optional notation are acceptable when neither misleading nor contrary to the course notation. Intentional simplification is acceptable; inconsistent omissions that reduce clarity can still be bugs. Actual notation errors also hurt credibility and can warrant `Low` or higher. Tiny diagrams may remain cosmetic defects even when zooming is possible.
* Missing fine detail in the UG is not automatically a bug when in-app help or errors explain it adequately. Hidden unreleased functionality need not be documented; simple, discoverable, intuitive features can also be omitted. Other exposed undocumented behavior may indicate missing guidance or an inadequately hidden unfinished feature.
* Minor typos, including UI text typos, are `VeryLow` documentation bugs and do not qualify for `NotInScope` merely because they are small. Minor grammar defects may qualify for `NotInScope` if they do not hinder readers.
* A misunderstanding can reveal inadequate documentation. Verify what the page actually leads the reader to believe. For missing requirements, explain the risk of future omission rather than counting missing entries mechanically.

### Report readiness

Before recommending a report, record: **evidence; expected-behavior basis; target-user/reader harm; type; severity; scope argument and counterargument; duplicate relationship; remaining uncertainty**. For feature flaws, explicitly record whether either scope exception applies and the evidence supporting that decision. Do not use `NotInScope` as a synonym for low severity, or the absence of an explicit UG promise as proof that no bug exists.

## Scope checks for exploratory tests

The [official bug-triaging guidelines](https://nus-cs2103-ay2627-s1.github.io/website/admin/tp-pe.html#guidelines-for-bug-triaging), checked on 6 November 2026, qualify the community test ideas in the [testing checklist](course-notes.md#alpha-beta-and-pe-testing-checklist):

* Unspecified behavior needs a reasonable target-user expectation and a scope assessment. The multiple-instance example mentioned in issue #720 is not a general recommendation to report unsupported concurrency; check the [course clarification in issue #719](https://github.com/NUS-CS2103-AY2627-S1/forum/issues/719) before relying on it.
* Distinguish plausible user mistakes from deliberate sabotage when testing extreme values or overflow. Judge long-input display problems by their actual effect: cosmetic clipping and a workflow blocked by unreadable output have different severities. Reasonable size limits are allowed.
* Manual data-file editing must provide at least AB3's baseline: correctly edited files work, while incorrect edits may lose data. Report support below that baseline, an unfulfilled stronger UG promise, or a format that violates the human-editable-file constraint. A crash or data loss after deliberately corrupting JSON is not automatically a valid bug. Use disposable copies and preserve a baseline for comparison.
* Console exceptions and saved-file contents can establish the cause or persistence of a product defect. Terminal output alone is generally outside PE scope unless it unnecessarily attracts and alarms the user; identify the user-visible or stored-data consequence when reporting it.

## Grading lenses

[tP grading](https://nus-cs2103-ay2627-s1.github.io/website/admin/tp-grading.html): review product fit/cohesion, implementation quality, QA, documentation, and project management when within scope. Check meaningful logging, exceptions, assertions, defensive coding, SLAP, duplication, and standards beyond Checkstyle. Assess UG/DG explanations, navigation, examples, visuals, links, UML, requirements, and glossary against their audience. Tie each concern to a concrete obstacle; avoid demanding more content merely for volume. Accuracy of classifications and responses matters; do not inflate severity or invent exact penalties. Use the live rubric for marks and administrative details.
