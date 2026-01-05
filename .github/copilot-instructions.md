# Soc Ops — AI Agent Instructions

Social Bingo game for in-person mixers. React + TypeScript + Tailwind v4 + Vite.

## Development Checklist

Before committing changes, always verify:
- [ ] `npm run lint` — No ESLint errors
- [ ] `npm run test` — All 21 Vitest tests pass
- [ ] `npm run build` — TypeScript compiles cleanly
- [ ] `npm run dev` — Local preview works as expected

## Architecture

**State Pattern:** `useBingoGame` hook manages state with localStorage persistence. Pure game logic in [bingoLogic.ts](../src/utils/bingoLogic.ts) (Fisher-Yates shuffle, win detection). Flow: Hook → Components → User actions → Hook updates.

**Data Model:** 25 squares (24 questions + center free space). Questions from [questions.ts](../src/data/questions.ts) shuffled per game. Win = 5 in a row (12 possible lines: 5 rows, 5 columns, 2 diagonals).

**Components:** App.tsx → StartScreen | GameScreen (BingoBoard → BingoSquare×25, BingoModal)

## Key Files

- [questions.ts](../src/data/questions.ts) — 24 customizable bingo prompts
- [types/index.ts](../src/types/index.ts) — `BingoSquareData`, `BingoLine`, `GameState`
- [index.css](../src/index.css) — Tailwind v4 `@theme` (NOT config file)
- [vite.config.ts](../vite.config.ts) — Auto-detects GitHub Pages base from `VITE_REPO_NAME`

## Conventions

**Styling:**
- Tailwind v4 with `@theme` directive (NOT `tailwind.config.js`)
- Custom colors: `bg-accent`, `bg-marked`, `border-marked-border`, `bg-bingo`
- Follow [frontend-design.instructions.md](instructions/frontend-design.instructions.md) for creative, non-generic aesthetics
- See [tailwind-4.instructions.md](instructions/tailwind-4.instructions.md) for v4-specific patterns

**State Updates:**
- Always return new arrays/objects (immutability)
- Use `queueMicrotask` for cascading state updates (see [useBingoGame.ts#L136](../src/hooks/useBingoGame.ts#L136))
- LocalStorage schema includes version field for future migrations

**Testing:**
- Pure logic functions in `utils/` are fully tested
- Mock-free tests using actual game logic
- Focus on board generation, toggle mechanics, win conditions

## Custom Agents

This project includes specialized agents in `.github/agents/`:
- **quiz-master.agent.md** — Updates questions with creative themes
- **pixel-jam.agent.md** — Design-first UI workflows with Playwright review
- **tdd-*.agent.md** — Test-driven development workflow (red-green-refactor)
- **ui-review.agent.md** — Automated UX/accessibility audits

Use these for domain-specific tasks rather than generic coding prompts.

## Common Tasks

**Add New Questions:** Edit [questions.ts](../src/data/questions.ts) array (keep 24+ items)
**Change Colors:** Update `@theme` block in [index.css](../src/index.css)
**Modify Win Logic:** Adjust `getWinningLines()` in [bingoLogic.ts](../src/utils/bingoLogic.ts)
**Add Game Mode:** Extend `GameState` type + add new screen component + update [App.tsx](../src/App.tsx) routing
