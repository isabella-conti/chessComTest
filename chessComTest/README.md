# Technical Documentation — chessComTest

## Overview
**chessComTest** is a Vue 3 (Vite-based) application that displays a chessboard alongside a sidebar.  
Users can click on board squares to highlight them and record the sequence of their clicks; this list is shown in the sidebar.

---

## Running the Project Locally

### Requirements
- **Node.js** ≥ 20
- **npm**, **yarn**, or **pnpm**

### Setup Steps
```bash
# 1. Navigate to the project folder
cd chessComTest

# 2. Install dependencies
npm install

# 3. Start the development server
npm run dev
```
---

## Structure and Components

### [`App.vue`](chessComTest/src/App.vue)
**Central State:**
- `selectedSquares` — reactive array storing the order of clicked squares.
- `sidebarPosition` — controls the sidebar placement (`'right'` or `'bottom'`).

**Functions:**
- `addSquare` — adds a square name to `selectedSquares`.
- `clearSquares` — clears all selected squares.

**Responsive Logic:**
- Observes viewport size via `matchMedia`.
- Detects iPad-like devices to adjust the sidebar layout dynamically.

---

### [`ChessBoard.vue`](chessComTest/src/components/ChessBoard/ChessBoard.vue)
**Behavior:**
- Renders 64 `div` elements as chessboard squares.
- Highlights the clicked square via `highlightedSquare`.
- Formats indices into chess notation using `nameSquare(index)` (e.g., `E4`).

**Emitted Events:**
- `'square-click'` — emitted with the square name when a square is clicked.
- `'square-highlight'` — emitted to trigger highlighting behavior.

**Responsive Feature:**
- Uses `ResizeObserver` to compute and expose a CSS variable `--board-size-rem` based on board size.

---

### [`Sidebar.vue`](chessComTest/src/components/Sidebar/Sidebar.vue)
**Props:**
- `squares` — list of clicked square names.
- `position` — defines sidebar alignment (`'right'` or `'bottom'`).

**Features:**
- Automatically scrolls to the bottom when new squares are added.
- Uses `--board-size-rem` for responsive layout styling.

---

## Technologies
- **Vue 3** (Composition API with `<script setup>`)
- **Vite** (for bundling and development)
- **Pure CSS** (responsive layout using Flexbox, Grid, `clamp()`, and `aspect-ratio`)

---

## Implementation Highlights

### Responsiveness
- The main layout (`App.vue`) uses a Flexbox-based structure.
- A breakpoint (`@media (max-width: 48rem)`) stacks the board and sidebar vertically.
- `sidebarPosition` dynamically switches based on:
  - `matchMedia('(max-width: 768px)')`
  - `isIPadLike()` heuristic for tablet detection.

### Events and State Management
1. **User clicks a square**  
   → `ChessBoard.vue` emits `'square-click'`.  
2. **`App.vue` listens to the event**  
   → Updates `selectedSquares` via `addSquare()`.  
3. **Highlight management**  
   → Controlled internally in `ChessBoard.vue` via `highlightedSquare`.
