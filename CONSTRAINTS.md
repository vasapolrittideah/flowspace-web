# Constraints

Last reviewed: 2026-11-07 by @vasapolrittideah

## Floor

| ID | Rule |
| --- | --- |
| F1 | No new suppression comments, including `@ts-ignore`, `@ts-expect-error`, `eslint-disable`, `istanbul ignore`, `v8 ignore`, or security-tool allow comments. |
| F2 | No unimplemented stubs, empty catches, or placeholder TODOs in source. |
| F3 | No skipped or deleted tests, or removed assertions, without a reviewed exception. |
| F4 | No secrets in source; findings are always reported with values redacted. |
| F5 | This file does not get weakened to make a change pass. Tightening is allowed; exceptions require an owner and expiry. |

The floor blocks immediately. No script enforces it yet, so each review reads the diff for F1 to F5 until the PR that adds the first pnpm scripts also adds a floor check and a secret scan.

## Enforced with numbers

None yet. The repository has no application code, pnpm scripts, or CI, so no numbered constraint has a command that can run.

## Planned

Each row moves to Enforced with numbers in the PR that adds its command and shows that the command passes. Until then, a row is a target, not a check.

| ID | Dimension | Rule | Checked by | Runs at | Reason |
| --- | --- | --- | --- | --- | --- |
| C1 | Formatting | Zero formatting differences | `pnpm format:check` (Prettier) | every edit | Formatting is mechanical, so any difference should be fixed rather than reviewed. |
| C2 | Lint | Zero findings from the repository ESLint configuration | `pnpm lint` | every edit, CI | A configuration that starts at zero findings stays useful only while it stays at zero. |
| C3 | Types | Zero TypeScript errors in strict mode | `pnpm typecheck` (`tsc --noEmit`) | every edit, CI | Strict types also check the generated API types against their use. |
| C4 | Tests | Zero Vitest failures | `pnpm test` | task end, CI | A failing test means the change is not ready. |
| C5 | Changed-line coverage | Added executable lines ≥ 80% covered | `pnpm coverage` with the diff | task end, CI | 80% forces meaningful regression coverage while permitting small configuration changes. It matches the backend threshold. |
| C6 | Secrets | Zero findings | `gitleaks detect --redact` | every edit, CI | Any credential in the working tree is unacceptable, and redaction prevents a finding from becoming another leak. |
| C7 | Accessibility | Zero critical or serious axe violations | `@storybook/addon-a11y` for components and `@axe-core/playwright` for pages | task end for components, CI for pages | Critical and serious violations block real users, while moderate and minor findings are often debatable. axe is an external check that the agent cannot argue with. |
| C8 | Web performance | LCP ≤ 2500 ms and CLS ≤ 0.1 on each public page and the authenticated shell | Lighthouse against the preview deployment | CI after preview deployment | These are the Core Web Vitals "good" thresholds. Lighthouse needs a running URL, so it runs against a preview and not in the edit loop. |

## Lifecycle

| Phase | Command | Budget |
| --- | --- | --- |
| Edit | `pnpm check:fast` | Under 5 seconds after tools are installed and warm. |
| Task end | `pnpm check:task` | At most 90 seconds locally. |
| CI | GitHub Actions | Unrestricted, within free-tier limits. |

These commands do not exist yet. The PR that adds the first application code adds them with the rows that they enforce.

## Measured, not yet enforced

| Metric | Today | Direction |
| --- | --- | --- |
| Project coverage | No tests yet | Record the first measured value, then it must not fall by more than 0.5 percentage points. |
| JavaScript sent by each route | No build yet | Record the first production build, then it must not grow without a reviewed reason. |

## Exceptions

None.

Exceptions expire within 90 days because that is long enough to schedule a repair and short enough to keep the debt visible.
