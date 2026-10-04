# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives:** In eval mode, compare the issue context, repro-evidence block, observed artifacts, supplied source excerpts, and maintainer analysis with the candidate plan's problem statement and proposed cause. In live mode, read the issue body, the student's posted reproduction comment, relevant discussion, and the implicated repository files before assessing the draft explanation.

**What good looks like:** The explanation accounts for the behavior the reproduction actually establishes, and the proposed intervention explains how it changes that behavior at the responsible boundary. If the cause is not yet proven, the plan labels it as a hypothesis and identifies a check before making a change that depends on it.

## Scope

**Where it lives:** In eval mode, inspect the candidate plan's intended outcome, actions, named files or areas, exclusions, and any deviation notes against the issue context. In live mode, inspect the detailed plan and proposed comment against the issue and, if supplied, the implementation diff.

**What good looks like:** Each change supports one issue-related outcome, including necessary supporting edits and verification. Unrelated cleanup and speculative expansion are excluded; the number of files and use of a scope heading are not acceptance criteria.

## Executability

**Where it lives:** In eval mode, compare the implementation actions and their order with repository facts, source excerpts, prerequisites, and stated decision points. In live mode, inspect the named repository files or symbols, relevant setup documentation, and the draft's proposed sequence.

**What good looks like:** A contributor can locate the starting point, identify the intended modification, and follow material dependencies without inventing the central approach. Investigation is executable when it identifies what will be inspected and what finding will determine the next action.

## Test plan

**Where it lives:** In eval mode, compare the candidate verification steps, inputs, triggers, and expected observations with the repro-evidence block and any supplied test code. In live mode, compare the plan's verification section with the student's Unit 2 commands and artifacts, relevant existing tests, and the affected source or documentation.

**What good looks like:** The planned check reaches the relevant behavior and specifies an observation that distinguishes the failure from success. A repro rerun or an equivalent regression check is acceptable; for a documentation defect, comparing the relevant files and exercising the documented copy operation can be sufficient without runtime services.

## Honesty

**Where it lives:** In eval mode, read assumptions, risks, certainty claims, decision points, and deviation notes against the issue, repro evidence, and supplied repository facts. In live mode, also compare updated plan notes with any supplied implementation evidence and follow-up comment.

**What good looks like:** Proven facts are distinguished from hypotheses, and unresolved questions that could change the approach have a concrete resolution or boundary. After a reported or evidenced deviation, the notes identify what changed and why, including effects on scope or verification; a pre-build plan does not need fabricated results or deviations.

## Comms

**Where it lives:** In eval mode, read the candidate plan comment against the detailed plan, thread highlights, repo-facts block, templates, and contribution policy. In live mode, read the current issue discussion and relevant repository contribution instructions, then compare them with the draft comment and detailed plan; also consult scope.md and voice-guide.md as directed by SKILL.md.

**What good looks like:** The comment states the actual proposed work and how it will be checked, responds to material maintainer requests, and satisfies applicable repository requirements. Apply disclosure policies to the artifacts they actually cover; a pull-request-only disclosure rule is not automatically a comment rule, and a classmate's example is not itself policy.
