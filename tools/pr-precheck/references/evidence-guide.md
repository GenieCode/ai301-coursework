# Evidence guide: where evidence lives in a PR package

Use this guide to locate evidence, not to introduce additional checks.
Apply the pass conditions and verdict rule in rubric.md. In eval mode,
use only the supplied bundle; do not fetch its source repository or
import requirements from the live Path Review repository.

## Plan fidelity (harness category: silent-drift)

### Where it lives in an eval bundle

- `Issue` and `Thread highlights`: the requested outcome and any
  maintainer constraints.
- `Plan context`: the accepted approach, named files or areas,
  exclusions, test plan, and any recorded deviations.
- `Candidate PR` -> `Diff` and `Commits`: what the candidate actually
  changes.
- `Candidate PR` -> `Title` and `Description`: what the candidate
  claims to deliver, including completion claims and limitations.
- Read any deviation notes wherever the bundle supplies them; do not
  require a particular heading unless the repository requires it.

### Where it lives in a live PR

- The issue URL's body and discussion, including the posted plan.
- `plan.md`, including `## Deviations`, for current scope and reasons
  for changes from the original plan.
- `git diff main...HEAD` and `git log main..HEAD`, run at the working
  repository root, for committed changes and commit information.
- `pr_draft.md` for the outgoing title, description, and reviewer notes.
- For a house-chain submission, use the supplied house plan and issue.

### What good looks like

Every material diff change and planned omission is accounted for by
the plan or a documented deviation explaining what changed and why.
The title and description accurately describe the implemented outcome,
including any disclosed partial result, rather than claiming more or
less than the diff supports.

### Calibration example

In calib-01, `Plan context` calls for removing only the first duplicate
key in gruvmax-fang and excludes other themes. The diff removes that
line, retains the effective value, and agrees with the description.

## Test evidence (harness category: not-tested)

### Where it lives in an eval bundle

- `Plan context`: the original reproduction, expected result, and
  promised verification.
- `Candidate PR` -> `Test evidence`: commands, inputs, before/after
  observations, suite results, and stated limitations.
- `Candidate PR` -> `Diff`: added or changed tests and assertions.
- `Repo facts`: required checks and contribution instructions.
- `Candidate PR` -> `Description`: test claims and any testing
  checkboxes to compare with the actual evidence.

### Where it lives in a live PR

- The issue's reproduction comment or the supplied house repro pack
  for the original trigger and observed failure.
- `plan.md` for the intended verification and documented changes.
- `test_evidence.md` for the before/after reproduction and captured
  check commands and outputs.
- The committed diff for regression tests and relevant assertions.
- The target repository's `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` for required checks and applicable exceptions.
- `pr_draft.md` for the results a reviewer will actually see.

For this assignment, capture make test-unit, make test-integration,
make lint, and make typecheck. Preserve the expected "no tests ran"
integration output rather than relabeling it as passing tests. Do not
require post-opening CI to have run before the PR exists. These live
requirements do not become requirements for unrelated eval repositories.

### What good looks like

The evidence exercises the affected behavior and supplies an observable
before/after result or equivalent decisive verification, rather than
only saying "tests pass." Applicable repository checks have recorded
outcomes; failures, environment blocks, and remaining limits are
reported accurately instead of concealed or presented as success.

### Calibration example

In calib-01, the strict-parser repro changes from DuplicateOptionError
to "parsed OK." The git-config output retains exactly one effective
value, and the package reports the planned smoke check; these concrete
observations support the stated outcome without requiring Path Review's
make commands.

## Diff quality (harness category: unreviewable)

### Where it lives in an eval bundle

- `Candidate PR` -> `Diff`: changed files, individual hunks, test
  changes, debug remnants, commented-out code, and unrelated edits.
- `Candidate PR` -> `Commits`: the candidate's commit list.
- `Plan context`, `Issue`, and `Repo facts`: the basis for deciding
  whether each change is necessary supporting work or unrelated scope.

### Where it lives in a live PR

- `git diff main...HEAD` for the actual committed hunks.
- `git diff main...HEAD --stat` for an overview, not a substitute for
  reading the full diff.
- `git log main..HEAD` for the commit list.
- `plan.md`, the issue thread, and contribution instructions for the
  relationship between changed files and the intended fix.
- Repository instructions for issue-specific xfail markers and
  suppressions when those are removed as part of a seeded-bug fix.

### What good looks like

The fix and necessary supporting changes are visible without unrelated
cleanup, abandoned code, debug leftovers, or generated noise obscuring
them. Judge the role of each change rather than the number of files or
commits; disclosure does not make unrelated work necessary.

### Calibration example

In calib-01, the diff consists of the planned one-line removal.
The changed hunk directly addresses the duplicate key and includes
no unrelated theme edits.

## Standards and comms (harness category: standards-wall)

### Where it lives in an eval bundle

- `Repo facts`: PR template requirements, contribution rules, and
  stated policy, including whether an AI-use policy exists.
- `Issue` and `Thread highlights`: applicable maintainer direction.
- `Candidate PR` -> `Title` and `Description`: issue references,
  required sections, testing claims, disclosures, and reviewer notes.
- Other candidate metadata when a stated requirement governs it.

Do not infer a missing policy from personal expectations. A bundle
explicitly saying there is no template does not require template
sections borrowed from another repository.

### Where it lives in a live PR

- The target repository's `.github/PULL_REQUEST_TEMPLATE.md`,
  `docs/CONTRIBUTING.md`, and any applicable policy they reference.
- The issue thread for explicit maintainer requests.
- `pr_draft.md` for the title, completed template, issue linkage,
  testing checkboxes, screenshots or explained N/A, and reviewer notes.
- For this course, the AI-use disclosure belongs in Notes for Reviewers.
- `test_evidence.md` and the diff corroborate testing statements and
  other factual claims.
- `voice-guide.md` supplies separate live-only feedback on the title
  and description; it does not independently change the verdict.

### What good looks like

The outgoing PR satisfies the requirements actually stated for its
repository, in the artifacts those requirements govern, with real
content rather than unfilled boilerplate. A reviewer can identify
what changed, why, how it was checked, and the material limitations;
check claim-to-diff truthfulness under plan fidelity as well.

### Calibration example

In calib-01, the captured contribution rules request a small PR and
an issue reference; the diff is focused and the description names
#2211. The bundle states that no PR template or AI policy applies,
so their absence must not become an invented failure.
