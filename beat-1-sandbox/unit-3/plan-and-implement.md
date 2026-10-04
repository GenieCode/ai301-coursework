# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

GenieCode

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5975982914

Text of the posted comment, fetched with `gh api repos/codepath/pathreview-ai301-fa26-s1/issues/comments/5975982914 --jq '.body'` and quoted unchanged:

> ## Plan for #73
>
> This builds on my reproduction above at commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`.
>
> ### Diagnosis
>
> In my run, `README.md` Quick Start tells the reader to add the key and then copy the template:
>
> ```text
> 24:# Configure environment (add your OPENROUTER_API_KEY to .env)
> 25:cp .env.example .env
> ```
>
> The template's LLM block has no OpenRouter entry:
>
> ```text
> 17:# Options: "mock" (default, no API key needed), "openai"
> 18:LLM_PROVIDER=mock
> 19:OPENAI_API_KEY=sk-your-key-here
> ```
>
> Searching `.env.example`, and the `.env` copied from it, both gave `NO OPENROUTER_API_KEY MATCH FOUND`. `core/config.py:20` declares `openrouter_api_key: str = Field(default="")`. So a reader who follows Quick Start gets a `.env` with no line for the key the README asks for. My reproduction shows this setup-documentation mismatch only. It does not show a startup failure or any OpenRouter request.
>
> ### Should `openrouter` be listed as an `LLM_PROVIDER` option?
>
> I don't think the template should say that yet. Here is what I found at the same commit:
>
> - `settings.llm_provider` is not read anywhere outside `core/config.py`. Searching for `llm_provider` / `LLM_PROVIDER` finds only `core/config.py:18`, `.env.example`, and `LLM_PROVIDER: mock` in the CI workflows. No code branches on its value.
> - `openrouter_api_key`, `openrouter_base_url`, and `openrouter_model` are also not read outside `core/config.py`.
> - `ReviewGenerator` (`rag/generator/review_generator.py:26`) mentions "OpenAI API or OpenRouter", but it takes an explicit `ReviewConfig` and is not built from `settings` in application code.
> - The only provider-name check I found is `get_embedding_provider()` in `ingestion/embeddings/provider.py`. It accepts `"mock"` and `"openai"` and is an embedding factory, not the `LLM_PROVIDER` selector.
> - I found no maintainer guidance on accepted values in this thread or in `README.md`, `docs/SETUP.md`, `docs/CONTRIBUTING.md`, or `docs/ARCHITECTURE.md`.
>
> So I plan to add the key with wording that acknowledges the OpenRouter settings, and leave `openrouter` off the `Options:` line. If a maintainer confirms that `openrouter` is an intended selector value, I can add it to that line instead.
>
> ### Proposed change (`.env.example` only)
>
> Lines 16–19 stay as they are. I'll insert four lines after `OPENAI_API_KEY`:
>
> ```dotenv
> # LLM provider
> # Options: "mock" (default, no API key needed), "openai"
> LLM_PROVIDER=mock
> OPENAI_API_KEY=sk-your-key-here
> # OpenRouter key named in README Quick Start (core/config.py also defines
> # OPENROUTER_BASE_URL and OPENROUTER_MODEL defaults). "openrouter" is not
> # listed as an LLM_PROVIDER option; setting this key does not change LLM_PROVIDER.
> OPENROUTER_API_KEY=
> ```
>
> The value is left empty to match `Field(default="")` and so no fake key ends up in `.env`. `LLM_PROVIDER=mock` stays the default.
>
> ### Not included
>
> - Adding `openrouter` to the `Options:` line, or any runtime provider dispatch.
> - `OPENROUTER_BASE_URL` / `OPENROUTER_MODEL` entries, since `core/config.py` already has defaults and the README asks only for the key.
> - Changes to `README.md`, `docs/SETUP.md`, `core/config.py`, or other sections of `.env.example`.
> - Any real API request.
>
> ### Verification
>
> 1. `grep -n '^# Options:' .env.example` should still show `17:# Options: "mock" (default, no API key needed), "openai"`.
> 2. `grep -nE '^(LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER_API_KEY)=' .env.example` should show `18:LLM_PROVIDER=mock`, `19:OPENAI_API_KEY=sk-your-key-here`, and `23:OPENROUTER_API_KEY=`.
> 3. After `cp .env.example` to a temp `.env`, the same grep on the copy should return the same three lines.
> 4. With project dependencies installed, loading the copy should print `'mock' ''`:
>    `env -u LLM_PROVIDER -u OPENROUTER_API_KEY .venv/bin/python -c "from core.config import Settings; s = Settings(_env_file='<tmp>/.env'); print(repr(s.llm_provider), repr(s.openrouter_api_key))"`
> 5. `git diff --stat` should show `1 file changed, 4 insertions(+)`, with no real credential.
> 6. `make check && make test-unit`, per `docs/CONTRIBUTING.md`. No Python files change, so I don't expect these to be affected, but I'll report what they actually output.
>
> These are expected observations. I'll post the actual output with the PR.
>
> ### Limitations
>
> - I am not claiming that OpenRouter (or OpenAI) works as an `LLM_PROVIDER` value. At this commit nothing reads that setting.
> - `docs/SETUP.md:47` says the key is "required for AI features". I couldn't confirm that from the source, and this change leaves that line alone.
> - This addresses the template mismatch only, not runtime provider behavior.
>
> I used AI assistance (Claude Code) to help search the source and draft this plan.

---

## Your branch

**Branch**

fix/73-openrouter-env-template

**Evidence**

The capture below is `template-comparison.txt`, reproduced unchanged. It was taken on
`fix/73-openrouter-env-template` at base `f89c06fc3ff292df2a04a39ac51319d32a76b779`, while the
`.env.example` edit was still an uncommitted working-tree change. That same edit was then
committed, unchanged, as `51d1857` and pushed. The BEFORE half is a fresh run of my Unit 2
inspection and copy steps against the template as committed at the base commit. The AFTER half
runs the same steps against the changed template.

```text
# Issue #73: template before/after comparison
# Generated: 2026-10-04T03:26:34Z
# Run from the repository root in bash. Each command below is printed exactly as run.
# Temporary directories are referenced only through $BEFORE_DIR / $AFTER_DIR,
# and all inspection runs inside (cd ...) subshells with relative filenames.

## Context
$ git rev-parse HEAD
f89c06fc3ff292df2a04a39ac51319d32a76b779
[exit status: 0]

$ git branch --show-current
fix/73-openrouter-env-template
[exit status: 0]

$ git status --short -- .env.example
 M .env.example
[exit status: 0]

$ git --version
git version 2.50.1 (Apple Git-155)
[exit status: 0]

## BEFORE: fresh check of the original tracked .env.example at f89c06fc3ff292df2a04a39ac51319d32a76b779
# This is a new run against the template as committed at the base commit.
# It is NOT a replay of the historical run in issue-73-before.txt.

$ BEFORE_DIR="$(mktemp -d)"
[exit status: 0]

$ git show f89c06fc3ff292df2a04a39ac51319d32a76b779:.env.example > "$BEFORE_DIR/.env.example"
[exit status: 0]

$ (cd "$BEFORE_DIR" && grep -n '^# Options:' .env.example)
17:# Options: "mock" (default, no API key needed), "openai"
[exit status: 0]

$ (cd "$BEFORE_DIR" && grep -nE '^(LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER_API_KEY)=' .env.example)
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
[exit status: 0]

$ (cd "$BEFORE_DIR" && grep -n '^OPENROUTER_API_KEY=' .env.example)
[exit status: 1]

$ (cd "$BEFORE_DIR" && cp .env.example .env)
[exit status: 0]

$ (cd "$BEFORE_DIR" && grep -n '^# Options:' .env)
17:# Options: "mock" (default, no API key needed), "openai"
[exit status: 0]

$ (cd "$BEFORE_DIR" && grep -nE '^(LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER_API_KEY)=' .env)
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
[exit status: 0]

$ (cd "$BEFORE_DIR" && grep -n '^OPENROUTER_API_KEY=' .env)
[exit status: 1]

$ rm -rf "$BEFORE_DIR"
[exit status: 0]

## AFTER: current working-tree .env.example (uncommitted change on this branch)

$ AFTER_DIR="$(mktemp -d)"
[exit status: 0]

$ cp .env.example "$AFTER_DIR/.env.example"
[exit status: 0]

$ (cd "$AFTER_DIR" && grep -n '^# Options:' .env.example)
17:# Options: "mock" (default, no API key needed), "openai"
[exit status: 0]

$ (cd "$AFTER_DIR" && grep -nE '^(LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER_API_KEY)=' .env.example)
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
23:OPENROUTER_API_KEY=
[exit status: 0]

$ (cd "$AFTER_DIR" && grep -n '^OPENROUTER_API_KEY=' .env.example)
23:OPENROUTER_API_KEY=
[exit status: 0]

$ (cd "$AFTER_DIR" && cp .env.example .env)
[exit status: 0]

$ (cd "$AFTER_DIR" && grep -n '^# Options:' .env)
17:# Options: "mock" (default, no API key needed), "openai"
[exit status: 0]

$ (cd "$AFTER_DIR" && grep -nE '^(LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER_API_KEY)=' .env)
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
23:OPENROUTER_API_KEY=
[exit status: 0]

$ (cd "$AFTER_DIR" && grep -n '^OPENROUTER_API_KEY=' .env)
23:OPENROUTER_API_KEY=
[exit status: 0]

$ rm -rf "$AFTER_DIR"
[exit status: 0]

## Diff of the working-tree template against the base commit
$ git diff --stat f89c06fc3ff292df2a04a39ac51319d32a76b779 -- .env.example
 .env.example | 4 ++++
 1 file changed, 4 insertions(+)
[exit status: 0]

$ git diff f89c06fc3ff292df2a04a39ac51319d32a76b779 -- .env.example
diff --git a/.env.example b/.env.example
index 1be8b38..00e66ac 100644
--- a/.env.example
+++ b/.env.example
@@ -17,6 +17,10 @@ VECTOR_DB_URL=http://localhost:8001
 # Options: "mock" (default, no API key needed), "openai"
 LLM_PROVIDER=mock
 OPENAI_API_KEY=sk-your-key-here
+# OpenRouter key named in README Quick Start (core/config.py also defines
+# OPENROUTER_BASE_URL and OPENROUTER_MODEL defaults). "openrouter" is not
+# listed as an LLM_PROVIDER option; setting this key does not change LLM_PROVIDER.
+OPENROUTER_API_KEY=
 
 # App settings
 APP_ENV=development
[exit status: 0]
```

Other verification from the after-change run (`issue-73-after.txt`):

- **`Settings` load check: NOT RUN.** There is no project virtualenv, and the system Python cannot
  import `pydantic_settings`. Dependencies were not installed, so the expected `'mock' ''`
  result is unverified.

  ```text
  $ test -x .venv/bin/python
  [exit status: 1]

  $ python3 -c 'import pydantic_settings'
  Traceback (most recent call last):
    File "<string>", line 1, in <module>
      import pydantic_settings
  ModuleNotFoundError: No module named 'pydantic_settings'
  [exit status: 1]

  RESULT: NOT RUN. .venv/bin/python does not exist (project environment not set up via make setup), and the system python3 cannot import pydantic_settings. Dependencies were not installed, per instructions.
  ```

- **`make check`: failed** at its first target (`lint`) because `.venv/bin/ruff` is missing.
  This is a local toolchain problem, not something caused by the `.env.example` change.
- **`make test-unit`: not run.** It was chained with `&&`, so it never started after
  `make check` failed. Lint, format, typecheck, and unit tests are all unverified locally.

  ```text
  $ make check && make test-unit
  .venv/bin/ruff check .
  make: .venv/bin/ruff: No such file or directory
  make: *** [lint] Error 1
  [exit status: 2]

  NOTE: make check failed (exit 2) at its first prerequisite target (lint); because of &&, make test-unit did NOT run. The format (black) and typecheck targets also did not run.
  ```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One documented full run:

1. 2026-09-29T01:37:48Z: 20/20 (`agreement: 20/20 scored items  (bar: 18/20: PASS)`). This is
   the final run, saved with `--save-run` and committed here as `eval-run.txt`. Every category
   was fully matched: `clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.

Limitation: this is the only run with a saved record. It is not necessarily the only run that
occurred. `--save-run` writes only the final
complete run. Before this submission, the course repository held only the unfilled template
`eval-run.txt` (unchanged since the initial commit), and neither the course repository nor the
starter repository has a committed record of an earlier run. I can't establish the scores of any
earlier runs from the available records, so I have not listed any.

**Package analysis**

`pkg-04` (`junegunn/fzf#4260`, category `thread-convention`). My rubric's verdict was
**reject**, and the gold label is **reject** (`pkg-04  thread-convention  reject  reject   yes`).
The saved eval records only the verdict, not the result of each check, so the reasoning below
is my reading of the package against the rubric. It is not a record of what the grader decided
for each check.

The issue asks for keys to reach the `execute` child without any workaround. The repro evidence
states: "Expected: keys reach `less` without a redirection workaround." The candidate plan instead
makes the workaround the deliverable: "Scope: documentation only. In scope: the man page and the
README examples for `execute` bindings; a new FAQ entry. Not in scope: any change to fzf's input
handling code." Its comment gives the reason: "Since the behavior has a reliable workaround, I plan
to document it properly". Documenting `> /dev/tty` tells users how to avoid the bug. It does not
deliver the requested behavior, where `less` receives keys without redirection. That falls short of
**Cause-directed change** ("bypassing the affected path does not pass unless the evidence and
requested outcome establish that behavior as the intended solution"), and here the requested
outcome says the opposite.

It also falls short of **Thread and repository alignment**. The maintainer had already identified
the cause and started a code fix: junegunn wrote "This seems to be the culprit", pointing at "the
console input handling in `src/tui/light_windows.go` (lines 70-84)", and "posted a patched test
binary from commit 8916cbc and asked the reporter to test it". The reporter answered that "the
patched binary pretty much solves it; only the first key press is still swallowed." The candidate
comment never mentions the maintainer's diagnosis, the patched binary, or the remaining first-key
problem. It also does not explain why documentation should replace that direction, and it presents
the docs PR as settled ("I can have the docs PR up this week"). The thread does not forbid
documentation changes. The problem is that the plan ignores where the thread had already gone,
when the rubric requires that "A different proposal may pass if it directly acknowledges the
earlier request, explains the evidence for the alternative, and seeks any required agreement".

**Check rationale**

From `tools/plan-check/rubric.md`, exactly as it reads now:

> | Thread and repository alignment | Compare the draft plan comment and detailed plan with the issue discussion, maintainer requests, repo-facts block, contribution rules, and applicable disclosure or template requirements. | Pass when the comment communicates the actual intended change and verification, agrees with the detailed plan, and honors relevant maintainer constraints and repository requirements. If a required disclosure applies to this comment, it must be present. A different proposal may pass if it directly acknowledges the earlier request, explains the evidence for the alternative, and seeks any required agreement rather than presenting a prohibited change as settled. | required |

Rationale for the wording (my reasoning for the design, not a history of revisions): a plan
comment is posted into an existing conversation, so the check makes the grader read it against
that conversation. That means maintainer requests and analysis, the contribution rules, and any
disclosure or template requirement. A plan can be technically reasonable and still be wrong to
post if it ignores what the maintainer already said or skips a required disclosure. Requiring the
comment to "agree with the detailed plan" catches comments that promise something different from
what will be built. I also did not want the check to reward simple deference. Earlier suggestions
in a thread can be wrong, so the last sentence lets a different proposal pass when it is backed by
evidence and engages with the thread: it acknowledges the earlier request, explains the evidence for
the alternative, and asks for any agreement it needs instead of presenting the change as settled.

**Trade-offs**

This check can hold back a proposal that is useful on its own terms. In `pkg-04`, documenting
the `> /dev/tty` workaround would probably help Windows users today, but the plan is rejected
because it ignores the maintainer's diagnosis and patched binary. A contributor who just wants to
ship something helpful has to engage with the thread's direction first. The check accepts that
cost in exchange for not posting comments that talk past the maintainer. In the other direction,
because a different proposal can pass when it acknowledges the earlier request and backs the
alternative with evidence, the check does not demand agreement with every earlier suggestion. The
cost of that leniency is reliance on the grader's judgment. A plan that mentions the maintainer's
direction only briefly and then thinly justifies going elsewhere could be read as "acknowledging"
and pass when it should not. That is a case I accept this check may miss.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
