# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # Start dev server at http://localhost:5173
npm run build     # Production build
npm run preview   # Preview production build
npm run lint      # Run ESLint
```

No test runner is configured.

## Architecture

Single-page React app with no external state management, routing, or UI libraries. Vanilla CSS for styling.

**All application logic lives in `src/App.jsx`** — one monolithic component using `useState` hooks. There are no sub-components.

### State shape

```js
transactions: [{
  id: number,
  description: string,
  amount: string,      // stored as string, not number
  type: "income" | "expense",
  category: string,    // food | housing | utilities | transport | entertainment | salary | other
  date: string         // ISO format
}]
```

Form fields (`description`, `amount`, `type`, `category`) and filters (`filterType`, `filterCategory`) are also top-level state.

### Data flow

User input → form state → `handleSubmit` appends to `transactions` array → derived totals computed inline during render → filtered view via `filteredTransactions`.

### Known limitations in the starter code

- Amounts are stored as strings; numeric calculations may behave unexpectedly
- No data persistence (resets on page refresh)
- Delete button CSS exists (`.delete-btn`) but the feature is not implemented
- 8 hardcoded sample transactions are seeded in initial state
