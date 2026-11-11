# Technology stack

These tables are the source of truth for replaceable implementation tools. These selections support the frontend architecture but do not each warrant an ADR. Replace one when measured limitations or maintenance cost justify it, while preserving the owning ADR's contract.

Each section groups the tools for one concern. A row with no owning ADR shows a dash in the ADR column.

## Language and runtime

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| Runtime | Node.js active LTS | Frontend toolchain and Next.js runtime. | — |
| Package manager | pnpm | Reproducible dependency installation and lockfile management. | — |
| Application language | TypeScript in strict mode | Static typing for application and generated API code. | [ADR-0012](adr/0012-frontend-types-come-from-openapi.md) |

## Application framework

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| Application framework | Next.js App Router | Routing, layouts, rendering, builds, and application delivery. | [ADR-0002](adr/0002-public-ssr-and-protected-client-rendering.md), [ADR-0005](adr/0005-feature-owned-code-and-thin-routes.md) |
| UI runtime | React | Component model and browser interactivity. | [ADR-0002](adr/0002-public-ssr-and-protected-client-rendering.md) |

## UI system

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| Styling | Tailwind CSS | Design tokens, responsive layouts, and component styling. | — |
| Component foundation | AlignUI v1.2 source | Accessible component source copied as needed and maintained locally. | — |
| Component workshop | Storybook | Isolated development and documentation for reusable components. | — |

## State and forms

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| Server state | TanStack Query | Cache, synchronize, and mutate API data. | [ADR-0021](adr/0021-one-cache-owns-api-state.md) |
| Shared client state | Zustand | Client-only state shared across components. | [ADR-0022](adr/0022-client-state-uses-narrowest-owner.md) |
| Form state | React Hook Form | Form registration, state, validation execution, and submission. | [ADR-0022](adr/0022-client-state-uses-narrowest-owner.md) |
| Client validation | Zod | Runtime schemas for form input and other untrusted client values. | [ADR-0023](adr/0023-backend-validation-and-errors-are-authoritative.md) |

## APIs and contracts

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| API transport | Browser Fetch API | Typed REST/JSON requests to the Go API. | [ADR-0003](adr/0003-browser-calls-go-apis-directly.md), [ADR-0013](adr/0013-one-shared-transport-boundary-for-feature-api-clients.md) |
| API type generation | openapi-typescript | Generate TypeScript types from the Protobuf-derived OpenAPI contract. | [ADR-0012](adr/0012-frontend-types-come-from-openapi.md) |
| API mocking | Mock Service Worker | Shared API mocks for development, Storybook, and tests. | [ADR-0024](adr/0024-mocks-do-not-replace-real-api-tests.md) |

## Authentication and browser security

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| Browser authentication | Flowspace Identity REST/JSON API | Login, refresh, and logout requests; browser session handling remains an open proposal. | [ADR-0025](adr/0025-flowspace-identity-replaces-keycloak-for-browser-authentication.md), [ADR-0007](adr/0007-authentication-tokens-stay-in-memory.md), [ADR-0008](adr/0008-protected-ui-waits-for-authentication.md) |
| Browser security | Content Security Policy + Next.js response headers | Browser security policy and HTTP security headers. | [ADR-0011](adr/0011-browser-trust-boundaries-restrict-exposure.md) |

## Delivery

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| Frontend hosting | Vercel | Next.js preview and production deployments. | [ADR-0004](adr/0004-frontend-deploys-separately-from-backend.md) |

## CI and quality

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| Source and CI | GitHub + GitHub Actions | Source hosting and automated frontend quality gates. | — |
| Static analysis | ESLint | Next.js, React, dependency-specific, and project correctness rules. | — |
| Formatting | Prettier + `prettier-plugin-tailwindcss` | Deterministic formatting and Tailwind class ordering. | — |
| Dependency updates | Renovate | Propose reviewed dependency updates without automerge. | — |
| Unit test runner | Vitest | Fast tests for frontend logic and components. | — |
| Component testing | React Testing Library | User-facing component behavior tests. | — |
| End-to-end testing | Playwright | Critical workflows in real browsers. | [ADR-0024](adr/0024-mocks-do-not-replace-real-api-tests.md) |
| Accessibility automation | `@storybook/addon-a11y` + `@axe-core/playwright` | Automated component- and page-level accessibility checks. | — |

## Design

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| Design tool | Penpot (Penpot Cloud) | Screen designs. | — |
| Design access for agents | Penpot MCP server (remote, Penpot Cloud) | Read and edit Penpot designs through the Penpot plugin in an open browser tab; replacement must not need a paid Penpot plan. | — |

## Observability

| Area | Selection | Role / replacement boundary | ADR |
| --- | --- | --- | --- |
| Client error monitoring | Sentry | Browser error collection, release tracking, and source maps. | [ADR-0011](adr/0011-browser-trust-boundaries-restrict-exposure.md) |
| Real-user performance | Vercel Speed Insights | Deployed Core Web Vitals measurement. | — |
