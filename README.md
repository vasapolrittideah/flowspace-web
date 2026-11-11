# Flowspace Web

Flowspace Web is the browser application of Flowspace, a work-management app. When it is complete, teams will use it to share workspaces, projects, tasks, comments, and notifications. The project is also a place to learn production-grade frontend engineering.

## Project status

The repository has design documents but no application code yet. The browser calls the public Go APIs of the [Flowspace API](https://github.com/vasapolrittideah/flowspace-api) repository, which owns the API contracts.

## Documents

| Document | Contents |
| --- | --- |
| [Architecture](docs/architecture.md) | Frontend boundaries, browser behavior, API consistency, delivery, and learning evidence |
| [Project structure](docs/project-structure.md) | Where code belongs and which dependency directions are allowed |
| [Technology stack](docs/technology-stack.md) | Replaceable tools and the ADRs that they support |
| [Backend integration](docs/backend-integration.md) | The API contract source, request rules, origins, and backend gaps |
| [Constraints](CONSTRAINTS.md) | The quality bar that each change must meet |
| [Architecture Decision Records](docs/adr/README.md) | Accepted decisions and their rationale |
| [Repository instructions](AGENTS.md) | The Git workflow and the conventions for each kind of artifact |
| [Glossary](GLOSSARY.md) | Terms with a project-specific meaning |

## Design tools

Agents read and edit Penpot designs on Penpot Cloud through the Penpot MCP server. The [Technology stack](docs/technology-stack.md) lists its role. [`.mcp.json`](.mcp.json) connects to the remote server at `design.penpot.app`, and it reads your Penpot MCP key from your environment:

1. In Penpot, open Your account > Integrations > MCP Server, turn on Status, and generate an MCP key. Penpot shows the key only 1 time.
2. Set the key as `PENPOT_MCP_KEY` in your shell profile, and start Claude Code from a shell that has it. Do not put it in a repository file, because the key is part of the server URL.
3. Open a design file in Penpot, and select File > MCP Server > Connect.
4. Keep that Penpot tab open while the agent works. The agent can change only the page that has focus in that tab.

If you generate a new key, Penpot revokes the old key. Update `PENPOT_MCP_KEY` after you do this.
