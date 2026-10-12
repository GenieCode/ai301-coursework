# Unit 4: Pull Request Submission

## PR link

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/153

## Branch

`fix/73-openrouter-env-template`

## Run history

I completed one full evaluation using Sonnet with my installed
`pr-precheck` skill:

- **Full run 1: 18/20 agreement**
- clear-accept: **5/7**
- not-tested: **4/4**
- silent-drift: **4/4**
- standards-wall: **2/2**
- unreviewable: **3/3**

This met the target of at least 18/20 agreement and at least one match
in every category. The two disagreements were `pkg-08` and `pkg-11`:
both had a gold label of `accept`, but my skill returned `reject`.

I inspected both packages and retained the evaluated skill version.
I did not run partial evaluations or additional full evaluations.
The submitted `eval-run.txt` is the unchanged harness output from
this full run.

Separately, I ran the skill in live mode three times on my own draft PR:

1. **Reject:** the frontend check required by `docs/CONTRIBUTING.md`
   had no recorded result. I ran
   `cd frontend && npm ci && npm test -- --run`, which passed
   18 tests in 2 files, and added the result to the evidence and draft.
2. **Reject:** the revised PR description was missing its AI-use
   disclosure. I restored a disclosure under Notes for Reviewers.
3. **Accept:** all six required checks and the preferred
   reviewer-navigation check passed.

These live runs were draft reviews, not scored package evaluations.

## Package analysis

I analyzed **pkg-08**.

- **My skill's verdict:** reject
- **Gold label:** accept
- **Failing check reported by the harness:** Repository check evidence

The package addresses lazygit's untracked-only stash flow. Its plan
calls for showing an explanatory toast instead of opening a prompt
that cannot create a stash, while preserving the tracked-change flow.

The evidence includes before/after observations showing the new toast
and absence of the name prompt, with the untracked file still present.
It also reports a successful tracked-change control, a passing named
integration test, passing `go test ./...`, and `go generate ./...`
producing no cheatsheet diff. The description states that the code
was formatted.

These facts support the gold acceptance. The captured repository
requirements ask for formatted code but do not explicitly require
a separate formatter-command transcript. My rejection suggests that
the skill may have interpreted its evidence requirement too strictly,
demanding additional execution evidence for an outcome requirement.

The results table identifies the failing check but does not show
its precise deciding evidence. Therefore, the formatter-transcript
explanation is a hypothesis, not a confirmed account of the grader's
reasoning. The broader lesson is to distinguish explicitly required
command executions from repository requirements about the resulting
artifact.

## Check rationale

The **Repository check evidence** check currently has this pass condition:

> Pass when each applicable repository check has a recorded execution and result, or the supplied repository rules establish that it is not applicable. Reported failures, environment blocks encountered during execution, and expected no-tests outcomes may pass when their reasons and limits are disclosed accurately. An unattempted required check, an unexplained missing result, or a false success claim does not pass. Do not demand pre-opening CI when CI only runs after opening the PR.

I wrote this condition to catch unsupported testing claims and omitted
repository checks without treating every nonzero exit as missing
evidence. A recorded failure can still provide useful evidence if the
description accurately explains its outcome and limitations.

This distinction mattered in my own PR. The integration command
collected zero tests and returned a nonzero status, which I reported
without checking the integration-passed box. The check also caught a
real omission: I had run the four checks listed in the assignment but
had not yet run the frontend check required by CONTRIBUTING.md.
I added that execution and its results before opening the PR.

The pkg-08 disagreement exposes a limitation of the current wording:
the skill may demand command-level evidence where the repository
only specifies an outcome, such as formatted code. I retained the
evaluated wording and documented this limitation rather than claiming
the check was perfectly calibrated.

## Trade-offs

The full run correctly rejected all 13 gold-reject packages but
falsely rejected two of the seven gold-accept packages. This favors
catching submission problems at the cost of sometimes holding a
ready PR for unnecessary clarification or evidence.

I retained the 18/20 version because it met the target and matched
every category, including both standards-wall packages. Its main
observed limitation is overly strict judgment on some acceptable
packages, particularly around repository-check evidence and
description fidelity.

I did not loosen the checks solely to match the two gold labels.
A future revision should distinguish material omissions from minor
wording imprecision and distinguish required command executions from
artifact-quality requirements. Any such revision should be tested
against the affected packages and relevant canaries, followed by a
new full evaluation.
