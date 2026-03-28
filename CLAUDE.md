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

### Component structure

| File | Responsibility |
|------|---------------|
| `src/App.jsx` | Root component. Owns `transactions` state. Passes data and callbacks to children. |
| `src/Summary.jsx` | Receives `transactions`, computes and displays total income, expenses, and balance. |
| `src/TransactionForm.jsx` | Owns its own form state (`description`, `amount`, `type`, `category`). Calls `onAdd(transaction)` prop on submit. |
| `src/TransactionList.jsx` | Receives `transactions`, owns filter state (`filterType`, `filterCategory`), renders filtered table. |

### State shape

```js
transactions: [{
  id: number,
  description: string,
  amount: number,
  type: "income" | "expense",
  category: string,    // food | housing | utilities | transport | entertainment | salary | other
  date: string         // ISO format
}]
```

### Data flow

- `App` holds the `transactions` array and passes it to all three child components.
- `TransactionForm` manages its own controlled inputs; on submit it calls `onAdd` which appends to `transactions` in `App`.
- `TransactionList` filters the received `transactions` locally using its own filter state.
- `Summary` derives totals directly from the `transactions` prop on each render.

### Known limitations

- No data persistence (resets on page refresh)
- Delete button CSS exists (`.delete-btn`) but the feature is not implemented
- 8 hardcoded sample transactions are seeded in initial state
