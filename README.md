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
# 1. Clone the repo
git clone

# 2. Navigate to the project folder
cd chessComTest

# 3. Install dependencies
npm install

# 4. Start the development server
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
- Renders 64 `div` elements as chessboard squares in an 8x8 CSS Grid.
- Highlights the clicked square via `highlightedSquare`.
- Formats indices into chess notation using `nameSquare(index)` (e.g., `E4`).

**Emitted Events:**
- `'square-click'` — emitted with the square name when a square is clicked.
- `'square-highlight'` — emitted to trigger highlighting behavior.

**Responsive Feature:**
- Uses pure CSS with `min()` function and media queries for automatic sizing.
- Adapts to viewport width and height constraints to prevent scrolling.
- No JavaScript dimension calculations required.

---

### [`Sidebar.vue`](chessComTest/src/components/Sidebar/Sidebar.vue)
**Props:**
- `squares` — list of clicked square names.
- `position` — defines sidebar alignment (`'right'` or `'bottom'`).

**Features:**
- Automatically scrolls to the bottom when new squares are added.
- Matches ChessBoard height in desktop layout (`position='right'`).
- Adapts to mobile layout with reduced height (`position='bottom'`).

---

## Technologies
- **Vue 3** (Composition API with `<script setup>`)
- **Vite** (for bundling and development)
- **Pure CSS** (responsive layout using Flexbox, Grid, `min()`, and media queries)
- **Cross-browser compatible** (Safari, Chrome, Firefox, Edge)

---

## Implementation Highlights

### Responsiveness
- The main layout (`App.vue`) uses a Flexbox-based structure with centered alignment.
- A breakpoint (`@media (max-width: 56rem)`) stacks the board and sidebar vertically.
- `sidebarPosition` dynamically switches based on `matchMedia('(max-width: 56rem)')`.
- ChessBoard and Sidebar use CSS `min()` to adapt to viewport dimensions automatically.
- Layout prevents vertical scrolling by constraining components to available viewport height.

### Events and State Management
1. **User clicks a square**  
   → `ChessBoard.vue` emits `'square-click'`.  
2. **`App.vue` listens to the event**  
   → Updates `selectedSquares` via `addSquare()`.  
3. **Highlight management**  
   → Controlled internally in `ChessBoard.vue` via `highlightedSquare`.


---

## Recent Improvements

### CSS-Only Responsive Refactor
- **Removed JavaScript sizing logic**: Eliminated ResizeObserver and viewport event listeners from ChessBoard.
- **Pure CSS implementation**: All responsive behavior now handled via CSS `min()` and media queries.
- **Safari compatibility**: Removed browser-specific CSS variables (`--safe-area-inset-*`).
- **Viewport-aware sizing**: Components automatically adjust to available screen space without scrolling.
- **Synchronized heights**: Sidebar matches ChessBoard height in desktop layout.
- **Updated breakpoint**: Changed from 52rem to 56rem (896px) for better tablet support.

### Benefits
- ✅ Simpler, more maintainable code
- ✅ Better performance (less JavaScript execution)
- ✅ Consistent cross-browser behavior
- ✅ Automatic viewport adaptation
- ✅ No vertical scrolling required
