# CmpDeepDive

An **Angular 18** standalone components demo — a single-page operational dashboard that showcases modern Angular APIs including **signals**, **new control flow syntax**, **content projection**, and **standalone architecture** without a single `NgModule`.

Built as a hands-on exploration of Angular's evolving component model, this project replaces traditional decorator-heavy patterns with signal-based reactivity and the `@if`/`@for` template syntax.

---

## Features

- **Server Status Panel** — Simulates real-time server health polling (online / offline / unknown) using `signal()` and `setInterval`, with automatic cleanup via `DestroyRef`.
- **Traffic Chart** — Renders a bar chart for the last 7 days from hardcoded data using dynamic `[style.height]` bindings.
- **Support Tickets** — Add new tickets via a form, toggle detail visibility, and mark tickets as completed. All state is local to the component.
- **Reusable Dashboard Card** — `app-dashboard-item` wraps child content with an image header via `<ng-content>` content projection.
- **Styled Button Directive** — `button[appButton]` attribute selector applies custom styling to native buttons and anchor elements with multi-slot icon projection.
- **Form Control Wrapper** — `app-control` wraps form inputs with a label, using `contentChild()` to access projected elements.

---

## Tech Stack

| Category       | Technology                                                  |
|----------------|-------------------------------------------------------------|
| Framework      | Angular 18.0.0                                              |
| Language       | TypeScript 5.4 (strict mode)                                |
| Styling        | Plain CSS (no SCSS, no preprocessor)                        |
| Build Tool     | Angular CLI / `@angular-devkit/build-angular`               |
| Test Runner    | Karma 6.4 + Jasmine 5.1 (Chrome launcher)                   |
| Bundling       | ES2022 modules, `moduleResolution: "bundler"`               |

**Runtime dependencies:** `@angular/forms` (for `ngModel`), `rxjs` ~7.8, `zone.js` ~0.14.

---

## Project Structure

```
src/
  index.html                   Root HTML shell with <app-root>
  main.ts                      Application entry — bootstrapApplication(AppComponent)
  styles.css                   Global styles (Google Fonts import, layout reset)

  app/
    app.component.ts           Root component — imports all dashboard widgets
    app.component.html         Template layout — header + three dashboard-item cards

    header/
      header.component.*       App header with logo, nav links, and styled logout button

    dashboard/
      dashboard-item/
        dashboard-item.*       Generic card wrapper — receives image/title inputs,
                               projects child content via <ng-content>

      server-status/
        server-status.*        Simulated server health — signal<string> updated every 5s
                               Uses DestroyRef for interval cleanup

      traffic/
        traffic.*              Bar chart visualization — hardcoded 7-day data, computed
                               maxTraffic for proportional bar heights

      tickets/
        ticket.model.ts        Ticket interface — { id, title, request, status }
        tickets.component.*    Ticket list manager — stateful array, add/close handlers
        ticket/
          ticket.*             Single ticket card — toggle details, mark as completed
        new-ticket/
          new-ticket.*         Form component — ngModel binding, emits add output event

    shared/
      button/
        button.*               Attribute-selector component — button[appButton], a[appButton]
                               Multi-slot content projection with <span ngProjectAs="icon">
      control/
        control.*              Label + input wrapper — contentChild query, ViewEncapsulation.None

public/                        Static assets (favicon, status.png, globe.png, etc.)
```

---

## Installation & Setup

### Prerequisites

- [Node.js](https://nodejs.org/) 18.19+ or 20.11+
- npm 10+ (ships with Node)
- Angular CLI 18 (`npm install -g @angular/cli` optional — the project uses the local copy)

### Steps

```bash
# 1. Clone the repository
git clone <repo-url>
cd cmp-deep-dive

# 2. Install dependencies
npm install

# 3. Start the development server
npm start
# → Navigate to http://localhost:4200/
```

---

## Available Scripts

| Command           | Description                                        |
|-------------------|----------------------------------------------------|
| `npm start`       | `ng serve` — development server with hot reload    |
| `npm run build`   | `ng build` — production build to `dist/cmp-deep-dive/` |
| `npm run watch`   | `ng build --watch --configuration development`     |
| `npm test`        | `ng test` — launches Karma + Jasmine (no specs yet) |
| `npm run ng`      | Direct access to the local Angular CLI binary      |

> **Note:** No test files (`.spec.ts`) currently exist. The Karma + Jasmine infrastructure is configured and ready — new components should include corresponding tests.

---

## Learning Objectives

This project was created as a deep dive into Angular 18's component model. It demonstrates:

| Concept                      | Implementation                                                      |
|------------------------------|---------------------------------------------------------------------|
| **Standalone components**    | Every component has `standalone: true`; no `NgModule` in the app    |
| **Signal-based inputs**      | `input.required<T>()` replaces `@Input({ required: true })`         |
| **Signal-based outputs**     | `output<T>()` replaces `@Output() EventEmitter`                     |
| **Angular 17+ control flow** | `@if` / `@else if` / `@else` / `@for` / `@empty` instead of structural directives |
| **Content projection**       | Single-slot (`<ng-content>`) and multi-slot (`ngProjectAs="icon"`)  |
| **Attribute selectors**      | `button[appButton]` — styling native elements without extra DOM     |
| **Lifecycle hooks**          | `onInit`, `afterViewInit`, `afterRender`, `afterNextRender`, `DestroyRef` |
| **Template-driven forms**    | `FormsModule` + `ngModel` in the new-ticket component               |

### Key Architectural Decisions

- **All state is local** — no services, no HTTP, no external data sources.
- **No router** — single-page layout with static content projection.
- **Strict TypeScript** — `strict: true`, `strictTemplates: true`, `noImplicitOverride: true`, `noPropertyAccessFromIndexSignature: true`.
- **`useDefineForClassFields: false`** — required by Angular decorator semantics.
