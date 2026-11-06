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

Talk to Figma edits designs. Its WebSocket server and Figma plugin come only from its repository, so each developer runs it from a local clone at the reviewed commit:

1. Clone `https://github.com/grab/cursor-talk-to-figma-mcp` outside this repository, and check out commit `ddd90f3a6d454ea0b2fc29f1b084f50fd062b880`.
2. Run `bun install` in the clone.
3. Run `bun socket` in the clone. The server listens on port 3055.
4. In the Figma desktop app, select Plugins > Development > Import plugin from manifest, and select `src/cursor_mcp_plugin/manifest.json` in the clone.
5. Add the MCP server only for your own checkout with `claude mcp add talk-to-figma --scope local -- bun run <clone path>/dist/server.js`.
6. Run the plugin in Figma, and give the agent the channel name that the plugin shows.
