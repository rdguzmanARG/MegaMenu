# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies
yarn install

# Start dev server with hot-reload
yarn dev

# Type-check only
yarn type-check

# Build for production (type-check + bundle)
yarn build

# Preview production build
yarn preview
```

No test suite is configured. The package manager is **yarn** (not npm).

## Architecture

This is a **Vue 3 + Vite + TypeScript** project implementing a responsive mega-menu component intended to be embedded in intranet portals (Tenaris/Ternium brands).

### Component hierarchy

```
App.vue                        ← dev harness, feeds data.json into MegaMenu
└── MegaMenu.vue               ← root component; accepts MenuItem[] via :items prop
    ├── DropdownMenu.vue        ← top-level items that have sub-menus
    └── IconMenu.vue            ← top-level items that are plain links (no children)
```

All components live in `src/vue-components/`. `src/main.ts` mounts `App.vue`.

### Data model

`MenuItem` (exported from `MegaMenu.vue`) supports up to 3 nesting levels:

```ts
interface MenuItem {
  text: string;
  url?: string;          // present on leaf items
  iconClass?: string;    // Font Awesome class (e.g. "fas fa-home")
  iconUrl?: string;      // SVG/image URL used as background-image icon
  items?: MenuItem[];    // presence determines DropdownMenu vs IconMenu
}
```

`src/data.json` is the live sample data passed to the menu in development. Icons can be either a Font Awesome `iconClass` string **or** an `iconUrl` SVG path — never both at once.

### Styling / theming

All styles are in `src/vue-components/mega-menu.scss`, which is imported directly by `MegaMenu.vue`. Theme variables are defined in one of two theme files:

| File | Brand | Primary color |
|---|---|---|
| `tenaris-today.scss` | Tenaris Today (active) | `#189922` (green) |
| `ternium-hoy.scss` | Ternium Hoy | `#ffa626` (orange) |

To switch themes, edit the `@use` line at the top of `mega-menu.scss` (currently `tenaris-today.scss` is active, `ternium-hoy.scss` is commented out). Theme files only expose two variables: `$mm-primary-color` and `$mm-color`.

The breakpoint between desktop and mobile layout is **1024 px**. Below this, the hamburger menu activates and desktop-only elements (`mm-options-desktop`, `mm-options-desktop-n1`) are hidden in favour of their `-mobile` counterparts.

### Desktop vs mobile rendering

`DropdownMenu.vue` renders **two parallel DOM trees** for the same data:

- `.mm-options-desktop` / `.mm-options-desktop-n1` — shown above 1024 px; hover-driven (`@mouseenter`)
- `.mm-options-mobile` / `.mm-options-mobile-n1` — shown below 1024 px; click-driven (`@click`)

CSS toggling (`display: none` / `display: flex`) via media queries controls which tree is visible.

### Open/close state management

`MegaMenu.vue` owns the single `openMenu: string | null` state. It uses two distinct events:

- `@open` — desktop hover: unconditionally opens the hovered menu
- `@toggle` — mobile tap: toggles (open → close → open) the tapped menu

`DropdownMenu` also owns `selectedIndex` for which N1 sub-item is expanded inside an open dropdown.
