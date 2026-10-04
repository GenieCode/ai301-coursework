# Plan for issue #73: Align the environment template with OpenRouter setup guidance

## Diagnosis

At commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`, the Quick Start instructions and the supplied environment template disagree about OpenRouter configuration.

My reproduction recorded the README instruction:

> `README.md` instructs the reader to add an OpenRouter key and then copy the environment template:
>
> ```text
> 24:# Configure environment (add your OPENROUTER_API_KEY to .env)
> 25:cp .env.example .env
> ```

The same reproduction recorded the current provider block in `.env.example`:

> ```text
> 17:# Options: "mock" (default, no API key needed), "openai"
> 18:LLM_PROVIDER=mock
> 19:OPENAI_API_KEY=sk-your-key-here
> ```

Searching the complete template for `OPENROUTER_API_KEY` produced:

> ```text
> NO OPENROUTER_API_KEY MATCH FOUND
> ```

After copying `.env.example` to a temporary `.env`, the search again produced:

> ```text
> NO OPENROUTER_API_KEY MATCH FOUND
> ```

The reproduction also found that `core/config.py` declares:

> ```text
> 18:    llm_provider: str = Field(default="mock")
> 19:    openai_api_key: str = Field(default="")
> 20:    openrouter_api_key: str = Field(default="")
> 21:    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
> 22:    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")
> ```

This establishes a static setup-documentation mismatch: the README names `OPENROUTER_API_KEY`, and the settings class declares an OpenRouter key, but the template copied by the documented setup step omits it. The evidence does not establish successful OpenRouter requests or any runtime failure.

### Whether `openrouter` is an accepted `LLM_PROVIDER` value

The issue says the template "offers only `mock` and `openai` for `LLM_PROVIDER`", which raises the question of whether `openrouter` should be added to that list. I checked the source at the same commit and found nothing that establishes it:

- `settings.llm_provider` is not read anywhere outside `core/config.py`. A search of the repository for `llm_provider` / `LLM_PROVIDER` returns only `core/config.py:18`, `.env.example:17-18`, and `LLM_PROVIDER: mock` in `.github/workflows/ci.yml` and `.github/workflows/eval.yml`. No code branches on its value, so no value is accepted or rejected at runtime, including `openrouter` (and `openai`).
- `openrouter_api_key`, `openrouter_base_url`, and `openrouter_model` are also not read outside `core/config.py`.
- `rag/generator/review_generator.py:26` says it generates reviews "using LLM (OpenAI API or OpenRouter)", but it takes an explicit `ReviewConfig(api_key, base_url, model)` and is not constructed from `settings` anywhere in application code.
- The only provider-name dispatch in the codebase is `get_embedding_provider()` in `ingestion/embeddings/provider.py:104-125`, which accepts `"mock"` and `"openai"` only. It is an embedding factory, not an `LLM_PROVIDER` selector, and it is not called from application code.
- No comment on issue #73 is from a maintainer (every commenter's association is `NONE`), and `README.md`, `docs/SETUP.md`, `docs/CONTRIBUTING.md`, and `docs/ARCHITECTURE.md` do not name the accepted `LLM_PROVIDER` values.

Because neither the source nor maintainer guidance establishes `openrouter` as a selector value, the template should add the key the README asks for and acknowledge the OpenRouter settings, without listing `openrouter` as an `LLM_PROVIDER` option.

## Scope

### In scope

- Add an `OPENROUTER_API_KEY=` entry to the LLM provider section of `.env.example`, directly after `OPENAI_API_KEY`.
- Add a short comment above it that ties it to the README Quick Start, mentions the other OpenRouter settings in `core/config.py`, and says that `openrouter` is not listed as an `LLM_PROVIDER` option.
- Keep the existing `# Options:` line, `LLM_PROVIDER=mock`, and `OPENAI_API_KEY=sk-your-key-here` unchanged.
- Re-run the reproduction's static inspection and the documented copy step, and confirm that the copied `.env` still loads with `llm_provider == "mock"`.

### Out of scope

- Adding `openrouter` to the `# Options:` line, or otherwise saying it is a supported `LLM_PROVIDER` value.
- Implementing or changing runtime provider dispatch, or wiring `ReviewGenerator` to `settings`.
- Making a real OpenAI or OpenRouter API request.
- Adding `OPENROUTER_BASE_URL` or `OPENROUTER_MODEL` entries. `core/config.py` already has defaults for both, and the README asks only for the key.
- Changing `README.md`, `docs/SETUP.md`, or `core/config.py`.
- Removing or rewording the existing `"openai"` option, even though the source does not establish runtime support for it either (see Risks).
- Reorganizing any other section of `.env.example`.

## Files to change

- `.env.example`: insert three comment lines and one `OPENROUTER_API_KEY=` line after line 19.

No application source file changes. The reproduced defect is in the contents of the supplied environment template.

## Approach

Proposed LLM provider block (lines 16–23 after the change; lines 16–19 are unchanged):

```dotenv
# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here
# OpenRouter key named in README Quick Start (core/config.py also defines
# OPENROUTER_BASE_URL and OPENROUTER_MODEL defaults). "openrouter" is not
# listed as an LLM_PROVIDER option; setting this key does not change LLM_PROVIDER.
OPENROUTER_API_KEY=
```

1. Leave lines 1–19 of `.env.example` as they are.
2. Insert the three comment lines and `OPENROUTER_API_KEY=` after `OPENAI_API_KEY=sk-your-key-here`.
3. Leave the value empty. That matches the `Field(default="")` in `core/config.py:20`, gives the user an obvious place for the key the README asks for, and avoids putting a non-empty fake key into `.env`. Nothing reads the field at this commit, so the empty value does not change any behavior.
4. Leave lines 20 onward of the current file (`# App settings` through `GITHUB_TOKEN`) unchanged. They move down by four lines.
5. Review the final diff: only those four lines are added, and no real credential appears.

## Test plan

This is a template/documentation mismatch, so verification uses direct inspection, the README's copy step, and loading the copied file through `Settings`. It does not start the application or make an external API request.

### Before the change

The Unit 2 reproduction ran:

```bash
grep -nE 'OPENROUTER|OPENAI|LLM_PROVIDER|mock|openai' .env.example
grep -n 'OPENROUTER_API_KEY' .env.example
```

The provider block showed `mock` and `openai`, and the exact-key search found no match.

The reproduction then copied the template and searched the result:

```bash
TEMP_REPRO_DIR="$(mktemp -d)"
cp .env.example "$TEMP_REPRO_DIR/.env"
grep -nE 'OPENROUTER|OPENAI|LLM_PROVIDER|mock|openai' "$TEMP_REPRO_DIR/.env"
grep -n 'OPENROUTER_API_KEY' "$TEMP_REPRO_DIR/.env"
rm -rf "$TEMP_REPRO_DIR"
```

The copied `.env` also contained no `OPENROUTER_API_KEY`.

### After the change

1. Inspect the template:

   ```bash
   nl -ba .env.example | sed -n '16,24p'
   grep -n '^# Options:' .env.example
   grep -nE '^(LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER_API_KEY)=' .env.example
   ```

   Expected:
   - Lines 16–19 match the current file.
   - Lines 20–23 are the three new comment lines followed by `OPENROUTER_API_KEY=`.
   - The `# Options:` line is still `17:# Options: "mock" (default, no API key needed), "openai"`. It does not include `openrouter`.
   - The key search returns `18:LLM_PROVIDER=mock`, `19:OPENAI_API_KEY=sk-your-key-here`, and `23:OPENROUTER_API_KEY=`.

2. Follow the README copy step and load the result through `Settings`. This needs project dependencies installed, for example via `make setup`. The `env -u` calls keep shell variables from overriding the file:

   ```bash
   TEMP_REPRO_DIR="$(mktemp -d)"
   cp .env.example "$TEMP_REPRO_DIR/.env"
   grep -nE '^(LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER_API_KEY)=' "$TEMP_REPRO_DIR/.env"
   env -u LLM_PROVIDER -u OPENROUTER_API_KEY .venv/bin/python -c \
     "from core.config import Settings; s = Settings(_env_file='$TEMP_REPRO_DIR/.env'); print(repr(s.llm_provider), repr(s.openrouter_api_key))"
   rm -rf "$TEMP_REPRO_DIR"
   ```

   Expected:
   - The copied `.env` contains the same three key lines as the template, including `23:OPENROUTER_API_KEY=`.
   - The Python check prints `'mock' ''`: the default provider is still `mock`, and the new line parses into the existing `openrouter_api_key` field.

3. Review the diff:

   ```bash
   git diff --stat
   git diff .env.example
   ```

   Expected: `1 file changed, 4 insertions(+)`, with no deletions and no real credential.

4. Repository checks required before a PR (`docs/CONTRIBUTING.md`): `make check && make test-unit`. No Python file changes, so these should not be affected. I will report whatever they actually output.

## Risks and unknowns

- `openrouter` is not listed as an `LLM_PROVIDER` option because neither the source nor maintainer guidance establishes it as an accepted value. If a maintainer says otherwise, the `# Options:` line can be extended in a follow-up.
- The existing `"openai"` option is not established at runtime either, because nothing reads `settings.llm_provider`. I am leaving that line alone to keep this change narrow. It is a separate question for maintainers.
- `docs/SETUP.md:47` says `OPENROUTER_API_KEY` is "required for AI features". At this commit no application code reads that key, so I could not verify that claim. This change does not touch it.
- The reproduction did not test provider dispatch or make a real API request. This plan fixes the template mismatch, not runtime provider behavior.
- The `Settings` load check needs installed dependencies. If they are not available, the check will be reported as not run rather than assumed.
- The new key has an empty value, so no placeholder could be mistaken for a real credential.

## Deviations

### Implementation

The implementation matched the approved plan, with no deviations. On branch `fix/73-openrouter-env-template` (base `f89c06f`), the only change is the four approved lines inserted into `.env.example` after `OPENAI_API_KEY=sk-your-key-here`. Lines 16–19 and every other line are unchanged. `git diff --stat` reported `.env.example | 4 ++++` and `1 file changed, 4 insertions(+)`, and `git diff --check` exited 0 with no output.

### Verification

- **Before capture (extra step, not in the original plan):** Before editing, I re-ran the inspection and copy step at `f89c06f`. `grep -n 'OPENROUTER_API_KEY'` found no match in `.env.example` or in the copied `.env`, with grep exit 1 both times.
- **Step 1, template inspection: ran, matched expectations.** Lines 20–23 are the new block. `grep -n '^# Options:'` returned `17:# Options: "mock" (default, no API key needed), "openai"`, so `openrouter` was not added. The assignment grep returned `18:LLM_PROVIDER=mock`, `19:OPENAI_API_KEY=sk-your-key-here`, and `23:OPENROUTER_API_KEY=`. All exit statuses were 0.
- **Step 2, copy step: partly run.**
  - The copy greps ran in a fresh temporary directory and matched expectations: the same three assignment lines, plus the unchanged `Options:` line, which was an extra check. The real `.env` was not read or modified.
  - The `Settings` load check was **NOT RUN**, so the expected `'mock' ''` result is unverified. `.venv/bin/python` does not exist (`test -x` exited 1), and the system `python3` cannot import `pydantic_settings` (`ModuleNotFoundError`). Dependencies were not installed.
- **Step 3, diff review: ran, matched expectations.** The diff has no deletions and no real credential.
- **Step 4, `make check && make test-unit`: failed for an environment reason, and tests did not run.**
  - `make check` stopped at its first target, `lint`, with `make: .venv/bin/ruff: No such file or directory` and exit status 2.
  - Because of `&&`, `make test-unit` did not run. The `format` and `typecheck` targets did not run either.
  - The failure is a missing local toolchain (no `.venv`), not something caused by the `.env.example` change. However, lint, format, typecheck, and unit tests are all **unverified** locally. The PR's CI run (all five jobs) will need to provide that result.

Commands, output, and exit statuses were captured in `/tmp/issue-73-before.txt` and `/tmp/issue-73-after.txt`. Both logs are preserved in `~/Documents/issue-73-evidence/`, alongside `template-comparison.txt`.
