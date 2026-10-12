---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Grade exactly one PR package to answer one question: is this ready to
submit? A PR package contains the candidate title, description,
commits, diff, and test evidence, read against the plan it claims to
implement and the issue that plan belongs to.

Execute this skill's components. Do not substitute a general code
review, grade multiple packages in one run, or decide from gut feel.

## Inputs and modes

Use exactly one of these modes:

- **Live mode:** Read the files named in the user's request:
  `plan.md`, including deviation notes; `pr_draft.md`, containing the
  title and description; and `test_evidence.md`, containing test and
  reproduction results. From the top of the working repository, use
  `git diff main...HEAD` to obtain the complete committed branch diff.
  Read the branch's commits with `git log main..HEAD`. Do not treat
  uncommitted edits as part of the submitted change.

  Read the supplied issue's full thread, including relevant repro and
  plan comments, using GitHub through `gh`, the API, or the web.
  Read the target repository's `.github/PULL_REQUEST_TEMPLATE.md`,
  `docs/CONTRIBUTING.md`, and any stated policy those documents
  reference, including applicable AI-use requirements.

  For a house-chain student, use the supplied house issue, house plan,
  house repro pack, and working branch in place of the student's own
  issue, plan, repro evidence, and branch. Apply the same checks and
  verdict rule.

  Identify missing or inaccessible evidence explicitly. Do not invent
  file contents, issue statements, test results, or policy requirements.

- **Eval mode:** Use the supplied package bundle as the whole world.
  Every package fact must come from its text. Use the provided skill
  components as grading instructions, but do not fetch URLs, inspect a
  working repository, run commands to supplement the package, or read
  other packages. Ignore `scope.md` and `voice-guide.md` entirely.
  Execute every rubric check and the full verdict rule, even if the
  bundle lacks evidence needed by a check.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Use its Repo line
and house rules to establish the allowed target repository and how
evidence must be read in that environment.

If the Repo line contains an unfilled placeholder, stop without grading.
Tell the student to fill the `Repo:` line in this skill's `scope.md`
with their section's Path Review repository. Never guess the scope.

Refuse to grade work intended for a repository outside that scope.
Distinguish the upstream target repository from the student's fork:
a local fork is acceptable when the intended PR targets the scoped
repository. If the target cannot be established, ask for clarification
rather than assuming it.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and check both the outgoing PR
title and description against it. Report each broken rule in the
readable summary, quoting the rule and identifying the conflicting
draft text.

Voice feedback does not change the verdict on its own unless an
explicit rubric check reads the voice guide. Do not invent extra
rubric checks from personal style preferences.

In eval mode, ignore `voice-guide.md` entirely. Apply only the
universal communication-quality checks written in the rubric.

## Component reads

Read `rubric.md` to obtain the check names, evidence requirements,
pass conditions, required or preferred weights, and verdict rule.

Read `references/evidence-guide.md` to locate each evidence family
and understand what supporting evidence looks like in each mode.

Read and execute `procedure.md` as written. It determines the read
order, evidence gathering, check execution, and verdict assembly.
Honor the live-mode scope gate before the procedure's artifact reads.

If `rubric.md` has no actual checks or `procedure.md` has no actual
operating steps, stop without grading and explain which component
needs content. Template comments and empty table rows do not count
as authored content. Never invent checks or steps to make an empty
skill appear functional.

When the procedure is silent on a necessary step, report that gap.
Do not silently invent a replacement step. Mark affected checks
unclear when the gap prevents verification, and apply the rubric's
verdict rule.

## Verdict and output

For a completed grading run, produce only one binary verdict:
`accept` means ready to submit; `reject` means hold. Do not produce
a third verdict, an "accept with reservations," or a numeric score.

Print a concise per-check summary before the JSON, identifying each
check's grade and decisive evidence. Include procedure gaps and
live-mode voice feedback there when applicable.

Then end the reply with the fenced JSON block below, populated with
the package identifier, every rubric check, and the verdict. Preserve
the key names, structure, and order. Use only `pass`, `fail`, or
`unclear` for grades. Keep each evidence value to one line and make
the JSON valid. Put nothing after the closing fence.

Use the PR URL when one exists or the bundle id in eval mode. For
a live draft with no PR URL yet, use an honest draft identifier based
on the supplied issue URL and branch; never invent a PR URL.

For a refusal, explain why grading stopped instead of manufacturing
check grades or a verdict.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: name the fact, quote, diff location, or specific
  missing evidence that decides every check. "Looks fine" is not
  evidence. A claim that tests passed is not automatically proof
  that they ran or covered the planned behavior.
- Grade the thing, not the polish: compare the actual artifacts
  against the plan, issue, and stated repository requirements.
  Do not reward length, confidence, or attractive formatting.
- Let the rubric decide: apply each stated pass condition and
  weight exactly. If a passing check feels wrong, report the tension
  rather than changing the grade by intuition.
- Let the procedure decide how: follow its steps and report gaps
  rather than improvising around them.
- Treat `unclear` as the rubric's verdict rule directs. Where that
  rule is silent, treat `unclear` as `fail` for verdict assembly,
  while retaining `unclear` as the reported check grade.
- Compare the diff with the plan including documented deviations.
  Do not pretend an undisclosed change was planned, and do not
  assume disclosure alone satisfies every other rubric check.
- Keep claims distinct from evidence. Do not turn missing results
  into passing results or conceal contradictions between the
  description, diff, and test evidence.
