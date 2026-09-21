# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73

**Verdict output**
accept


**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
    "checks": [
      {
        "name": "Maintainer activity",
        "grade": "pass",
        "evidence": "Collaborator Aburke225 opened this issue 2026-09-16 and authored main commits 2026-09-16, within 180 days of 2026-09-20."
      },
      {
        "name": "Repository in use",
        "grade": "pass",
        "evidence": "Last 5 main commits dated 2026-09-16 (x3) and 2026-08-24 (x2) — 5 of 5 within 180 days."
      },
      {
        "name": "Newcomer scope",
        "grade": "pass",
        "evidence": "Body names exactly two files, README.md and .env.example, and asks only to 'Make the two files agree.'"
      },
      {
        "name": "Issue availability",
        "grade": "pass",
        "evidence": "assignees empty, 0 comments, and the repo has no pull requests at all, so no linked PR or claim."
      },
      {
        "name": "Project policy fit",
        "grade": "pass",
        "evidence": "docs/CONTRIBUTING.md contains no prohibition on AI-generated code or documentation."
      },
      {
        "name": "Reproduction clarity",
        "grade": "pass",
        "evidence": "Verified live: .env.example has OPENAI_API_KEY and LLM_PROVIDER=mock but no OPENROUTER_API_KEY, which README Quick Start tells you to set."
      }
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/63",
    "checks": [
      {
        "name": "Maintainer activity",
        "grade": "pass",
        "evidence": "Collaborator Aburke225 opened this issue 2026-09-10 and authored main commits 2026-09-16, within 180 days of 2026-09-20."
      },
      {
        "name": "Repository in use",
        "grade": "pass",
        "evidence": "Last 5 main commits dated 2026-09-16 (x3) and 2026-08-24 (x2) — 5 of 5 within 180 days."
      },
      {
        "name": "Newcomer scope",
        "grade": "pass",
        "evidence": "Bounded to one test fixture in tests/unit/test_readme_scorer.py: extend the fixture or correct the assertion."
      },
      {
        "name": "Issue availability",
        "grade": "pass",
        "evidence": "assignees empty, 0 comments, timeline shows only labeled events, and the repo has zero pull requests."
      },
      {
        "name": "Project policy fit",
        "grade": "pass",
        "evidence": "docs/CONTRIBUTING.md contains no prohibition on AI-generated code or documentation."
      },
      {
        "name": "Reproduction clarity",
        "grade": "pass",
        "evidence": "Body gives the command 'pytest tests/unit/test_readme_scorer.py -q' and the observed failure 'assert 51 > 100'."
      }
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/70",
    "checks": [
      {
        "name": "Maintainer activity",
        "grade": "pass",
        "evidence": "Collaborator Aburke225 opened this issue 2026-09-10 and renamed it 2026-09-16, within 180 days of 2026-09-20."
      },
      {
        "name": "Repository in use",
        "grade": "pass",
        "evidence": "Last 5 main commits dated 2026-09-16 (x3) and 2026-08-24 (x2) — 5 of 5 within 180 days."
      },
      {
        "name": "Newcomer scope",
        "grade": "pass",
        "evidence": "Bounded to the README parser test area: de-indent the sample_readme_text fixture and remove the xfail marker; no repository-wide redesign required."
      },
      {
        "name": "Issue availability",
        "grade": "pass",
        "evidence": "assignees empty, 0 comments, and the repo has no pull requests, so no open linked PR or claim."
      },
      {
        "name": "Project policy fit",
        "grade": "pass",
        "evidence": "docs/CONTRIBUTING.md contains no prohibition on AI-generated code or documentation."
      },
      {
        "name": "Reproduction clarity",
        "grade": "pass",
        "evidence": "Body states test_parse_standard_readme asserts heading_count > 0 while every fixture line is indented four spaces, so the parser finds no headings."
      }
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

I ran the evaluator in this order:

1. Full run: `agreement: 15/20 scored items`.
2. Targeted disagreement run: `agreement: 2/5 scored items`.
3. Targeted run after revising scope wording: `agreement: 3/5 scored items`.
4. Full run: `agreement: 16/20 scored items`.
5. Targeted policy run on issue-12: `agreement: 0/1 scored items`.
6. Targeted policy run after revising the policy check: `agreement: 1/1 scored items`.
7. Targeted run on issue-09, issue-15, and issue-20: `agreement: 1/3 scored items`.
8. Complete saved run: `agreement: 17/20 scored items`.
9. After making the maintainer-activity and availability thresholds explicit, I ran another complete evaluation: `agreement: 19/20 scored items`.
10. After filling in my fit profile in `scope.md`, I regenerated the complete harness-written run committed as `eval-run.txt`: `agreement: 19/20 scored items`.

The early rubric rejected clear accepts because the scope and availability checks were too broad. I changed maintainer activity to a 180-day threshold, treated only claims or work updates within 90 days as active, treated closed pull requests as inactive, and added an explicit contribution-policy check. The final committed run matched every category and scored 19/20.
agreement: 19/20 scored items

**Issue analysis**

I analyzed `issue-12`.

The evaluation table recorded:

> `issue-12  reject  reject   yes`

My rubric's decision was `reject`, and the gold label was also `reject`. The repository was active, and the issue had no assignee or linked pull request, so the activity and availability checks passed. The deciding evidence was the repository-facts contribution-policy text:

> “We do not accept AI-generated code or documentation.”

My required `Project policy fit` check treats that explicit policy as a failure because this course uses an AI-assisted contribution workflow. Before I added that threshold, my rubric accepted issue-12. The revised check rejected it for the same reason as the gold label.

**Check rationale**

The current check in my rubric is:

> | Project policy fit | `repo-facts` block: the contribution-policy text and its named section; `issue body` and `comment thread`: any additional maintainer restrictions on contribution methods | Pass if the contribution policy does not explicitly prohibit AI-generated code or documentation. Fail if the policy states that AI-generated code or documentation is not accepted, because this course uses an AI-assisted contribution workflow. Also fail if a maintainer explicitly prohibits the contribution method required by the course. Mark unclear only if the policy uses ambiguous language about whether AI-generated contributions are accepted. | required |

I made this check required because a repository can be active, an issue can be unclaimed, and the task can be bounded while still being incompatible with the course workflow. The check names the contribution-policy text as its evidence source and uses an explicit prohibition as the failure threshold. It does not fail a repository merely because no AI policy is present. I added this wording after issue-12 passed every other required check even though its contribution policy prohibited AI-generated contributions.

**Trade-offs**

This policy check gives up otherwise suitable issues in repositories that explicitly prohibit AI-generated code or documentation. For example, issue-12 was active, unassigned, and bounded, but the policy check changed its result from accept to reject. That trade-off is intentional because selecting an issue that cannot be completed under both the repository's rules and the course's AI-assisted workflow would create a problem in Unit 2.

The relevant category result changed from:

> `policy 0/1`

to:

> `policy 1/1`

The check may still miss a restriction that is not present in the captured contribution policy, issue body, or maintainer comments. I accepted that limitation rather than treating missing policy language as a failure.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Issue #73 fits my interests and available time because it is a small documentation and configuration consistency fix. It is limited to `README.md` and `.env.example`, is labeled Tier 1 and good first issue, and does not require a broad application-code change. I prefer a focused, low-risk first contribution, so it ranked ahead of the two Python test issues.

2. The verdict correctly identified that the repository is active, the issue has no assignee, comments, active claim, or linked pull request, the work is bounded to two named files, and the repository policy does not prohibit the course workflow. The rubric could not fully weigh my personal confidence or schedule. I separately considered that a two-file documentation/configuration change is a better fit for my current experience and available time than changing parser or scoring tests.

3. I expect claiming it to be straightforward because the live evidence showed no assignee, no comments, and no linked pull request. The main difficulty is that this is a shared course repository, so another student could claim it before I complete the Unit 2 claim process. I will check its current assignment, comments, and pull-request status again before posting the Unit 2 claim.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
