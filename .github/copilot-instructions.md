# Bingo Game - AI Agent Instructions

## Development Checklist
- [ ] 
pm run lint - pass linting
- [ ] 
pm run build - verify production build
- [ ] 
pm run test - pass all tests

## Architecture

**State Flow**: useBingoGame (state mgmt)  App (router)  Components (presentational)

**Key Separation**: 
- ingoLogic.ts - pure functions for game mechanics (board generation, win detection)
- useBingoGame.ts - React state + localStorage persistence
- Components - receive props, emit callbacks (no state)
- 	ypes/index.ts - domain types (BingoSquareData, BingoLine, GameState)

## Critical Patterns

**Board**: 5x5 grid (25 squares, index 0-24). Center (index 12) is FREE SPACE (pre-marked, disabled).

**Game States**: 'start'  'playing'  'bingo' (strict transitions)

**Bingo Detection**: checkBingo() checks 12 lines (5 rows + 5 cols + 2 diagonals). Returns BingoLine or null.

**Persistence**: useBingoGame loads/saves game state + winning line via localStorage with alidateStoredData() guard.

**Board Generation**: generateBoard() shuffles 24 questions via Fisher-Yates, places at indices 0-11 + 13-24 (skips center).

## Component Patterns

Props  Data Flow (no prop drilling via context). Callbacks bubble events up.

Example: BingoSquare receives square, isWinning, onClick. Derives CSS classes from booleans:
- Marked: g-marked class
- Winning: g-amber-200 highlight
- Free space: ont-bold, disabled=true

## Adding Features

**New Game Mode**: Add GameState in types  handle in useBingoGame  conditional render in App.

**New Questions**: Update src/data/questions.ts (24 items, 1 FREE_SPACE).

**Styling**: Tailwind v4 utilities only. Custom colors in Tailwind config (e.g., g-marked).

**Tests**: Add *.test.ts files. Pure function tests in ingoLogic.test.ts. Environment is Node (not jsdom).

## Build/Deploy

- **Base Path**: Auto-detected from VITE_REPO_NAME env var (GitHub Actions for Pages)
- **Build**: 
pm run build  TypeScript + Vite to dist/
- **Deploy**: Auto-publish to GitHub Pages on push to main
- **Dev**: 
pm run dev  Vite HMR on port 5173

## Key Constraints

- State immutability: ingoLogic returns new arrays/objects
- No async game logic (synchronous)
- Type safety: Strict TypeScript
- No context providers needed
