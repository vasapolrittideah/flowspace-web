# Project structure

Status: accepted direction with implementation details still open.

Updated: 2026-11-06.

This document defines where frontend code belongs and which dependency directions are allowed. It applies the frontend boundaries from the [Architecture](architecture.md), the feature layout from [ADR-0005](adr/0005-feature-owned-code-and-thin-routes.md), and the tools in the [Technology stack](technology-stack.md).

## Principles

- Organize product code by feature, not by global technical layer.
- Keep App Router files thin: route entry points compose features and framework behavior.
- Colocate a feature's components, API functions, queries, schemas, stores, tests, stories, handlers, and fixtures.
- Keep shared code smaller than feature code and give every shared directory a narrow owner.
- Prefer component state and URL state before introducing Zustand state.
- Never edit generated API types by hand.
- Create directories only when their first real file is added; the tree below is a target, not empty scaffolding.

## Target tree

```text
.
├── .github/
│   └── workflows/
│       └── ci.yml
├── .storybook/
│   ├── main.ts
│   └── preview.ts
├── docs/
│   ├── adr/
│   ├── architecture.md
│   ├── backend-integration.md
│   ├── project-structure.md
│   └── technology-stack.md
├── e2e/
├── public/
├── src/
│   ├── app/
│   │   ├── (public)/
│   │   ├── (auth)/
│   │   ├── (app)/
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── providers.tsx
│   ├── components/
│   │   └── ui/
│   ├── features/
│   │   ├── workspaces/
│   │   ├── projects/
│   │   ├── tasks/
│   │   ├── comments/
│   │   └── notifications/
│   ├── generated/
│   │   └── api.ts
│   ├── lib/
│   │   ├── api/
│   │   ├── auth/
│   │   ├── env/
│   │   └── query/
│   └── mocks/
│       ├── browser.ts
│       └── server.ts
├── .env.example
├── eslint.config.mjs
├── next.config.ts
├── package.json
├── playwright.config.ts
├── pnpm-lock.yaml
├── postcss.config.mjs
├── prettier.config.mjs
├── tsconfig.json
└── vitest.config.ts
```

Framework and tool configuration stays where the owning tool expects it. Add files such as Sentry instrumentation or Renovate configuration only when that integration is configured.

## Directory ownership

| Path | Owner and contents |
| --- | --- |
| `src/app/` | Next.js routes, layouts, metadata, route loading/error states, global CSS, and root provider composition. |
| `src/features/` | Product capabilities and their feature-owned UI, data access, state, validation, tests, stories, and mocks. |
| `src/components/ui/` | Selected AlignUI source copied into the repository and maintained locally. |
| `src/components/` | Reusable composed UI with no feature-specific data access or business workflow. |
| `src/generated/` | Machine-generated OpenAPI transport types. |
| `src/lib/api/` | Shared Fetch transport, structured API errors, deadlines, and request metadata. |
| `src/lib/auth/` | Flowspace Identity session state and browser-session integration. |
| `src/lib/env/` | Parsed and validated public runtime configuration. |
| `src/lib/query/` | Shared TanStack Query client construction and safe defaults. |
| `src/mocks/` | MSW browser/server setup and composition of feature-owned handlers. |
| `.storybook/` | Storybook configuration and global decorators. |
| `e2e/` | Playwright tests and browser-level support code. |
| `public/` | Static files served unchanged by Next.js. |
| `docs/` | Architecture, integration, decision, and operating documentation. |

`src/lib/` is not a general-purpose utility directory. Each child represents a named external or cross-cutting boundary. A file with one consumer stays beside that consumer.

## Route structure

Route groups express rendering and access boundaries without changing public URLs:

- `(public)` contains public pages that may use server rendering.
- `(auth)` contains browser authentication callbacks and transition screens.
- `(app)` contains the authenticated client application.
- The root `layout.tsx` owns the document shell.
- The root `providers.tsx` is the single client boundary that composes the authentication and TanStack Query providers.

Route directories should primarily contain Next.js special files such as `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`, and `not-found.tsx`. A `page.tsx` imports and composes feature code; it does not become the feature implementation.

Do not add Route Handlers as proxies for Go operations. A `route.ts` requires a separate platform need rather than convenience.

## Feature structure

A feature starts flat and gains subdirectories only when real files become difficult to navigate. A task feature may grow toward this shape:

```text
src/features/tasks/
├── task-api.ts
├── task-queries.ts
├── task-schema.ts
├── task-form.tsx
├── task-form.stories.tsx
├── task-form.test.tsx
├── task.handlers.ts
├── task.fixtures.ts
└── task-store.ts
```

Not every feature needs every file:

- `*-api.ts` contains feature-specific requests built on `src/lib/api/`.
- `*-queries.ts` contains query keys, query options, and mutations.
- `*-schema.ts` contains Zod schemas required at runtime.
- `*-store.ts` exists only for feature-owned client state that cannot stay local or in the URL.
- `*.handlers.ts` and `*.fixtures.ts` contain MSW scenarios and test data owned by the feature.
- `*.test.tsx` and `*.stories.tsx` stay beside the component they describe.

Split a feature into named subdirectories only after the flat layout stops being clear. Do not pre-create `components/`, `hooks/`, `types/`, or `utils/` inside every feature.

## Dependency direction

Production code follows this direction:

```text
src/app
  └── src/features
        ├── src/components
        │     └── src/components/ui
        ├── src/lib
        │     └── src/generated
        └── src/generated
```

Rules:

- Routes may import features and shared modules.
- Features may import shared components, named library boundaries, and generated API types.
- Shared components never import features.
- Shared libraries never import features or routes.
- Generated code imports no handwritten application module.
- Features do not reach into another feature's internal files; routes compose features at product boundaries.
- Test and Storybook code may import MSW setup and feature-owned handlers; production modules never import test or mock code.

When two features genuinely need the same code, first compose them at the route. Promote code to a narrowly named shared owner only after duplication or a stable shared contract exists.

## State placement

| State | Location |
| --- | --- |
| One component or subtree | Local React state beside the component |
| Shareable navigation, filters, or pagination | URL parameters managed by the route or feature |
| Go API resources and mutations | Feature-owned TanStack Query code |
| Form fields, dirty state, and submission | Feature-owned React Hook Form code |
| Runtime input validation | Feature-owned Zod schemas |
| Cross-component client-only state | Feature-owned Zustand store |
| Authentication tokens and session readiness | Authentication integration in `src/lib/auth/` |

Do not copy API resources into Zustand, copy form fields into Zustand, or expose authentication tokens through a global store.

## API and generated code

- `src/generated/api.ts` is produced by `openapi-typescript` from the backend's Protobuf-derived OpenAPI contract.
- Generated types describe transport shapes; feature code gives those shapes product meaning.
- `src/lib/api/` contains only shared transport behavior.
- Each feature owns its endpoint functions, query keys, cache policy, mutations, and user-facing error handling.
- No global `services/` directory mirrors backend services.
- No handwritten `types/` directory duplicates generated API models.

The API contract acquisition, version pinning, and generated-file verification workflow remains an API ADR decision.

## Components and Storybook

- `src/components/ui/` contains copied AlignUI primitives, including local accessibility-preserving changes.
- A component used by one feature stays in that feature.
- A presentation component used across features may move to `src/components/`.
- Shared components accept data and callbacks; they do not fetch feature data themselves.
- Reusable UI receives colocated stories for meaningful variants, interaction states, and accessibility checks.
- One-off page sections and route compositions do not require stories.

Do not copy the full AlignUI catalog. Add a component and its required dependencies when a real screen uses it.

## Tests and mocks

- Colocate Vitest and React Testing Library files as `*.test.ts` or `*.test.tsx`.
- Colocate Storybook files as `*.stories.tsx`.
- Keep feature fixtures and MSW handlers with the feature that owns the contract scenario.
- Keep `src/mocks/` limited to MSW setup and handler composition shared by Storybook and tests.
- Keep Playwright specifications under `e2e/` because they exercise routes and deployed boundaries.
- Test support code follows the same ownership rule as production code; do not create a global `test-utils` bucket without multiple concrete consumers.

## Placement checklist

Place a new file by asking these questions in order:

1. Is it a Next.js route or framework-required route file? Put it in `src/app/`.
2. Does it implement one product capability? Put it in that feature.
3. Is it copied AlignUI source? Put it in `src/components/ui/`.
4. Is it reusable presentation with no feature ownership? Put it in `src/components/`.
5. Is it generated? Put it in `src/generated/`.
6. Does it integrate a shared external or browser boundary? Put it in a narrowly named `src/lib/<boundary>/` directory.
7. Does it only compose MSW infrastructure? Put it in `src/mocks/`.
8. Otherwise, keep it beside its only caller until another concrete owner appears.

## Prohibited generic layers

Do not create these global dump directories:

```text
src/hooks/
src/services/
src/stores/
src/types/
src/utils/
```

Hooks, request functions, stores, types, and helpers are valid, but they stay with their owning feature or named shared boundary. A generic suffix does not establish ownership.

Also avoid:

- barrel files that hide dependency direction or introduce cycles;
- one-file abstractions created only to preserve a folder pattern;
- feature directories containing empty standard subdirectories;
- duplicate domain models beside generated API types;
- page files that accumulate feature implementation; and
- importing mock, Storybook, or test modules into production code.

## Growth rule

This structure grows by evidence:

- Add a directory when its first file has a clear owner.
- Extract shared code after a second real consumer demonstrates the shared contract.
- Split a feature when it contains independently changing product capabilities, not because it reaches a file-count threshold.
- Add a new top-level boundary only through an architecture decision when existing ownership rules cannot place the code cleanly.
