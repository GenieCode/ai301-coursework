# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of my claim and reproduction on the issue I chose in Unit 1, and of the evaluation runs that produced `eval-run.txt`.

---

## Your identity upstream

**GitHub username**

GenieCode

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5863301782

I’m claiming issue #73 to verify the setup-documentation mismatch. I’ll inspect the current repository revision to determine whether `README.md` directs users to configure `OPENROUTER_API_KEY` while `.env.example` omits that variable and lists only `mock` and `openai` for `LLM_PROVIDER`. I’ll also compare those instructions with the settings defined in `core/config.py`.

I’ll follow up with a reproduction report that records the commit and environment I checked, the exact inspection commands, relevant file excerpts, and the observed result. This claim is for the investigation and report; I am not claiming a fix yet.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5863602557

## Result

Reproduced. At commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`, the Quick Start instructions name `OPENROUTER_API_KEY`, but the environment template they tell the reader to copy does not contain that variable. The template also lists only `mock` and `openai` as `LLM_PROVIDER` options, while `core/config.py` defines OpenRouter settings.

## Environment

- OS: macOS 26.6.2, build 25G83
- Architecture: arm64
- Shell: zsh
- Git: 2.50.1 (Apple Git-155)
- Fork: `https://github.com/GenieCode/pathreview-ai301-fa26-s1.git`
- Branch: `main`
- Commit: `f89c06fc3ff292df2a04a39ac51319d32a76b779`
- Commit date: 2026-09-16 14:48:26 -0700
- Initial and final working-tree status: clean

This issue can be observed through static file inspection. I did not need to install dependencies, start the application, or use a real API key.

## Reproduction steps

From the root of my fork at the commit above, I ran:

1. Inspect the Quick Start instructions in `README.md`:

```bash
grep -nE 'OPENROUTER_API_KEY|LLM_PROVIDER|\.env\.example|cp \.env' README.md
nl -ba README.md | sed -n '18,30p'
```

2. Inspect the complete environment template and search it for the key named by the README:

```bash
nl -ba .env.example
grep -nE 'OPENROUTER|OPENAI|LLM_PROVIDER|mock|openai' .env.example
grep -n 'OPENROUTER_API_KEY' .env.example
```

3. Inspect the LLM-related configuration fields:

```bash
grep -nEi 'llm_provider|openai_api_key|openrouter_api_key|openrouter_base_url|openrouter_model' core/config.py
nl -ba core/config.py | sed -n '12,28p'
```

4. Follow the README's copy operation in a temporary directory and inspect the resulting `.env`:

```bash
TEMP_REPRO_DIR="$(mktemp -d)"
cp .env.example "$TEMP_REPRO_DIR/.env"
grep -nE 'OPENROUTER|OPENAI|LLM_PROVIDER|mock|openai' "$TEMP_REPRO_DIR/.env"
grep -n 'OPENROUTER_API_KEY' "$TEMP_REPRO_DIR/.env"
rm -rf "$TEMP_REPRO_DIR"
```

## Expected behavior

The Quick Start instructions and `.env.example` should give compatible setup guidance. If the README tells a user to add `OPENROUTER_API_KEY`, the template copied to `.env` should contain that variable or clearly explain how it should be added. The template's `LLM_PROVIDER` guidance should also account for the OpenRouter settings declared in `core/config.py`.

## Observed behavior and evidence

`README.md` instructs the reader to add an OpenRouter key and then copy the environment template:

```text
24:# Configure environment (add your OPENROUTER_API_KEY to .env)
25:cp .env.example .env
```

The LLM section of `.env.example` lists only `mock` and `openai`, and provides only an OpenAI key placeholder:

```text
17:# Options: "mock" (default, no API key needed), "openai"
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
```

Searching the complete `.env.example` produced:

```text
NO OPENROUTER_API_KEY MATCH FOUND
```

The configuration source includes OpenRouter settings:

```text
18:    llm_provider: str = Field(default="mock")
19:    openai_api_key: str = Field(default="")
20:    openrouter_api_key: str = Field(default="")
21:    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
22:    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")
```

After copying `.env.example` to a temporary `.env`, the LLM-related contents remained:

```text
17:# Options: "mock" (default, no API key needed), "openai"
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
```

A search of that copied `.env` also produced:

```text
NO OPENROUTER_API_KEY MATCH FOUND
```

The mismatch is therefore present in the checked revision: the README names `OPENROUTER_API_KEY`, the provided template omits it and does not list OpenRouter as a provider option, and the configuration source defines OpenRouter fields.

## Scope and limitations

I reproduced the documentation mismatch through static inspection and the documented copy operation. I did not start the application, make an LLM API request, or determine which provider values are accepted at runtime. This report establishes inconsistent setup guidance, not a runtime failure.

I ran the commands and checked the resulting evidence myself.

## Eval iterations

**Run history**

I performed two complete runs, in order:

1. My initial complete run agreed with 19 of 20 scored packages. It matched clear-accept 7/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, and wrong-target 4/4. The only disagreement was `pkg-09`: my rubric rejected it under `Target behavior evidenced`, while the gold label accepted it.
2. I inspected `pkg-09` and kept the rubric unchanged because the run already cleared the 18/20 bar, represented every category, and the disagreement exposed an intentional trigger-evidence threshold rather than an accidental omission. I then ran a second complete confirming evaluation with `--save-run eval-run.txt`. That final run again agreed on 19/20, with `pkg-09` as the only disagreement. This second run is the harness-written `eval-run.txt` committed with this submission.

**Package analysis**

I analyzed `pkg-09`. My rubric's verdict was **reject**, while the gold label was **accept**. The package reported that it could not reproduce scenario 2 of `sharkdp/fd#2033`. It recorded fd 10.4.2, Arch Linux x86_64, kernel 6.15, `ARG_MAX`, exact commands, five repeated runs, and output in which all `ONE` markers appeared before the `TWO` markers.

My rubric rejected the package under `Target behavior evidenced` because the report acknowledged that its padding approach might not have caused command `TWO` to hit the argument-size limit before command `ONE`. My check required evidence that the issue's relevant trigger was exercised, and the package could not establish that decisive precondition. The gold label accepted it as an evidenced and honest cannot-reproduce report because it documented the attempted path, displayed the observed result, identified the unverified precondition, and did not overstate its conclusion.

**Check rationale**

The current check in my uploaded `rubric.md` reads exactly:

```text
| Target behavior evidenced | Read the report's observed result and its output excerpts, logs, screenshots, or other artifacts against the behavior and trigger described by the issue. | Pass when the evidence shows what happened at the issue's relevant trigger and is capable of distinguishing the reported behavior from an adjacent problem. For a cannot-reproduce result, pass when the evidence shows the tested trigger and the different behavior that occurred. Confidence without supporting observation fails. | required |
I used this outcome-based check instead of judging the report by its length, headings, or number of steps. The evidence must show what happened at the issue's relevant trigger and distinguish it from an adjacent problem. I also explicitly included cannot-reproduce results because they can be useful evidence when they show the tested trigger and the different observed behavior. I kept the final sentence because confident wording alone should not substitute for an observation or artifact.

Trade-offs

This check favors proof that the issue's relevant trigger was exercised. That helps reject wrong-target reports that confidently demonstrate an adjacent behavior, but it can reject a useful cannot-reproduce investigation when an internal precondition cannot be forced or independently verified. pkg-09 demonstrates that cost: the report was transparent and repeatable, but my rubric rejected it because the flush-order precondition remained unverified. I accepted that trade-off and left the rubric unchanged after the first 19/20 run rather than loosening it and risking false accepts in the no-evidence or wrong-target categories.

Related paths: eval-run.txt in this directory; the skill files in tools/repro-check/.
