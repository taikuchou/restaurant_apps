# Sushi Restaurant Service Management System

A mobile-first Progressive Web App for managing sushi preparation, service tracking, and daily kitchen workflows in restaurant environments.

## Features

### 🍣 Roll Service Tracking

- **Roll 1 & Roll 2**: Track daily production targets and remaining portions for different sushi varieties
- Real-time inventory management with visual indicators for low stock
- Click-to-expand ingredient details and preparation notes
- Automatic persistence across sessions

### 📝 Recipe Notes

- Comprehensive ingredient reference library
- Detailed preparation instructions for each sushi roll type
- Quick lookup by product name

### ✅ Daily Workflow Checklist

- Three-phase task management (Pre-prep, Active prep, Post-prep)
- Progress tracking with completion rates
- 2-hour bento timer for food safety compliance
- Automatic daily reset at midnight

### 🌍 Multi-Language Support

- Traditional Chinese (繁體中文)
- Simplified Chinese (简体中文)
- English
- Live language switching without page reload

## Quick Start

### Running Locally

No build process required—simply open the HTML files in a web browser:

**Modern SPA Version (Recommended):**

```bash
open spa/sushi_assistant.html
```

**Legacy Multi-Page Version:**

```bash
open pages/roll1.html
open pages/roll2.html
open pages/sushinotes.html
open pages/r2_checklist.html
```

### Installation as PWA

On iOS/Android devices:

1. Open the app in Safari/Chrome
2. Tap "Add to Home Screen"
3. Launch from home screen for full-screen experience

## Project Structure

```
Sushi/
├── spa/                           # Modern SPA implementation (Recommended)
│   ├── sushi_assistant.html       # Main application
│   ├── sushi_assistant.js         # Application logic & state management
│   ├── i18n.js                    # Internationalization system
│   └── styles.css                 # Responsive styles
│
└── pages/                         # Legacy multi-page implementation
    ├── roll1.html / roll1.js      # Roll 1 service tracking
    ├── roll2.html / roll2.js      # Roll 2 service tracking
    ├── sushinotes.html / sushinotes.js  # Recipe notes
    ├── r2_checklist.html / r2_checklist.js  # Daily checklist
    └── styles.css                 # Page styles
```

## Technology Stack

- **Frontend**: Vanilla JavaScript (ES6+)
- **Styling**: CSS3 with custom properties
- **Storage**: Browser localStorage
- **Architecture**: Component-based with IIFE modules
- **No dependencies**: Zero npm packages, no build tools required

## Data Persistence

All application data is stored locally in the browser using localStorage:

- **Roll tracking state**: Remaining portions persist across sessions
- **Checklist progress**: Daily tasks with automatic midnight reset
- **Language preference**: Selected language remembered
- **Date-based versioning**: Automatic data migration and daily resets

**⚠️ Note**: Data is device-specific. Clearing browser data will reset all tracking.

## Browser Support

Optimized for:

- iOS Safari 12+
- Chrome/Edge (desktop & mobile)
- Modern mobile browsers with PWA support

## Development

### Adding New Sushi Products

1. Update translation keys in `spa/i18n.js`:

```javascript
"product.newRoll": "新壽司卷",
"ing.newIngredient": "新成分",
```

2. Add ingredient data in `spa/sushi_assistant.js`:

```javascript
const INGREDIENTS_DATA = {
  newRoll: {
    type: "ing.type2Inside",
    toppings: ["ing.whiteSesame"],
    fillings: ["ing.cucumber 20g", "ing.salmon 40g"],
  },
};
```

3. Add to inventory list:

```javascript
const initialItemsData = [
  { nameKey: "newRoll", target: 10, note: "", ingKey: "newRoll" },
];
```

### Modifying Checklist Tasks

Add translation keys for new tasks:

```javascript
"checklist.task.prep5": "新準備任務",
```

Update task arrays in the `buildData()` function:

```javascript
prep: [
  i18n.t("checklist.task.prep1"),
  i18n.t("checklist.task.prep5"), // New task
];
```

### Debugging

Clear localStorage in browser console:

```javascript
localStorage.clear();
// Or clear specific keys:
localStorage.removeItem("roll1-orders-spa");
localStorage.removeItem("checklist-spa-v2.0");
```

## Design Philosophy

- **Mobile-First**: Optimized for kitchen tablets and smartphones
- **Offline-Capable**: Works without internet connection
- **Touch-Friendly**: Large tap targets and swipe gestures
- **Bilingual UI**: Supports multilingual kitchen staff
- **Real-Time Feedback**: Instant visual updates on actions
- **Data Safety**: Automatic saves prevent data loss

## Use Cases

This system is designed for:

- Restaurant sushi preparation stations
- Kitchen inventory tracking
- Daily workflow management
- Food safety compliance (timer features)
- Multi-language kitchen environments
- Mobile device usage in commercial kitchens

## License

Proprietary - Kuchou Tai (2026)

## Contributing

For internal use only. Contact the development team for feature requests or bug reports.

---

**Last Updated**: October 2025
**Version**: 2.0 (SPA) / 3.0 (Multi-Page)
