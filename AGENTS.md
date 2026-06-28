# AGENTS.md — cmp-deep-dive

## Quick start

```bash
npm install          # install dependencies
npm start            # ng serve on http://localhost:4200/
npm test             # ng test via Karma + Jasmine (no spec files exist)
npm run build        # production build → dist/cmp-deep-dive
npm run watch        # ng build --watch --configuration development
```

## Architecture

- **Angular 18 standalone app** — no `NgModule`s. `src/main.ts` bootstraps `AppComponent` directly.
- **No router** — single-page dashboard. No routing setup.
- **No linter/formatter config** — no ESLint, Prettier, or stylelint.
- **`.editorconfig`** enforces 2-space indent, single quotes for TS, final newline.

## Project structure

```
src/
  main.ts
  index.html                              # <app-root> in body
  styles.css                              # global styles (plain CSS, no SCSS)
  app/
    app.component.ts/html                 # root, imports all dashboard components
    header/                               # selector: app-header
    dashboard/
      dashboard-item/                     # selector: app-dashboard-item (wrapper card)
      server-status/                      # selector: app-server-status
      traffic/                            # selector: app-traffic (hardcoded data)
      tickets/
        tickets.component.ts/html         # selector: app-tickets (stateful list)
        ticket.model.ts                   # Ticket interface
        ticket/                           # selector: app-ticket (single ticket)
        new-ticket/                       # selector: app-new-ticket (form)
    shared/
      button/                             # selector: button[appButton], a[appButton]
      control/                            # selector: app-control (label wrapper)
```

## Component conventions

- Each component in its own folder: `*.component.ts`, `*.component.html`, `*.component.css`.
- All **standalone** with `templateUrl` (not inline). No `styleUrls` on `AppComponent`.
- Selectors use `app-` prefix.
- `ButtonComponent` uses an **attribute selector** (`button[appButton], a[appButton]`) and multi-slot content projection with `ngProjectAs="icon"`.

## Key code patterns

- **Signal-based inputs/outputs** — `input.required()`, `output()`. Older `@ViewChild` decorator still used in `NewTicketComponent`.
- **Angular 17+ control flow** — `@if`, `@for`, `@empty` throughout (not `*ngIf`/`*ngFor`).
- **All state is local** — no services, no HTTP, no external data sources.
- **`FormsModule`** used only in `NewTicketComponent` for `ngModel` two-way binding.

## TypeScript

- `strict: true`, `strictTemplates: true`, `noImplicitOverride: true`, `noPropertyAccessFromIndexSignature: true`.
- `moduleResolution: "bundler"`, target `ES2022`.
- `useDefineForClassFields: false` (Angular decorator requirement).

## Testing

- Karma + Jasmine configured in `angular.json`. Chrome launcher only.
- **No `.spec.ts` files exist** — `npm test` returns zero specs. New components should include tests.
