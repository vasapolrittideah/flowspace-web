# Pull request conventions

This convention defines how to write a pull request (PR) and how to prepare it for review.

The [Repository instructions](../../AGENTS.md) state who merges a PR. Each squash commit has only the PR title, so the PR description holds the details of the change.

## Template

A PR has a title, [What changed](#what-changed), [Why](#why), [Breaking changes](#breaking-changes), [Related issues](#related-issues), [Risks or limitations](#risks-or-limitations), [Follow-up tasks](#follow-up-tasks), and an attribution line.

```markdown
<PR title>

## What changed

<changes and result>

## Why

<problem and the reason for the change>

## Breaking changes

<what breaks and what callers must change>

## Related issues

<Issue line>

## Risks or limitations

<revert safety, unresolved failures, checks that did not run, or remaining limits>

## Follow-up tasks

- <remaining work>

<attribution line, if any>
```

### Title

- Use only the subject line of the commit message [Template](commit-messages.md#template), with the same [Types](commit-messages.md#types) and [Scopes](commit-messages.md#scopes).
- Describe the result, not the branch or the changed files.
- Do not include a body or footer.

### What changed

- State the lasting effect of the merged PR on `main` first, for a later reader of `main`. Then state each change that a reviewer must know to review the PR.
- If the PR changes an existing value, such as a configuration value, a default, a limit, or an API field, show the value in a table, also when it is the only changed value. Name each value in the first column, and add a `Before` column and an `After` column.
- If the PR title has the `revert` type, start the section with `Reverts <full SHA>.`, and give the reason in [Why](#why). Write the SHA without backticks, so that GitHub links the reverted commit.
- Do not put a value that the PR adds or removes in the table, because its `Before` or `After` cell has no value. List the added or removed values in bullets.
- Do not list every changed file, because the diff shows them.
- Do not describe process history, such as a rebase or a method that did not work.

### Why

- State the problem that the PR solves and the reason for the chosen change.
- Link each decision that the change depends on, such as an ADR or a specification.

### Breaking changes

- If the PR title has a breaking marker, state what breaks and the changes that callers must make.
- State each other incompatible change, such as a renamed environment variable, and the changes that callers must make.
- If nothing breaks, write `n/a`.

### Related issues

- Write 1 line per Issue, and repeat the keyword on each line, because GitHub ignores an Issue that follows a comma. Put the lines without blank lines between them, and end each line with a period.
- Put the `Closes` lines before the `Refs` lines, and order each group by ascending Issue number.
- Write `Closes #<issue-number>.` for each Issue that the PR completes. GitHub closes the Issue after the PR merges into `main`.
- Write `Refs #<issue-number>.` for each Issue that stays open. An example is a PR that completes only part of the Issue.
- If no Issue is related, write `n/a`.
- Do not use `Fixes`, although GitHub accepts it. Use `Closes` for consistency.
- Do not repeat a follow-up Issue.

### Risks or limitations

- Show the evidence for each unresolved failure with these steps:
  1. Before the PR opens, put the local output that shows the failure in a code block under its bullet, because no CI run exists for the PR.
  2. After the first CI run finishes, replace the output with a link to that run.
- If a revert of the PR is not safe, such as after a migration, state why and what a revert needs.
- If the PR changes a shared component, a shared library boundary, a shared configuration, or a path that more than 1 feature uses, name the features that a failure of the change affects.
- Write each unresolved failure, local check that did not run, security risk, and remaining limit. Write each compatibility effect in [Breaking changes](#breaking-changes).
- For each unresolved failure, state the cause and link the CI run or the output that shows it.
- For each local check that did not run, state the reason.
- When a limitation has follow-up work, describe the limitation and refer to [Follow-up tasks](#follow-up-tasks) instead of repeating the work.
- If there is nothing to report, write `n/a`.
- Do not paste routine logs.
- Do not describe failed attempts that the PR resolved.

### Follow-up tasks

- If the work has an open Issue, write `- #<issue-number>: <remaining work>.` If the work has no open Issue, write `- <remaining work>.`, such as `- Add the Storybook story for the task form.`
- Write 1 bullet for each piece of work for later, even when there is only 1.
- Write the remaining work as 1 sentence that names the work. GitHub shows only the number of an Issue, so the sentence must name the work.
- Put the bullets with an Issue first, in ascending Issue number. Then put the bullets without an Issue.
- Before you create an Issue for the work, ask the maintainer. If the maintainer does not approve it, or the session has no chat, write the work without an Issue number.
- If the PR leaves no work for later, write `n/a`.
- Do not put `Closes`, `Fixes`, or `Resolves` before the Issue number. GitHub closes the Issue when the PR merges.

### Attribution line

- If the agent harness gives an attribution line for PR descriptions, put it at the end of the description, after [Follow-up tasks](#follow-up-tasks).

## Rules

### Format and content

- Start the description from the [`.github/pull_request_template.md`](../../.github/pull_request_template.md) template, and complete every section.
- Delete the HTML comments of the template.
- Write the [lasting record](../../GLOSSARY.md#commits-and-pull-requests) for a later reader of `main`. Write the other sections for the reviewer and the merge decision.
- If a section has content, replace its `n/a`.
- Do not copy CI results or measurements, such as coverage or reachable vulnerabilities, into the description.

### Workflow

- For a documentation-only change, make sure that the facts, examples, links, and formatting are correct. Application tests are unnecessary unless executable behavior changes.
- Keep changes that belong to another task out of your commits, and keep them in the working tree.
- Before each commit, inspect the staged diff. Exclude unrelated changes, secrets, local environment files, and unwanted build output.
- For a behavior fix, add a focused regression test.
- Before you open or update a PR, run each pnpm script for formatting, linting, type checking, tests, the Storybook build, the Next.js build, and Playwright that the change affects. If a script does not exist yet, state the check that did not run in [Risks or limitations](#risks-or-limitations).
- For a user-interface change, check the keyboard behavior and run the automated accessibility checks of the changed components and pages.
- Before you open or update a PR, inspect the complete PR diff with the same exclusions as for a commit.
- After each push, handle CI with these steps:
  1. Wait for the CI checks to finish.
  2. If the job log shows that a failure comes from the runner, the network, or an external service, such as a registry timeout, rerun the failed job 1 time.
  3. If it fails again, state the cause and link the run in [Risks or limitations](#risks-or-limitations).
  4. Fix every other failure in the PR.
- Do not write a squash message. The default squash message of the repository is the PR title with a blank body, and GitHub adds the `Co-authored-by` trailers of the checkpoint commits.
- Follow the [Repository instructions](../../AGENTS.md) for the branch, the PR, and merge authority.
- Do not weaken a command or hide a failure.

### Links and tracking

- Do not set an assignee or a reviewer.
- Do not add the PR to a GitHub Project.

### Status and approval

- Open the PR as a normal PR, not as a draft.
- Only after the PR meets every condition of a ready PR, tell the maintainer that it is ready. If the PR cannot become ready, tell the maintainer which condition fails and why. If the session has a chat, tell the maintainer there. If the session has no chat, tell the maintainer in a PR comment.

### Changes

- When the work changes, update the title and the description before you tell the maintainer that the PR is ready again.
- After the PR merges, edit its description only to fix a wrong fact, because the description is the lasting record of the change.
- If a commit needs a fix and no review comment exists, amend or rebase it. Push the branch with `git push --force-with-lease`. After a review comment exists, add new commits.
- If the PR has merge conflicts or needs a change that is on `main`, merge `main` into the branch.
- Apply a change of this convention to new PRs, and to open PRs when their description changes. Do not edit merged PRs to follow it.
- Do not force-push after a review comment exists.

## Generated API type PR

These rules apply to a PR that updates `src/generated/api.ts` or its generation configuration. [ADR-0012](../adr/0012-frontend-types-come-from-openapi.md) makes the backend OpenAPI contract the source of the generated types.

- Regenerate the types from the backend OpenAPI contract. Do not edit `src/generated/` by hand.
- Commit the generated output with the change of its source or configuration.
- In [What changed](#what-changed), name the backend contract revision that the types come from.

## Differences from the git-workflow-and-versioning skill

This convention applies where it differs from the [Git Workflow and Versioning](../../.agents/skills/git-workflow-and-versioning/SKILL.md) skill:

- Follow the [Template](#template) and the [What changed](#what-changed) rules for the description. The skill writes a summary with `CHANGES MADE`, `THINGS I DIDN'T TOUCH`, and `POTENTIAL CONCERNS`.
- Follow the file list rule in [What changed](#what-changed). The skill names each changed file.

## Examples

This title describes the result instead of the branch:

```text
Branch: fix/duplicate-notifications
Title:  fix(notifications): prevent duplicate delivery when an event is retried
```
