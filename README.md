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
| [Architecture Decision Records](docs/adr/README.md) | Accepted decisions and their rationale |
| [Repository instructions](AGENTS.md) | The Git workflow and the conventions for each kind of artifact |
| [Glossary](GLOSSARY.md) | Terms with a project-specific meaning |

## Design tools

Agents read and edit Figma designs through 2 MCP servers that need no paid Figma seat. The [Technology stack](docs/technology-stack.md) lists their roles.

Framelink reads designs. [`.mcp.json`](.mcp.json) starts it with a pinned version, and it reads a Figma personal access token from your environment:

1. In Figma, create a personal access token with read access to file content.
2. Set the token as `FIGMA_API_KEY` in your shell profile. Do not put it in a repository file.

Claude Talk to Figma edits designs. [`.mcp.json`](.mcp.json) starts its MCP server with a pinned version. Its WebSocket server and Figma plugin run from a local clone of its repository at the same version:

1. Clone `https://github.com/arinspunk/claude-talk-to-figma-mcp` outside this repository, and check out tag `v1.0.0`.
2. Run `bun install` and then `bun run build` in the clone.
3. Run `bun run socket` in the clone. The server listens on port 3055.
4. In the Figma desktop app, select Plugins > Development > Import plugin from manifest, and select `src/claude_mcp_plugin/manifest.json` in the clone.
5. Run the plugin in Figma, copy its channel ID, and tell the agent to connect to Figma on that channel.

Do not use the `npx claude-talk-to-figma-mcp` launcher of its README. It clones the latest commit instead of the pinned version and can install Bun with a remote script.
