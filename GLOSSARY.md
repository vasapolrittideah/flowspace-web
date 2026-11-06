# Glossary

This file defines each term that has a narrower or different meaning in Flowspace than in general English. The [Glossary entry](docs/conventions/glossary-entries.md) conventions state how to write an entry.

## Writing

| Term | Meaning |
| --- | --- |
| GitHub text | The text of Issues, pull requests, comments, and milestones on GitHub. |
| Markdown text | The text of Markdown files and GitHub text. |

## Conventions

| Term | Meaning |
| --- | --- |
| artifact | An item that the project makes many times, such as a commit message or an architecture decision record. |
| convention file | A file in `docs/conventions/` that tells agents and developers how to write 1 kind of artifact. |
| part | 1 piece of the format of an artifact, such as a heading, a labeled line, or a section. |
| skill | A set of agent instructions in `.agents/skills/`. |

## Architecture decision records

| Term | Meaning |
| --- | --- |
| index | The table of records in the [Architecture Decision Records](docs/adr/README.md) file. |
| record | 1 architecture decision record (ADR). |

## Commits and pull requests

| Term | Meaning |
| --- | --- |
| change that breaks callers | A change that removes or changes the meaning of a public route URL that users or other systems use. |
| checkpoint commit | A commit on a branch that records 1 tested work step. |
| lasting record | The [What changed](docs/conventions/pull-requests.md#what-changed), [Why](docs/conventions/pull-requests.md#why), [Breaking changes](docs/conventions/pull-requests.md#breaking-changes), and [Related issues](docs/conventions/pull-requests.md#related-issues) sections of a PR description. Each squash commit has only the PR title, so these sections hold the details of the change for a later reader of `main`. |
| main change | The change that a commit exists to make. Tests, generated output, and documents that change because of the main change are not part of it. |
| maintainer | The person who reviews and merges each PR. |
| ready | Of a PR: the local checks that the [Workflow](docs/conventions/pull-requests.md#workflow) rules of the pull request convention name and CI pass, and the title and description match the final work. |
| review comment | A comment or a review on a PR in GitHub. Feedback in the chat is not a review comment. |
| task | 1 piece of work that 1 PR completes. |
| work for later | Work that a PR does not do but shows to be needed, such as a gap that the PR finds or a step that its goal still needs. |
