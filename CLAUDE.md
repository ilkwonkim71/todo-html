# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # Start dev server at http://localhost:5173 (Vite watch mode)
npm run build    # Type-check (tsc) then production build
npm run preview  # Serve production build locally
npx tsc --noEmit # Type-check only, no output
```

## Architecture

React 18 + TypeScript SPA built with Vite. No routing, no UI library — all styles are custom CSS in `src/index.css`.

**State:** All application state lives in a single custom hook `src/hooks/useTodos.ts`. It owns the `todos[]` array, the active `filter`, and derived counts. Components receive data and callback props; they do not manage state themselves.

**Data flow:**
```
App (useTodos hook)
├── TodoInput   → onAdd(text, priority)
├── FilterBar   → onFilterChange(filter)
└── TodoList
    └── TodoItem → onToggle / onDelete / onEdit
```

**Persistence:** `localStorage` key `'claude-todos'` — the entire todos array is written on every change via `useEffect`.

**Types** (`src/types.ts`):
- `Priority`: `'low' | 'medium' | 'high'`
- `Filter`: `'all' | 'active' | 'completed'`
- `Todo`: `{ id, text, completed, createdAt, priority }`

## Key behaviors

- Todo IDs use `crypto.randomUUID()`
- New todos are prepended (front of list)
- Double-clicking a todo text enters inline edit mode; saving an empty edit deletes the todo
- Completed todos cannot be edited
- `VITE_BASE_PATH` env var controls the Vite base path (defaults to `/`)

## Language

UI strings are hardcoded in Korean. There is no i18n library.
