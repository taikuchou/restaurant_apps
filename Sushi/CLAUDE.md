# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Sushi Restaurant Service Management System** designed for kitchen staff to track sushi roll preparation, daily tasks, and ingredient notes. The application is built as a mobile-first Progressive Web App (PWA) with support for Traditional Chinese (zh-TW), Simplified Chinese (zh-CN), and English (en) localization.

## Architecture

### Two Implementations

The codebase contains **two parallel implementations** of the same functionality:

1. **SPA Version** (`spa/` directory)
   - Single-page application with client-side navigation
   - All features in one HTML file (`sushi_assistant.html`)
   - Hash-based routing (`#roll1`, `#roll2`, `#notes`, `#checklist`)
   - Comprehensive i18n system with live language switching
   - Main files: `sushi_assistant.html`, `sushi_assistant.js`, `i18n.js`, `styles.css`

2. **Multi-Page Version** (`pages/` directory)
   - Separate HTML pages for each feature
   - Traditional multi-page navigation
   - Files: `roll1.html/js`, `roll2.html/js`, `sushinotes.html/js`, `r2_checklist.html/js`, `styles.css`
   - Uses traditional Chinese only (no i18n system)

### Core Features

1. **Roll 1 & Roll 2 Service Tracking**
   - Track daily sushi roll production targets and remaining portions
   - Decrement counts as items are served
   - Click-to-expand ingredient details for each roll type
   - Visual indicators for low stock (≤2 portions) and completed items
   - Reset functionality to restore daily targets
   - Responsive mobile card view + desktop table view

2. **Notes (筆記)**
   - Reference library of sushi roll ingredients and preparation instructions
   - Dropdown selector to view specific roll recipes
   - Details include roll type, toppings, fillings, and special notes

3. **Checklist (流程)**
   - Daily kitchen workflow checklist with three phases:
     - Prep (準備前): Pre-preparation tasks
     - During (準備中): Active preparation tasks
     - Post (準備後): Post-preparation and cleanup tasks
   - Progress tracking with completion rates
   - 2-hour bento timer for food safety compliance
   - Section-level select all/deselect all controls
   - Daily automatic reset

### Data Persistence

- **localStorage** used for all state management
- Storage keys:
  - `roll1-orders-spa` / `juan3-orders-with-ingredients-v3-mobile`
  - `roll2-orders-spa`
  - `checklist-spa-v2.0`
  - `sushi-assistant-lang` (language preference)
- Data includes date stamps for automatic daily reset
- State persists across browser sessions

### Internationalization (SPA Only)

- **i18n Manager Class** (`spa/i18n.js`)
- Translation keys organized by feature domain:
  - `nav.*`: Navigation labels
  - `roll1.*`, `roll2.*`: Roll tracking UI
  - `notes.*`: Notes feature
  - `checklist.*`: Checklist feature
  - `product.*`: Product names
  - `ing.*`: Ingredient components
  - `note.*`: Recipe notes
  - `instruction.*`: Preparation instructions
- Language detection from browser settings
- Live language switching without page reload
- Custom event system (`languageChanged`) triggers re-rendering

## Development Workflow

### File Structure
```
Sushi/
├── spa/                    # Modern SPA implementation (RECOMMENDED)
│   ├── sushi_assistant.html
│   ├── sushi_assistant.js
│   ├── i18n.js            # Translation system
│   └── styles.css
└── pages/                  # Legacy multi-page implementation
    ├── roll1.html/js
    ├── roll2.html/js
    ├── sushinotes.html/js
    ├── r2_checklist.html/js
    └── styles.css
```

### Testing

**No build system or test suite exists.** Testing is manual:

1. Open HTML files directly in browser (file:// protocol supported)
2. Test on mobile viewport (iOS Safari and Chrome are primary targets)
3. Verify localStorage persistence across page reloads
4. Test language switching (SPA only)
5. Verify timer functionality (checklist feature)

### Mobile-First Design

- Application optimized for **mobile devices** (restaurant kitchen tablets/phones)
- Responsive breakpoints in CSS (`@media (max-width: 768px)`)
- Touch-friendly tap targets
- iOS PWA meta tags for home screen installation
- Viewport configuration prevents zoom and uses safe area insets

## Key Patterns and Conventions

### State Management Pattern
```javascript
// 1. Define initial data structure
const initialItems = [...];

// 2. Load from localStorage with fallback
function loadState() {
  const raw = localStorage.getItem(STORAGE_KEY);
  if (!raw) return initialItems;
  // Merge saved state with initial structure
  return initialItems.map(it => {
    const saved = parsed.find(p => p.id === it.id);
    return saved ? { ...it, remaining: saved.remaining } : it;
  });
}

// 3. Save after each state change
function saveState(state) {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
}
```

### Dual Rendering Pattern (Mobile + Desktop)
```javascript
function render() {
  // Clear both containers
  tbody.innerHTML = "";        // Desktop table
  mobileItems.innerHTML = "";  // Mobile cards

  // Render both views simultaneously
  for (const item of state) {
    // Create desktop table row
    const tr = document.createElement("tr");
    tbody.appendChild(tr);

    // Create mobile card
    const card = renderMobileCard(item);
    mobileItems.appendChild(card);

    // Sync state between both views
    // (button clicks update both DOM elements)
  }
}
```

### Translation Key Pattern (SPA)
```javascript
// Product names use prefixed keys
i18n.t('product.california')        // "加州卷" / "加州卷" / "California Roll"
i18n.t('ing.whiteSesame')           // "白芝麻" / "白芝麻" / "White Sesame"
i18n.t('checklist.task.prep1')      // Task-specific translations

// Compound ingredients with quantities
const parts = "ing.crabmeat 60g".split(' ');
// Result: i18n.t('ing.crabmeat') + ' 60g'
```

### Dynamic Content Updates
```javascript
// SPA uses custom events to trigger re-renders
window.addEventListener('languageChanged', (e) => {
  // Re-render all dynamic content
  if (window.roll1Render) window.roll1Render();
  if (window.roll2Render) window.roll2Render();
  // ...
});

// Expose render functions globally for i18n updates
window.roll1Render = function() { /* rebuild and render */ };
```

## Common Tasks

### Adding a New Sushi Roll Product

**In SPA version:**
1. Add translation keys to all three languages in `spa/i18n.js`:
   ```javascript
   "product.newRoll": "新卷名稱",
   "ing.newIngredient": "新成分",
   ```

2. Add ingredient data to `INGREDIENTS_DATA` in `spa/sushi_assistant.js`:
   ```javascript
   "newRoll": {
     type: "ing.type2Inside",
     toppings: ["ing.whiteSesame"],
     fillings: ["ing.cucumber 20g", "ing.avocado 30g"]
   }
   ```

3. Add to `initialItemsData` array:
   ```javascript
   { nameKey: "newRoll", target: 5, note: "", ingKey: "newRoll" }
   ```

**In pages version:**
1. Update `INGREDIENTS` object in `pages/roll1.js` or `roll2.js`
2. Add to `initialItems` array with Chinese name directly

### Modifying Daily Targets

Edit the `target` value in `initialItemsData` (SPA) or `initialItems` (pages):
```javascript
{ nameKey: "california", target: 15, ... }  // Change from 10 to 15
```

Users can reset to these targets via the "重設" button.

### Adding Checklist Tasks

**In SPA version:**
1. Add translation keys for all languages in `spa/i18n.js`:
   ```javascript
   "checklist.task.prep5": "新的準備任務",
   ```

2. Update `buildData()` function in checklist section:
   ```javascript
   prep: [
     i18n.t('checklist.task.prep1'),
     i18n.t('checklist.task.prep2'),
     i18n.t('checklist.task.prep5'),  // Add new task
   ]
   ```

**In pages version:**
Modify the task arrays directly in `pages/r2_checklist.js`.

### Debugging localStorage Issues

Clear specific storage keys in browser console:
```javascript
localStorage.removeItem('roll1-orders-spa');
localStorage.removeItem('roll2-orders-spa');
localStorage.removeItem('checklist-spa-v2.0');
localStorage.clear();  // Clear all
```

### Styling Changes

Both implementations share similar CSS structure:
- Global styles: card layouts, buttons, navigation
- Responsive utilities: `.mobile-items`, `.table-wrap`
- State classes: `.zero`, `.low`, `.complete`, `.checked`
- CSS custom properties for theming (`:root` variables)

Modify `spa/styles.css` or `pages/styles.css` depending on which version.

## Important Notes

- **No framework dependencies**: Pure vanilla JavaScript, no build step required
- **No version control apparent**: Repository initialized with `/init` but no git history visible
- **Production environment**: This appears to be actively used in a restaurant setting
- **Data safety**: localStorage is device-specific; no cloud backup or sync
- **Browser compatibility**: Primarily tested on iOS Safari and modern Chrome
- **Chinese-first**: Original implementation is Traditional Chinese; English added later
- **Date-based reset**: Checklist and potentially roll tracking resets daily at midnight (browser time)

## Preferred Approach for New Features

When extending this system:
1. **Use the SPA version** (`spa/`) as the base—it's more maintainable
2. Add translations to all three languages immediately
3. Follow the existing state management patterns (load → modify → save)
4. Test on mobile viewport first (320px - 768px widths)
5. Ensure both mobile card view and desktop table view work
6. Consider daily reset behavior for time-sensitive features
