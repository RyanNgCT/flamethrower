---
name: flamethrower
description: Review CS2103T team-project PRs and prepare practical-exam testing, bug reports, or responses using course standards and grading rules. Use for CS2103T review work, not general Java development.
---

# Flamethrower

Find defensible issues and explain their consequences. Be thorough in investigation and concise in reporting; do not invent findings to fill a quota.

For installation, prerequisites, and invocation examples, see [the setup guide](README.md).

## Establish scope

Identify the requested mode, repository or released artifact, revision, and relevant UG/DG. Use the user's specified scope; ask only for missing inputs that prevent useful work. Review requests authorize investigation and local drafts, not code changes or posting reviews/issues.

Read only the relevant resources:

* [PE rules](references/pe.md) for exam testing, reports, and responses.
* [Standards](references/standards.md) for Java, Git, Markdown, or documentation review.
* [Review issue catalogue](references/review-issues.md) for code/PR issue patterns, severity ratings, and fixes.
* [Course review lenses and notes index](references/course-notes.md) for design, tests, debugging, and UML. Use the bundled synthesis directly; no external notes folder is required.
* [Use-case audit guide](references/use-cases.md) for reviewing actor/system workflows, MSS, extensions, preconditions, guarantees, and deriving behavioral bug probes.
* [Diagram audit guide](references/diagram-audits.md) for sequence/architecture reviews, abstraction levels, message notation, and facade-boundary claims.

The references summarize the linked course sources and forum guidance for offline or quick use. When online, recheck administrative rules for an actual PE or grading decision, especially phases, limits, runtime, and submission instructions. Offline, continue with the summaries; flag decisions requiring uncached details or current rules. Use current course requirements over conflicting notes; project conventions fill gaps without silently overriding course requirements. Do not invent rules or mark deductions.

## Use-case audits

Use the [use-case audit guide](references/use-cases.md) when reviewing use cases or deriving tests from them. Check the externally observable actor goal, a self-contained MSS, meaningful alternatives, valid resume/end points, and any stated preconditions and guarantees against the UG and product. Trace code to explain a discrepancy, and reproduce the affected workflow before claiming a product bug.

Match the use case's abstraction level. The course recommends intention-focused steps without UI mechanics or internal implementation details; it does not require every validation partition, exact error string, storage failure, or optional use-case element to be listed. Numbering is a convention. Keep source-backed guidance, project conventions, and optional editorial suggestions distinct, and justify findings by their behavioral or reader impact.

## Temporary workspace

Keep all rough work and temporary files inside `<base>/tmp/`, where `<base>` is the session's starting project/workspace directory. The shorthand `/tmp` in this skill means that base-relative folder, not the system-root or OS temporary directory. Resolve this location before changing working directories. Use it for PR and PE code-review downloads, temporary clones or source archives, diffs, scratch scripts, logs, and disposable test data.

Before ending the session, retain requested deliverables in their intended destination, then clear all temporary files and subdirectories from `<base>/tmp/`, leaving the folder itself in place. Before recursive cleanup, verify the resolved absolute targets remain inside that exact folder.

## PR review

Establish the actual base/head and inspect the merge-base-to-head diff, surrounding code, callers, tests, and relevant documentation. Keep existing defects and local uncommitted work distinct from changes introduced by the PR.

Optional context helper (Python 3.9+ and Git; paths may contain spaces):

```text
python scripts/review_context.py "/path/to/repository" --base origin/main --head HEAD
```

Run from the skill folder, or use the script's absolute path. Use `python3` on macOS or `py -3` on Windows when appropriate. Supply the actual PR refs; the helper reads local Git data and does not fetch, switch branches, or run tests. Its status describes the current checkout, which may differ from the requested head.

Trace changed behavior through parsing, commands, model, storage, and UI where relevant. Check applicable standards manually as well as using the project's existing checks. Inspect the build/CI configuration before choosing commands. In Codex, skip Gradle execution because of the known sandbox permission issue; do not request permission or escalation to run Gradle. Continue with source inspection and available CI evidence, and report Gradle checks as unrun. In other harnesses, such as Claude Code and OpenCode, Gradle may be run: use `gradlew.bat` on Windows and `./gradlew` on macOS/Linux. Run focused tests at the reviewed revision when the harness permits them; report commands, results, and anything untested. Apply this harness rule to PE code reviews and other Gradle tasks in this skill as well. Passing Checkstyle or coverage alone does not establish correctness.

Return an impact-ordered list or table of issues and violations. For each entry, include the exact file/line, applicable standard or contract, evidence/trigger, consequence, justified severity, and a concrete fix. Use the [catalogue's rating policy](references/review-issues.md#rating-policy): PE labels for eligible bugs, and explicitly advisory PR ratings for code-only concerns. Separate demonstrated defects, verified standard violations, and optional suggestions; keep plausible but unverified candidates separate with the check needed to confirm them. Tie design criticism to an actual maintenance or correctness cost. Use PR priorities only if requested or required by the review format; do not equate them with PE severity. If there are no actionable findings, say so and state material verification gaps.

## PE investigation and output

Test the supplied release against its UG/DG and the target user's workflows. Use the [testing checklist](references/course-notes.md#alpha-beta-and-pe-testing-checklist) for alpha, beta, and PE investigations, and the [PE scope checks](references/pe.md#scope-checks-for-exploratory-tests) before turning exploratory results into PE reports. Source inspection suggests tests; behavior in a rebuilt checkout does not prove a defect in the supplied release. Label unexecuted candidates as hypotheses. Do not claim GUI behavior was observed when only source or logs were inspected.

Before classifying candidates, read [what bugs to report](references/pe.md#what-bugs-to-report-part-c) and the [triaging guidance](references/pe.md#guidelines-for-determining-bugs). For feature flaws, apply the [feature-flaw triage](references/pe.md#triaging-feature-flaw-bugs) even when behavior matches the UG: establish the normal target-user task, concrete harm, and whether a scope defence survives the essential-usefulness and low-additional-effort exceptions. For documentation, inspect the rendered page and explain the obstacle to its intended reader; use the separate UG/DG checks rather than treating every omission as a bug.

Record findings as they occur, reproduce candidates from a stated starting state, reduce the steps, and separate independent defects. Keep disposable test data inside `<base>/tmp/`. For every proposed report, provide:

* A title naming the observable failure.
* Artifact/version, OS/runtime, initial data, and exact steps/inputs.
* Expected behavior with its basis, actual behavior, and captured evidence.
* Impact on the intended user or reader; one justified type and severity.

Draft reports or phase-specific responses in the format the user requests. Distinguish evidence from inference, and explain any uncertain scope or classification. Finish with what was tested and the remaining coverage gaps.
