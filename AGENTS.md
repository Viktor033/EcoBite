# AGENTS.md

## Scope
- Active code is `frontend/` only. `backend/` is empty (`backend/.gitkeep`). Root has no monorepo workspaces — run all commands from `frontend/`.

## Frontend Stack (verified)
- Vite 8 + React 19 + TypeScript 6 (project references) + Tailwind CSS 4 via `@tailwindcss/vite` (no `tailwind.config.js`).
- UI: shadcn (style `base-vega`, `rsc: false`) + Base UI (`@base-ui/react`) + `lucide-react` + `@fontsource-variable/inter`. CSS entry `frontend/src/index.css` imports `tailwindcss`, `tw-animate-css`, `shadcn/tailwind.css` — do not remove order.
- Lint: `oxlint` (`.oxlintrc.json`), not ESLint. TS build uses `tsc -b` with references (`tsconfig.json` -> `tsconfig.app.json` + `tsconfig.node.json`).

## Package Manager
- `pnpm` only (`frontend/pnpm-lock.yaml`, `frontend/pnpm-workspace.yaml` with `packages: []`). Do not use `npm`/`yarn`. Use `pnpm dlx shadcn@latest ...` for shadcn.

## Commands (run from `frontend/`)
- `pnpm dev` — Vite dev server
- `pnpm build` — `tsc -b && vite build` (typecheck first, then build)
- `pnpm lint` — `oxlint`
- `pnpm preview` — serve built app
- `pnpm dlx shadcn@latest add <component>` — add shadcn components (e.g. `button`)

## Path Alias — Do Not Break
- Alias `@/*` -> `./src/*` must be in **both** `frontend/tsconfig.app.json` and `frontend/tsconfig.json` (root `tsconfig.json` is `files:[]` with references; shadcn validates it) with `baseUrl: "."` and `ignoreDeprecations: "6.0"` (TS 6 deprecates `baseUrl`).
- Vite alias in `frontend/vite.config.ts`: `resolve.alias["@"] = path.resolve(__dirname, "./src")` requires `import path from "path"` and `@types/node` (already in devDeps). Keep in sync with tsconfig or shadcn/build will fail.

## shadcn Config (`frontend/components.json`)
- `tailwind.css: "src/index.css"`, `baseColor: "neutral"`, `cssVariables: true`, `iconLibrary: "lucide"`, aliases: `components:@/components`, `utils:@/lib/utils`, `ui:@/components/ui`, `lib:@/lib`, `hooks:@/hooks`.
- Helper `cn()` lives in `frontend/src/lib/utils.ts` (`clsx` + `tailwind-merge`). Import UI as `@/components/ui/<name>`.

## Project Structure
- Entrypoint: `frontend/src/main.tsx` -> `frontend/src/App.tsx` (currently imports `@/components/ui/button`).
- `frontend/src/components/`, `frontend/src/pages/`, `frontend/src/services/`, `frontend/src/context/`, `frontend/src/lib/`, `frontend/src/assets/`.

## Gotchas
- No tests, no CI workflows, no `opencode.json` — do not assume test runner.
- `frontend/pnpm-workspace.yaml` is empty; do not add packages without reason.
- Tailwind 4: no config file; theme vars are in `frontend/src/index.css` (`@theme inline`, CSS variables). Edit there, not in a JS config.
