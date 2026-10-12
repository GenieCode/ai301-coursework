# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity | Compare the issue's requested outcome, the plan's implementation scope and exclusions, its deviation notes, and the diff's actual changes and omissions. | Pass when every material change is covered by the plan or a recorded deviation explaining what changed and why, and every material omission is explicitly accounted for. Necessary supporting edits may fit the planned scope without being named line by line. An honestly documented reduction in scope can pass; silently doing more or less than the plan cannot. | required |
| Description fidelity | Compare the PR title, summary, change bullets, completion claims, and reviewer notes with the actual diff, plan, deviations, and test evidence. | Pass when the description accurately communicates the implemented outcome and material limitations. It must not claim a fix, coverage, or completed work contradicted by the artifacts. Disclosed partial outcomes can pass when the title and description do not imply full completion. Do not require a bullet for every line or file when a concise explanation accurately covers the change. | required |
| Observable verification | Compare the plan's test plan and issue repro with the supplied test commands, inputs, before/after observations, relevant test changes, and stated verification limits. | Pass when supplied evidence exercises the affected behavior and shows an observable outcome that can distinguish the original problem from the intended result, with material planned checks performed or their omissions explicitly explained. A rerun of the repro or an equivalent meaningful check is sufficient. For documentation-only changes, direct inspection or the affected documented operation may be decisive. A disclosed remaining failure can pass when the evidence demonstrates the actual outcome and bounds the claim; "tested" without results, irrelevant tests, or assertions that only confirm a mock's programmed answer cannot. | required |
| Repository check evidence | Compare the repository's required checks from the template and contribution rules with recorded commands, outcomes, and the description's testing claims. | Pass when each applicable repository check has a recorded execution and result, or the supplied repository rules establish that it is not applicable. Reported failures, environment blocks encountered during execution, and expected no-tests outcomes may pass when their reasons and limits are disclosed accurately. An unattempted required check, an unexplained missing result, or a false success claim does not pass. Do not demand pre-opening CI when CI only runs after opening the PR. | required |
| Focused and reviewable diff | Read changed files, hunks, and commits against the issue, planned outcome, and supporting test or documentation needs. | Pass when the diff contains issue-related implementation and necessary supporting work, without debug debris, abandoned commented-out code, unrelated cleanup, or generated noise that buries the change. Changes to test markers or suppressions may pass when repository instructions require them for the fix; deleting meaningful assertions to hide a defect does not. Judge relevance and reviewability, not a fixed file count, line count, or number of commits. Disclosure alone does not justify unrelated changes. | required |
| Repository standards and disclosure | Compare the PR title and description with the supplied PR template, contribution rules, AI-use policy, and applicable maintainer requests in the issue thread. | Pass when every applicable stated requirement is satisfied in the artifact it governs, including issue linkage, required template information, truthful checkboxes, and required AI-use disclosure. A permitted N/A with a reason is acceptable. A disclosure in a private plan does not satisfy a requirement for disclosure in the PR description. Apply only requirements established by the supplied repository context; in eval mode do not import live-repository rules or personal voice preferences. | required |
| Reviewer navigation | Read the description's explanation of the change and its testing and reviewer notes against the diff's consequential behavior and known limitations. | Pass when a reviewer can identify what changed, why, how it was checked, and any material limits without guessing or reconstructing the account from scattered contradictions. A short, accurate description can pass; verbosity and elaborate formatting are not required. | preferred |

## Verdict rule

Accept only when every required check passes. Reject when any required
check fails or is unclear. Preferred checks never change the verdict.

Use pass when the evidence satisfies the written condition. Use fail
when the artifacts demonstrate a violation or a required element is
absent from an otherwise available package. Use unclear when a material
fact cannot be established or a necessary source is inaccessible.
Retain unclear as the reported grade, but treat it as fail for a
required check when assembling the verdict.

Apply the honest-outcome rule: a disclosed shortfall, failed command,
or documented deviation is not automatically a rejection. Judge
whether the relevant pass condition is met. Disclosure does not replace
observable testing, excuse an unattempted required repository check,
permit unrelated edits, or override an explicit repository requirement.

Evaluate readiness to submit, not readiness to merge. Do not require
post-opening CI, review approval, or a merge before submission.
