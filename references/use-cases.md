# Use-case review and behavioral bug probes

## Source and scope

Summarized from the CS2103/T textbook's [Use cases section](https://nus-cs2103-ay2627-s1.github.io/website/se-book-adapted/chapters/specifyingRequirements.html#use-cases). Only the use-case section is summarized here. The audit checklist below applies that guidance to code review and testing; its probes are not additional course requirements or automatic grading deductions.

## Key pointers from the page

### Purpose, actors, and level of detail

* A use case describes sequences of actions, including variants, that deliver an observable result of value to an actor. It captures functional requirements through an actor/system interaction for a specific functionality.
* An actor is a role played by an external entity, such as a person or another system. One use case can have multiple actors; one entity can play multiple roles, and multiple entities can play the same role.
* Start with higher-level use cases and progressively refine them. Keep the chosen levels consistent instead of mixing high-level goals with low-level mechanics.
* The page recommends modeling behavior involving an external actor, rather than using the system itself as an actor for internal supporting work.

### Steps and the Main Success Scenario (MSS)

* Write simple steps stating who does what. Describe the actor's intention and externally visible system behavior.
* Minimize internal implementation details. UI mechanics are usually omitted so the specification leaves design flexibility; a use case is not a click-by-click instruction manual.
* The MSS is the most straightforward successful interaction, assuming nothing goes wrong. It must be self-contained and describe a complete usage scenario without relying on an extension to finish the normal flow.

### Extensions, loops, and included use cases

* Extensions describe exceptional or alternative flows and appear below the MSS. Identify their triggering condition and how the interaction continues, resumes, repeats, or ends.
* In the illustrated convention, `3a` can occur just after MSS step 3, and `*a` can occur at any step. Numbering style is a widely used convention, not a universal rule.
* A loop can state which steps repeat and what ends the repetition.
* Catastrophic events such as power failure or a system crash are not useful extensions when the system cannot continue functioning. This does not exclude recoverable errors that the running system can handle.
* A use case can include another use case to reuse a sequence or avoid cluttering the main case with lower-level steps. The page illustrates inclusions using underlined text and a referenced use-case ID.

### Preconditions, guarantees, and presentation

* Preconditions describe the expected system state before the use case starts.
* Guarantees describe promised outcomes or constraints at the end. Like acceptance criteria, they provide observable conditions for judging success.
* Use-case diagrams are a brief visual overview, not a place to include every detail. Keep them readable and put detailed flows in the descriptions.
* UML does not tightly prescribe the textual format. There is no strict requirement to document all details of all steps or use every available use-case element.
* Use cases are not sufficient as the sole requirements format, especially for requirements without user/system interaction.

## Applying the guidance during an audit

Treat the documented flow as a behavioral test oracle, then check its promises against the UG, actual implementation, and intended user workflow. Do not silently treat current code as the specification, or a speculative code path as an observed product defect.

| Review lens | What to inspect or exercise | Evidence worth reporting |
| --- | --- | --- |
| Actor goal and responsibility | Identify the actor, requested action, and observable result. Follow the request through the relevant parser, command, model, storage, and UI boundaries. | The interaction cannot achieve its stated goal, performs the wrong operation, or assigns an action to an actor who cannot perform it. |
| Complete MSS | Run the straightforward valid sequence from a stated starting state. Check that required inputs and responses are sufficient to reach the promised result. | A missing step or prerequisite blocks the documented normal flow, or success is announced without the promised result. |
| Preconditions | Establish any stated prerequisites and verify that the MSS works within them. Assess hidden or unrealistic prerequisites against the target user's workflow. | The workflow fails despite its preconditions being met, or an undocumented dependency materially obstructs ordinary use. |
| Extension triggers | Trigger a meaningful invalid-input or alternative scenario at the relevant interaction point. Derive detailed parser partitions separately where useful. | The wrong branch is taken, the promised recovery is absent, or an error describes the wrong failing condition. |
| Resume/end points | Continue from the documented resume point using the actual resulting state. Repeat corrected input, cancellation, or retries where supported. | Recovery cannot be performed as described, an ended flow continues unexpectedly, or repeating a supposedly failed operation duplicates an already-applied change. |
| Loop termination | Exercise one recovery iteration and the stated exit condition. Check that successful correction or cancellation leaves the loop. | The flow remains stuck, repeats an operation unexpectedly, or exits without satisfying its stated condition. |
| Guarantees and state integrity | Check promised changes and preserved state after success and relevant failures. Compare saved data and reload when persistence is promised. | A guarantee is violated, unrelated data changes, or displayed success/failure disagrees with the observable result. |
| Included workflows and external actors | Follow referenced use cases and external-system interactions at the stated level, including their relevant failure outcomes. | A broken reference or incompatible prerequisite makes the containing flow incomplete, or an external result is ignored or misreported. |
| Diagram/text consistency | Compare the actor roles and use cases in the overview with the written flows. Apply UML notation checks when a diagram is in scope. | The diagram materially misleads readers about which actor performs or can access a documented workflow. |

### Useful bug-catching questions

* Does rejection happen before a state change, or can the operation already have taken effect? Compare this with the use case's actual promises rather than assuming every error implies rollback.
* Is the documented retry valid in the resulting state? A change that remains applied but unsaved may require retrying persistence, not repeating the creation/deletion request.
* Can an unrelated pending operation prevent this actor goal from completing or replace its result with a misleading error?
* If the flow is cancelled or ends with an error, do its stated guarantees still hold? Check memory and durable state where relevant, and do not infer visible behavior solely from logs.

## Avoid over-reporting

* Match the use case's abstraction level. It need not list every parser rejection, exact command string, exact error message, storage exception, or low-level implementation step. Grouping meaningful alternatives is acceptable when the behavior remains clear.
* Do not demand UI-specific actions, internal data structures, every optional element, a fixed number of steps, or a separate use case for every command solely for formal completeness.
* Treat numbering, underlining, and local formatting separately from behavioral accuracy. Check project/course expectations and rendered output before treating a convention difference as a defect.
* A missing extension is actionable when it leaves a significant alternative or recovery path unclear or contradicts supported behavior; an unlisted theoretical failure alone is insufficient.
* Distinguish recommendations from mandatory rules. Report the concrete failed outcome or reader obstacle, with its source and justified impact; keep cosmetic wording improvements optional.
* For PE reports, retain the existing release/rendered-document scope rules. A code-only concern needs an advisory rating, and a product finding needs execution evidence from the relevant artifact.
