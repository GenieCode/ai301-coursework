# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributor investigating one reported behavior at a time. I state what I tested, separate observations from assumptions, and give maintainers enough concrete information to repeat my work. I do not present myself as the maintainer or promise work that I have not completed.

## Rules I write by

### Rule: Name the actual investigation

I identify the issue-specific trigger or behavior instead of posting a generic claim that could apply to any issue.

- Wrong: "I would like to work on this issue."
- Right: "I’m claiming this issue to investigate why the review page fails to preserve the selected path after the reported navigation sequence."

### Rule: Promise investigation, not a fix

Before reproducing, I say what I will test and what evidence I will report. I do not promise that I will fix the issue or finish by a particular date.

- Wrong: "I’ll fix this by tomorrow."
- Right: "I’ll test the reported path in my fork and follow up with the environment, exact steps, observed result, and supporting output."

### Rule: Separate expectation from observation

I label what the issue says should happen separately from what I personally observed.

- Wrong: "The feature is broken exactly as reported."
- Right: "The issue expects the selected path to remain visible; in my run, the selection disappeared after I returned to the page."

### Rule: Match confidence to evidence

I use reproduced, partially reproduced, or could not reproduce according to the evidence I actually collected.

- Wrong: "Confirmed," when my output shows a different error.
- Right: "I could not reproduce the reported timeout; with the environment below, the request completed and returned status 200."

### Rule: Speak only for my own run

I do not use another person's reproduction as a substitute for my environment, steps, or evidence.

- Wrong: "Same as above; I can confirm."
- Right: "Using the environment and steps below, I reached the reported trigger and observed the following output."

## Things I never post

- A promise to fix an issue before I have investigated it.
- A deadline or completion date I cannot guarantee.
- A claim that I reproduced behavior without evidence from my own run.
- Another contributor's result presented as if it were mine.
- Credentials, access tokens, private paths, or other secrets.
- A required disclosure omitted from a repository that asks contributors to disclose AI assistance.
- Certainty when the available evidence supports only a partial or cannot-reproduce result.
