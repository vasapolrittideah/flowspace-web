# Commit message conventions

This convention defines the message of each checkpoint commit that people and agents write on a branch. The squash commit that merges a pull request (PR) into `main` has only the PR title, and the PR title follows the subject rules of this convention. Commits that Renovate creates follow its own configuration, but the title of a Renovate PR follows the subject rules.

The [Repository instructions](../../AGENTS.md) state when to create checkpoint commits and who squash merges a PR.

## Template

A message has a type, a scope, a breaking marker, a description, a body, Issue footers, and co-author trailers.

```text
<type of change>(<scope, if any>)<breaking marker, if any>: <description of the change>

<body, if any>

<Issue footer, if any>

Co-authored-by: <co-author name, if any> <<co-author email>>
```

### Type

- Use the type of the main change.
- If a change only edits Markdown agent instructions, conventions, skills, or commands, use `docs`. If it changes code or configuration for agents, such as a script or a hook, use the type of that change.
- Use one of the [Types](#types).
- Do not use `feat` only because the change adds a file.

### Scope

- Choose the scope with these steps, and stop after the first match:
  1. Use the first scope in the [Scopes](#scopes) table whose `Paths` column covers all of the main change. A row can cover a whole path or 1 part of a file, such as a script in `package.json`.
  2. If no row covers all of the main change, omit the scope.
- Use the feature scope for a change in 1 feature, also when the change includes the tests, stories, MSW handlers, and fixtures of that feature.
- Use `api` for an update of the generated API types. Use `codegen` only when the generation configuration or tooling is the main change.
- If a PR adds a feature directory under `src/features/` or a boundary directory under `src/lib/`, add its scope and paths to the [Scopes](#scopes) table in the same PR.
- Do not combine scope names.

### Breaking marker

- Write the marker as `!` directly before the colon.
- If a change breaks a public route URL that users or other systems depend on, add the marker.
- Do not add the marker for other incompatible changes, such as a renamed environment variable. Explain them in the [Breaking changes](pull-requests.md#breaking-changes) section of the PR description.

### Description

- Start the description with a lowercase verb in the imperative mood, such as `add` or `reject`.
- Do not end the description with a period.
- Name the specific change, such as `reject task updates based on an outdated version`. If you use a general verb, such as `update`, give it a specific object, such as `update Next.js to 16`.
- Do not write a description that names no change, such as `misc` or `fix things`.

### Body

- Put the body after the subject, separated by a blank line.
- Wrap prose lines at 72 characters.
- Separate paragraphs with 1 blank line.
- Start each bullet item with a hyphen and a space, and indent its wrapped lines by 2 spaces.
- Start each numbered item with its number, a period, and a space, and indent its wrapped lines by 3 spaces.
- In a checkpoint commit, write a body only when the subject does not explain the step. Write for a reviewer who reads the PR 1 commit at a time. Explain what the step changes, and the reason or trade-off when the subject does not make it clear. You can name the tests that the step adds, because they are part of the step.
- If a checkpoint subject matches the PR title, apply the checkpoint body rule above. This also applies to the only commit of a PR.
- Do not repeat text or add process history, abandoned methods, hypothetical objections, or unrelated files.
- Do not refer to the commit, the writer, or the time of writing, such as "This commit adds", "I", "we", "now", or "currently". State each change directly. The required first body line in [Commits that revert a commit](#commits-that-revert-a-commit) is an exception.

### Issue footers

- Put the footers after the body, or after the subject when there is no body, separated by a blank line.
- Write each footer as `Refs: #<Issue number>`.
- Do not leave blank lines between Issue footers.
- Do not add a period at the end of an Issue footer.
- Write 1 footer per Issue, and repeat the key on each line.
- Order the footers by ascending Issue number.
- If a checkpoint commit belongs to an Issue, add a `Refs` footer for it.
- Do not use `Closes` in a commit. The [Related issues](pull-requests.md#related-issues) section of the PR description closes each Issue when the PR merges.
- Do not add Issues from [Follow-up tasks](pull-requests.md#follow-up-tasks).

### Co-author trailers

- Put the trailers after a blank line at the end of the message.
- Spell the trailer key as `Co-authored-by`.
- Order the trailers by name in alphabetical order.
- Add 1 trailer for each person or agent, other than the commit author, who contributed to the commit.
- For a person, use their name and the email that GitHub links to their account, such as their `users.noreply.github.com` address. For an agent, use the name and email in the attribution trailer that its harness gives. If the harness gives none, use the identity in [Agent trailers](#agent-trailers).
- Keep the existing trailers when you amend or squash commits. GitHub copies the trailers of the checkpoint commits to the squash commit.
- Do not add a trailer for a person or agent who did not contribute.

## Rules

### Format and content

- Write the whole message as plain text, including text that comes from a PR description. Do not use backticks, Markdown links, or URLs. Name a file by its path, and refer to an Issue or a pull request by its number, such as `#123`.
- Keep the whole subject line at most 72 characters. Do not count the `(#<number>)` suffix and its leading space, which GitHub adds to a squash commit.
- Write the parts in the order shown in the [Template](#template).
- Follow [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/).
- Do not split code or trailers. A line that holds one of them can be longer than 72 characters.

### Workflow

- If a message has more than 1 line, write it to a file outside the working tree, and pass the file with `git commit --file <message-file>` in its own command.

### Changes

- Apply a change of this convention only to new commits and to commit messages that a later PR changes. Do not rewrite commits on `main` to follow it.

## Commits that revert a commit

Follow these rules for each commit that reverts an earlier commit. The [What changed](pull-requests.md#what-changed) section of the PR description names the reverted commit for the squash commit.

- Start the body with `This reverts commit <full SHA>.` Then give the reason in a new paragraph.
- Use the scope and the description of the reverted commit.
- Use the `revert` type.
- Do not copy the breaking marker of the reverted commit. Add the marker only when the revert itself breaks callers, as the [Breaking marker](#breaking-marker) rules state.

## Differences from the git-workflow-and-versioning skill

This convention applies where it differs from the [Git Workflow and Versioning](../../.agents/skills/git-workflow-and-versioning/SKILL.md) skill:

- Follow the [Scope](#scope) rules. The skill writes `<type>: <description>` without a scope.
- Follow the [Body](#body) rules for checkpoint commits. The skill explains only why a change was made.
- Follow the [Git workflow](../../AGENTS.md#git-workflow) of the repository instructions. The skill advises against squashing commits.
- Follow the [Type](#type) rules. The skill lists 6 types and has no `perf`, `build`, `ci`, `style`, or `revert` type.

## Reference

### Types

| Type | Purpose |
| --- | --- |
| `feat` | Add functionality |
| `fix` | Correct a bug |
| `docs` | Change documentation only |
| `refactor` | Restructure code without changing behavior |
| `test` | Add or correct tests |
| `perf` | Improve performance |
| `build` | Change build tooling, compilation, or packaging |
| `ci` | Change CI workflows or automated checks |
| `style` | Change formatting only |
| `chore` | Maintain the project when no more specific type fits |
| `revert` | Revert an earlier commit |

### Scopes

| Scope | Area | Paths |
| --- | --- | --- |
| `app` | Next.js routes, layouts, metadata, navigation, and root providers | `src/app/` |
| `auth` | Flowspace Identity integration, protected navigation, and browser-session lifecycle | `src/lib/auth/` |
| `workspaces` | Workspaces, memberships, invitations, roles, and workspace switching | `src/features/workspaces/` |
| `projects` | Project views and project-owned workflows | `src/features/projects/` |
| `tasks` | Tasks, assignments, status transitions, and versioned task mutations | `src/features/tasks/` |
| `comments` | Comment views and interactions | `src/features/comments/` |
| `notifications` | In-app notification views, read state, and refresh behavior | `src/features/notifications/` |
| `ui` | Shared components, AlignUI source, Tailwind tokens, and accessibility behavior | `src/components/` |
| `api` | Shared Fetch transport, generated OpenAPI types, and backend integration | `src/lib/api/`, `src/generated/` |
| `query` | Shared TanStack Query client and cache defaults | `src/lib/query/` |
| `config` | Frontend environment and framework configuration | `src/lib/env/`, `.env.example`, `next.config.ts`, `tsconfig.json`, `eslint.config.mjs`, `prettier.config.mjs`, `postcss.config.mjs` |
| `e2e` | Playwright configuration and cross-feature browser test infrastructure | `e2e/`, `playwright.config.ts` |
| `codegen` | Code-generation configuration and tooling | The `openapi-typescript` configuration and its scripts in `package.json` |
| `adr` | Architecture decision records | `docs/adr/` |
| `agents` | Agent instructions, conventions, skills, commands, and configuration | `AGENTS.md`, `CLAUDE.md`, `docs/conventions/`, `.agents/`, `.claude/`, `skills-lock.json` |
| `deps` | Dependency updates | The dependency fields of `package.json`, `pnpm-lock.yaml`, `renovate.json` |

### Agent trailers

| Agent | Trailer |
| --- | --- |
| Codex | `Co-authored-by: Codex <noreply@openai.com>` |
| Claude Code | `Co-authored-by: Claude <noreply@anthropic.com>` |

## Examples

### Subjects

This subject uses `feat` for a new workspace feature:

```text
feat(workspaces): add a workspace switcher
```

This subject uses `fix` for a bug fix in the tasks feature:

```text
fix(tasks): reconcile stale task updates
```

This subject uses `test` for a browser test of a workflow:

```text
test(e2e): cover workspace invitation acceptance
```

This subject uses the `api` scope for an update of the generated API types:

```text
fix(api): preserve correlation IDs in structured errors
```

This subject uses the `adr` scope for an architecture decision record:

```text
docs(adr): explain the choice of squash merging
```

This subject uses `fix` for a bug fix in agent code:

```text
fix(agents): preserve multiline PR descriptions
```

This subject uses the `codegen` scope for code-generation configuration:

```text
build(codegen): verify generated OpenAPI types
```

This subject has no scope, because no row of the [Scopes](#scopes) table covers the CI workflow:

```text
ci: add pull request title validation
```

This subject has a breaking marker for a change that breaks a public route URL:

```text
feat(app)!: move workspace pages under the workspace slug
```

This subject reverts an earlier commit with its scope and description:

```text
revert(agents): clarify convention headings
```

### Checkpoint commits

This checkpoint names the tests of its step and uses `Refs` to keep its Issue open until the PR merges:

```text
feat(tasks): send the expected version with task updates

The task form sends the version that the browser last read, and a
version conflict reloads the task before the user retries.
task-form.test.tsx checks the reload after a conflict response.

Refs: #12

Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>
```

This documentation checkpoint shows a complete message with Codex attribution:

```text
docs(agents): clarify convention headings

Co-authored-by: Codex <noreply@openai.com>
```

### Revert commits

This revert commit keeps the scope and description of the reverted commit and names the reverted commit and the reason in its body:

```text
revert(agents): clarify convention headings

This reverts commit 2f77dde4c1b9a6e3d5f8a0b7c2e4d6f8a1b3c5e7.

The new headings broke links from other convention files.

Co-authored-by: Claude Opus 5.5 <noreply@anthropic.com>
```
