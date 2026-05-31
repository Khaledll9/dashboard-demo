# AGENTS.md — cmp-deep-dive

## Quick start

```bash
npm install          # install dependencies
npm start            # ng serve on http://localhost:4200/
npm test             # ng test via Karma + Jasmine
npm run build        # production build → dist/cmp-deep-dive
```

## Architecture

- **Angular 18 standalone app** — no `NgModule`s. `src/main.ts` bootstraps `AppComponent` directly.
- **No router** — single-page dashboard. No `provideRouter` or routing setup.
- **No linter/formatter config** — no ESLint, Prettier, or stylelint config present.
- **`.editorconfig`** enforces 2-space indent, single quotes for TS, final newline.

## Component conventions

- Each component lives in its own folder with three files: `*.component.ts`, `*.component.html`, `*.component.css`.
- All components are **standalone** with `templateUrl` (not inline).
- Component selectors use `app-` prefix (e.g., `app-header`, `app-dashboard-item`).

## Project structure

```
src/
  main.ts                   # app entry (bootstrapApplication)
  index.html                # root HTML, uses <app-root>
  styles.css                # global styles (no SCSS)
  app/
    app.component.ts/html   # root component
    header/                 # header component
    dashboard/
      dashboard-item/       # wrapper card component
      server-status/        # server status panel
      traffic/              # traffic chart panel
      tickets/              # support tickets panel
public/                     # static assets (favicon, images)
```

## Testing

- Karma + Jasmine. Config is in `angular.json` under `test` builder.
- Tests use `tsconfig.spec.json` (extends root tsconfig, adds jasmine types).
- No e2e setup installed.

## TypeScript

- `strict: true`, `strictTemplates: true`, `noImplicitOverride: true`.
- `moduleResolution: "bundler"`, target `ES2022`.
- `useDefineForClassFields: false` (Angular decorator requirement).
