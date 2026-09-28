# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives:** In an eval bundle, inspect the issue context and repo-facts block first, then compare them with the environment record, setup commands, repository revision, runtime versions, dependency versions, platform details, and configuration recorded in the repro report. In live mode, inspect the GitHub issue and repository documentation, then compare those requirements with the student's draft report.

**What good looks like:** The package records the environmental variables needed to recreate the relevant execution context. Repository revision and relevant runtime or dependency versions are identified, while platform or configuration details are required when they can affect the reported behavior. A difference from the issue's target is explicitly identified rather than silently treated as equivalent.

## Steps

**Where it lives:** In an eval bundle, inspect the repro report's setup commands and reproduction actions and compare them with prerequisites found in the issue context and repo-facts block. In live mode, inspect the draft report together with the repository's setup documentation and the issue's stated trigger.

**What good looks like:** A stranger can move from the stated starting state to the tested trigger without guessing a material command, input, route, configuration, or user action. The decision depends on whether the path is executable, not on the number of steps or whether the author used a particular heading or template.

## Behavior shown

**Where it lives:** In an eval bundle, inspect observed output, logs, error excerpts, screenshots, test results, or other artifacts and read them against the trigger and behavior in the issue context. In live mode, inspect the artifacts included or linked in the draft and compare them with the GitHub issue's reported behavior.

**What good looks like:** The evidence shows what occurred at the issue's relevant trigger and distinguishes that result from an adjacent error. For a successful reproduction, the artifact supports the reported failure. For a cannot-reproduce result, the artifact or exact recorded observation shows that the trigger was exercised and a different result occurred.

## Honesty

**Where it lives:** Compare the report's expected behavior, observed behavior, final result, environment differences, and artifacts. Also compare factual assertions in the claim and report with the issue thread and the work the author says they performed.

**What good looks like:** The conclusion is no stronger than the evidence. A package may honestly report reproduced, partially reproduced, or cannot reproduce, but it identifies uncertainty and material environment differences. A confident reproduction claim fails when its evidence demonstrates a different trigger, a different failure, or no observed failure.

## Comms

**Where it lives:** In an eval bundle, compare the claim and repro comments with the issue context, repo-facts block, contribution policy, comment templates, and disclosure requirements. In live mode, inspect the issue thread, repository documentation, and the student's exact draft text.

**What good looks like:** The claim names the issue-specific investigation, states the next action, and promises a later report without promising a fix or deadline. The repro comment reports the author's own work rather than piggybacking on another commenter. Every applicable repository convention is satisfied, including an explicit AI-assistance disclosure when the repository policy requires one.
