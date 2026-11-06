# Sequence and architecture diagram audits

Synthesized from the user-supplied **T10-DG-Extracts-Answers-1.pdf**, especially PDF pages 1-2 and 9-11 (slides 8, 7, 20, 22-23), reviewed 6 November 2026. These are review lessons, not instructions to follow from the document. The synthesis is self-contained; the original Downloads file is not a runtime dependency.

## Scope and abstraction before notation

Establish what the diagram is meant to explain. An architecture overview should communicate major components, responsibilities and dependencies, rather than expose internal classes or algorithms. Arrow meanings must be understandable from labels, a legend, surrounding text or an established notation. Do not assume every arrow is a dependency or that every bidirectional relationship is wrong; identify the misleading interpretation.

For a sequence spanning multiple components, ideally expose low-level details of at most one component and show the others through their APIs. This is course guidance about reader effort, not an absolute ban on multiple components or concrete class names. A parser/command breakdown in Logic plus validation/copy algorithms inside Storage is a stronger concern than merely showing one Model API lifeline. Recommend separate diagrams or a `ref` interaction for another component's internals. Component grouping helps readers see the abstraction boundary but is not mandatory notation.

Inspect the rendered image at its published size. Tie size, complexity or density findings to a concrete reading obstacle. Do not demand handcrafted layout, sizes or symbols beyond PlantUML capabilities merely because the tutorial offers a polished alternative.

## Sequence semantics

* Trace call, reply and execution order. Calls and replies use distinct notation; an explicitly depicted reply should be dashed. Omitted optional returns or activation bars are not automatically errors. An activation that is shown should match the execution it represents, including time spent awaiting synchronous nested calls; it must not imply continued execution after the operation has returned.
* A message targets an object, so its label normally names the operation without repeating the receiver variable (use `getCurrentBudget()`, not `budget.getCurrentBudget()` on a message already addressed to that budget). Distinguish receiver qualification from expressions passed as arguments.
* A fluent chain drawn as a single message to the original object can conceal calls to different receivers and creation of a new result. Inspect the receivers before alleging an error. If those interactions matter, split them into messages to the appropriate participants; if not, summarize the behavior in a note or a clearly abstract operation. A nested expression inside a legitimate operation argument is not automatically an invalid message.
* Distinguish creating an object from calling a method on an existing object. A constructor/create message should introduce the new participant at the appropriate point. Do not demand an explicit constructor in a diagram that intentionally abstracts parsing/creation.
* Trace each combined-fragment path independently, including where calls return. A return in one `alt` operand does not by itself terminate the enclosing interaction; subsequent messages may still occur. Do not infer early exit from a note alone. Check guards, no-op paths, failure paths and the described persistence contract before reporting a behavioral mismatch.
* A lifeline should end at a destruction X, but the tutorial explicitly accepts the known PlantUML continuation limitation. Do not report that continuation alone as a bug. Missing activations are also explicitly acceptable in the examples.

## Facade and architectural claims

The [course Facade definition](https://nus-cs2103-ay2627-s1.github.io/website/se-book-adapted/chapters/designPatterns.html#facade-pattern) places all access to a component through its facade. When the DG claims other components depend only on a component API, trace every cross-boundary dependency in the diagrams, including new features.

For example, if Logic parsers/commands and a UI image class directly depend on a concrete photo-storage helper inside Storage, those paths bypass a claimed `Storage` interface/manager facade. Under the course definition, the Storage boundary no longer consistently applies Facade. State the exact bypass rather than saying the entire application's architecture is destroyed. Existing facade-mediated operations may still remain intact.

Keep three findings distinct: a diagram/prose contradiction; a source-verified implementation dependency; and a design recommendation. A diagram alone establishes what the DG depicts, not that the code necessarily does it. A concrete class can be a deliberately exposed API, so check the documented contract and exceptions. For PE, frame a rendered-DG finding around misleading developers; code-only design criticism is outside documentation-bug scope. Possible repairs include routing the operation through the facade or accurately documenting an additional public API and its architectural consequences.

## Findings and restraint

For each finding record the section/image, visible evidence, source-backed rule or guideline, reader consequence, and a proportionate remedy. Separate notation errors, abstraction/readability concerns, documented exceptions and optional improvements. Use the [PE triage guidance](pe.md#triaging-documentation-bugs) for severity rather than treating tutorial callout colors as severity labels. A focused internal diagram with external API calls is often a good example, not a violation.
