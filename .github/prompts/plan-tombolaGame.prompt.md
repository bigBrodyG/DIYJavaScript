# Tombola Game - Interactive Italian Bingo

Build a fully-featured, animated Tombola (Italian Bingo) game with a 90-number board, random extraction system, and engaging visual effects using Tailwind CSS and vanilla JavaScript.

## Core Requirements

### Game Mechanics
- 90-number board (1-90) displayed in a clear grid layout
- Random number extraction without duplicates
- Visual feedback when numbers are extracted
- Complete extraction history tracking
- Game state management (start, pause, reset)
- Prevention of cheating/duplicate extractions

### User Interface
- **Main Display Area**: Large, prominent display for the currently extracted number with eye-catching animations
- **Game Board**: 90-number grid (suggest 9 rows × 10 columns) with visual states (unextracted, extracted, last-extracted)
- **Control Panel**: Start Game, Extract Number, Reset Game buttons with clear visual states
- **History Panel**: Scrollable list of extracted numbers in chronological order (most recent first)
- **Statistics**: Total numbers extracted, remaining numbers, progress indicator

### Visual Design (Tailwind CSS)
- Vibrant gradient backgrounds (`bg-gradient-to-br from-purple-600 via-pink-500 to-orange-400`)
- Italian carnival theme: warm colors (red #DC2626, gold #FBBF24, green #059669)
- Card-based layout with shadows (`shadow-2xl`) and rounded corners (`rounded-3xl`)
- Responsive design: mobile-first, adapt to tablet/desktop
- Dark mode support (optional but recommended)

## Implementation Steps

### 1. HTML Structure & Setup
Create semantic HTML5 structure in `/home/giordi/Repos/DIYJavaScript/Tombola/src/index.html`:
- `<!DOCTYPE html>` with UTF-8 charset and viewport meta
- Include Tailwind CSS via CDN: `<script src="https://cdn.tailwindcss.com"></script>`
- Custom Tailwind config for extended animations and carnival colors
- Semantic sections: `<header>`, `<main>`, `<aside>` (for history)
- Accessibility: ARIA labels, semantic buttons, keyboard navigation support

**Key Elements:**
```html
- <div id="currentNumber"> - Hero display for extracted number
- <div id="gameBoard"> - Grid container for 90 numbers
- <div id="controls"> - Button group
- <div id="history"> - Extraction history list
- <div id="stats"> - Game statistics
```

### 2. CSS Animations & Styling
Custom CSS in `<style>` tag or Tailwind config:

**Animations to implement:**
- `@keyframes popIn` - Scale bounce effect (0 → 1.2 → 1) for extracted numbers
- `@keyframes pulse-glow` - Pulsing shadow/glow effect for current number
- `@keyframes slideInRight` - History item entrance animation
- `@keyframes shake` - Subtle shake on invalid action
- `@keyframes confetti` - Particle effects for game milestones
- `@keyframes flip` - 3D card flip effect for number reveal

**Tailwind Classes to use:**
- Transitions: `transition-all duration-300 ease-out`
- Transforms: `hover:scale-110 active:scale-95`
- Shadows: `shadow-lg hover:shadow-2xl`
- Colors: Dynamic with `bg-gradient-to-*`, `from-*`, `via-*`, `to-*`
- Grid: `grid grid-cols-10 gap-2 md:gap-4`

### 3. JavaScript Game Logic
Core functionality in `<script>` tag or external JS file:

**State Management:**
```javascript
const gameState = {
  numbers: Array.from({length: 90}, (_, i) => i + 1),
  extracted: [],
  remaining: [],
  isPlaying: false,
  currentNumber: null,
  lastExtractedTime: null
};
```

**Key Functions:**
- `initGame()` - Initialize board, reset state, bind event listeners
- `startGame()` - Enable extraction, update UI state
- `extractNumber()` - Random extraction logic with validation
  - Use `Math.floor(Math.random() * remaining.length)`
  - Remove from remaining array
  - Add to extracted array
  - Trigger animations
- `updateBoard()` - Refresh all 90 number cells with current state
- `addToHistory(number)` - Prepend to history list with animation
- `resetGame()` - Clear all data, restore initial state
- `saveGameState()` - localStorage persistence
- `loadGameState()` - Restore from localStorage on page load

**Validation & Edge Cases:**
- Disable "Extract" button when no numbers remain
- Show completion message when all 90 numbers extracted
- Prevent rapid clicking (debounce extraction)
- Handle page refresh without losing state

### 4. Advanced Animations & Effects

**On Number Extraction:**
1. Current number display: Scale up from center with `popIn` animation
2. Board cell: Change color from gray to golden with glow effect
3. Sound effect: Short beep/click (Web Audio API or `<audio>` element)
4. History: Slide in from right with slight fade
5. Confetti burst at milestones (15, 30, 45, 60, 75, 90 numbers)

**Interactive Feedback:**
- Hover effects on all clickable elements
- Active/pressed states on buttons
- Loading spinner during extraction animation
- Progress bar showing completion percentage
- Ripple effect on button clicks

**CSS Implementation:**
```css
@keyframes popIn {
  0% { transform: scale(0); opacity: 0; }
  50% { transform: scale(1.2); }
  100% { transform: scale(1); opacity: 1; }
}

@keyframes pulse-glow {
  0%, 100% { box-shadow: 0 0 20px rgba(251, 191, 36, 0.6); }
  50% { box-shadow: 0 0 40px rgba(251, 191, 36, 1); }
}
```

### 5. Enhanced User Experience

**Keyboard Shortcuts:**
- `Space` or `Enter` - Extract next number
- `R` - Reset game
- `H` - Toggle history panel
- `Esc` - Close modals/overlays

**Responsive Behavior:**
- Mobile (<768px): Stack layout, larger touch targets (min 44×44px)
- Tablet (768-1024px): Side-by-side board and history
- Desktop (>1024px): Full layout with all panels visible
- Use Tailwind breakpoints: `sm:`, `md:`, `lg:`, `xl:`

**Accessibility:**
- Screen reader announcements for extracted numbers
- Focus management for keyboard navigation
- High contrast mode support
- Reduced motion preference detection: `prefers-reduced-motion: reduce`

**Persistence:**
- `localStorage.setItem('tombolaGame', JSON.stringify(gameState))`
- Auto-save on each extraction
- Prompt user to continue or start fresh on page load

### 6. Polish & Extras

**Visual Enhancements:**
- Particle effects library for confetti (or custom canvas implementation)
- Gradient animation on main background
- Number cells with 3D depth effect using shadows
- Smooth color transitions between states
- Loading skeleton screens

**Audio (Optional):**
- Number extraction sound (short, pleasant)
- Milestone celebration sounds
- Background music toggle
- Volume control
- Mute option with localStorage preference

**Game Features:**
- Speed modes: Slow (2s delay), Normal (1s), Fast (0.5s), Turbo (instant)
- Auto-play mode with adjustable interval
- Export extraction history as CSV/JSON
- Share results (clipboard or social)
- Multiple game modes (traditional, speed tombola)

## Technical Specifications

### Browser Compatibility
- Modern browsers (Chrome 90+, Firefox 88+, Safari 14+, Edge 90+)
- CSS Grid and Flexbox support required
- ES6+ JavaScript (const, let, arrow functions, template literals)
- LocalStorage API
- Optional: Web Audio API for sounds

### Performance Targets
- Initial load: <2s
- Extraction animation: 60fps (16.67ms per frame)
- Board update: <100ms for 90 elements
- Memory usage: <50MB
- No layout thrashing or reflows during animations

### File Structure
```
Tombola/src/
├── index.html       (All-in-one: HTML + CSS + JS)
└── assets/          (Optional: for sounds/images)
    ├── sounds/
    └── images/
```

## Design Decisions

### Theme: Italian Carnival
- **Primary Colors**: 
  - Gold: `#FBBF24` (extracted numbers)
  - Red: `#DC2626` (current number)
  - Green: `#059669` (buttons, accents)
  - Purple: `#7C3AED` (gradients)
- **Typography**: Bold, playful fonts (Tailwind default or custom Google Font)
- **Patterns**: Subtle diagonal stripes or dots in background

### Animation Philosophy
- **Purposeful**: Every animation serves a functional purpose
- **Performant**: CSS transforms and opacity only (GPU-accelerated)
- **Respectful**: Reduced motion for accessibility
- **Delightful**: Celebrate user actions with micro-interactions

### User Flow
1. **Landing**: Welcoming screen with "Start Game" button
2. **Playing**: Main board visible, extract button prominent
3. **Extracting**: Brief animation (0.5-1s), number revealed
4. **Completed**: Celebration screen with game summary and reset option

## Testing Checklist

- [ ] All 90 numbers can be extracted without errors
- [ ] No duplicate numbers appear
- [ ] Reset function clears all state correctly
- [ ] localStorage persists and restores game state
- [ ] Responsive on mobile, tablet, desktop
- [ ] Keyboard shortcuts work correctly
- [ ] Animations perform at 60fps
- [ ] Accessibility: screen reader compatible, keyboard navigable
- [ ] No console errors or warnings
- [ ] Works in multiple browsers
- [ ] Handles edge cases (rapid clicking, page refresh, full extraction)

## Future Enhancements

1. **Multiplayer**: Socket.io for synchronized games across devices
2. **Printable Cards**: Generate and print bingo cards for players
3. **Caller Voice**: Text-to-speech for number announcements
4. **Statistics**: Track historical games, most frequent numbers
5. **Themes**: Customizable color schemes and visual styles
6. **Leaderboard**: Time to complete, fewest extractions for patterns
7. **PWA**: Install as standalone app with offline support
