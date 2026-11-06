# Repository instructions

Follow these repository instructions for every task. Before making changes, read [`CONSTRAINTS.md`](CONSTRAINTS.md), [`GLOSSARY.md`](GLOSSARY.md), and the matching convention in the table below. Do not weaken the constraints to make a change pass. For product, architecture, or implementation work, also read [`docs/architecture.md`](docs/architecture.md), [`docs/project-structure.md`](docs/project-structure.md), [`docs/backend-integration.md`](docs/backend-integration.md), relevant [architecture decision records (ADRs)](docs/adr/README.md), and [`docs/technology-stack.md`](docs/technology-stack.md). Treat open architecture proposals as undecided until explicitly approved.

## Delivery authority

- Prepare and test changes on a branch, then submit them for review through a pull request (PR).
- Do not push directly to `main`, merge a PR, or enable auto-merge.
- The maintainer reviews and squash merges each PR.

## Git workflow

1. Inspect the working tree and read the relevant project documents.
2. Preserve work that is outside the task, as the [pull request workflow](docs/conventions/pull-requests.md#workflow) states.
3. For new work, create a short-lived branch from the current `main`. When updating a PR, continue on its branch.
4. Keep `main` ready for deployment.
5. Make small changes and test each change.
6. Create checkpoint commits for the tested changes.
7. Keep unrelated refactoring and formatting separate from behavior changes.
8. Run the local checks that the pull request workflow names.
9. Push the branch. Open a PR to `main` for new work, or update the existing PR. Then wait for CI, as the pull request workflow states.
10. Address review comments, as the pull request workflow states.
11. After the maintainer merges the PR, remove the branch if it contains no work to preserve.

Keep each branch and PR limited to one reviewable change. Split unrelated changes into separate PRs.

If tasks run at the same time, use a separate worktree for each task.

## Conventions

Read the matching convention before writing or updating an artifact. If a skill gives a different rule, template, or file path, follow the project convention and [`CONSTRAINTS.md`](CONSTRAINTS.md).

The groups below follow the order of the work, from planning to delivery.

### All work

| Work | Convention |
| --- | --- |
| Writing or updating Markdown, English prose, or code comments | [Markdown and English prose](docs/conventions/markdown-and-english-prose.md) |
| Writing or updating term definitions in `GLOSSARY.md` | [Glossary entries](docs/conventions/glossary-entries.md) |
| Writing or updating convention files in `docs/conventions/` | [Convention files](docs/conventions/convention-files.md) |

### Planning

| Work | Convention |
| --- | --- |
| Writing or updating architecture decision records in `docs/adr/` | [Architecture decision records](docs/conventions/adrs.md) |

### Delivery

| Work | Convention |
| --- | --- |
| Creating a short-lived branch for a pull request | [Branch names](docs/conventions/branch-names.md) |
| Writing checkpoint commit messages | [Commit messages](docs/conventions/commit-messages.md) |
| Creating or updating pull requests | [Pull requests](docs/conventions/pull-requests.md) |
