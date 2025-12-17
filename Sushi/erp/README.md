# Product Inventory Management System
# 產品庫存管理系統

A comprehensive inventory management system for tracking product stock levels, reorder schedules, and automated alerts.

## Features

### 📊 Stock Level Monitoring
- **Current Quantity vs Minimum Stock** tracking
- **4-Level Alert System:**
  - 🚨 Out of Stock (quantity = 0)
  - 🚨 Critical (quantity ≤ 50% of minimum)
  - ⚠️ Low Stock (quantity ≤ minimum)
  - ✅ Normal (quantity > minimum)

### 📅 Automatic Reorder Calculations
- Define reorder cycle (in days) for each product
- Automatic calculation of next order date
- Customizable notification days before order date
- Color-coded order status indicators

### 🎨 Visual Dashboard
- Real-time summary cards showing:
  - Critical stock items
  - Low stock items
  - Items needing order
  - Normal stock items
- Color-coded table rows by priority
- Dual status columns (Stock + Order)

### 🔍 Search & Filter
- **Search** by product name or product number
- **Filter by Stock Status:**
  - Critical
  - Low Stock
  - Normal
- **Filter by Order Status:**
  - Needs Order
  - Normal
- Filters work together with search

### ↕️ Column Sorting
- **Click any column header** to sort the table
- **Sort by:**
  - Product Name (alphabetical)
  - Product Number (alphanumeric)
  - Current Quantity (numeric)
  - Minimum Stock (numeric)
  - Unit (alphabetical)
  - Last Import Date (chronological)
  - Reorder Cycle (numeric)
  - Next Order Date (chronological)
  - Notify Days (numeric)
  - Stock Status (priority: critical → low → normal)
  - Order Status (priority: urgent → normal)
- **Toggle direction:** Click same column to switch between ascending ▲ and descending ▼
- **Visual indicators:** Active sort column shows arrow (⇅ ▲ ▼)
- **Works with filters:** Sorting applies to filtered results

### 💾 Data Management
- **LocalStorage Persistence** - All data saved automatically
- **Export to JSON** - Download your inventory data
- **Import from JSON** - Bulk import or merge data
  - Replace existing data
  - Merge (add only new products)
- **Inline Editing** - Click any field to edit directly
- **Modal Form** - Full product add/edit interface

### 📱 Mobile Responsive Design
- **Dual View System:**
  - **Desktop/Tablet (>768px)**: Full-featured table view with sortable columns
  - **Mobile (<768px)**: Card-based layout optimized for small screens
- **Mobile Card Features:**
  - Large, touch-friendly tap targets
  - Prominent current quantity display
  - Color-coded borders (critical, low stock, needs order)
  - Inline editing by tapping any value
  - Quick access to edit/delete buttons
- **Responsive Elements:**
  - Stack summary cards 2x2 on mobile
  - Full-width search and filter controls
  - Optimized font sizes for readability
  - Touch-optimized button sizes
- **PWA-ready** (Progressive Web App)
- **Supports iOS Safari and Android Chrome**

## File Structure

```
erp/
├── product_inventory.html    # Main application file
├── sample_products.json       # Sample data for import
└── README.md                 # This file
```

## Getting Started

### 1. Open the Application
Simply open `product_inventory.html` in a modern web browser. No server or installation required.

### 2. Add Products
Click **"➕ 新增產品 Add Product"** to add a new product manually, or import from JSON.

### 3. Import Sample Data
1. Click **"📂 匯入JSON Import JSON"**
2. Select `sample_products.json`
3. Choose to replace or merge data
4. Review the imported products

## JSON Import Format

```json
[
  {
    "name": "Product Name",
    "productNumber": "PROD-001",
    "currentQuantity": 10,
    "minimumStock": 5,
    "unit": "kg",
    "lastImportDate": "2025-12-01",
    "reorderCycle": 14,
    "notifyDays": 3,
    "notes": "Optional notes"
  }
]
```

### Required Fields
- `name` - Product name
- `productNumber` - Unique product identifier
- `unit` - Unit of measurement (kg, g, 件, 盒, etc.)
- `lastImportDate` - Date of last import (YYYY-MM-DD)
- `reorderCycle` - Days between orders (integer)
- `notifyDays` - Days before order date to notify (integer)

### Optional Fields
- `currentQuantity` - Current stock quantity (default: 0)
- `minimumStock` - Minimum stock level (default: 0)
- `notes` - Additional notes

## Usage Tips

### Mobile vs Desktop Experience

**Mobile View (< 768px):**
- Card-based layout with one product per card
- Tap any value to edit inline
- Swipe to scroll through products
- Large, prominent current quantity display
- Color-coded card borders for quick status identification
- Edit/Delete buttons in card header

**Desktop View (> 768px):**
- Full table view with all columns visible
- Click column headers to sort
- Inline editing by clicking cells
- Hover effects for interactive elements
- More data visible at once

### Setting Up Stock Alerts
1. Set `minimumStock` to your desired safety stock level
2. System will alert when `currentQuantity` ≤ `minimumStock`
3. Critical alert when `currentQuantity` ≤ 50% of `minimumStock`

### Managing Reorder Schedule
1. Set `reorderCycle` based on supplier delivery frequency
2. Set `notifyDays` to give enough time for ordering
3. `nextOrderDate` is calculated automatically
4. Update `lastImportDate` when new stock arrives

### Using Search & Filters
- Search works on both product name and number
- Combine search with filters for precise results
- Click filter buttons to activate/deactivate
- "All" resets the filter category

### Using Column Sorting
- Click any column header to sort by that column
- Click again to reverse the sort direction (ascending ↔ descending)
- Look for the arrow indicator (▲ ▼) to see active sort
- Sorting works together with search and filters
- Useful combinations:
  - Sort by "Stock Status" to see critical items first
  - Sort by "Next Order Date" to prioritize upcoming orders
  - Sort by "Current Quantity" to find low stock items
  - Sort by "Product Name" for alphabetical browsing

### Editing Data
- **Inline Edit**: Click any editable field in the table
- **Modal Edit**: Click ✏️ button for full form
- **Bulk Edit**: Import JSON with updated values

### Exporting Data
1. Click **"💾 匯出資料 Export"**
2. JSON file downloads automatically
3. Use as backup or template
4. Edit in text editor and re-import

## Browser Compatibility

- Chrome/Edge 90+ ✅
- Firefox 88+ ✅
- Safari 14+ ✅
- iOS Safari 14+ ✅
- Android Chrome 90+ ✅

## Testing on Mobile Devices

### iOS (iPhone/iPad)
1. Open in Safari
2. Tap Share button → "Add to Home Screen"
3. Access like a native app from home screen
4. Works offline after first load

### Android
1. Open in Chrome
2. Tap menu (⋮) → "Add to Home Screen"
3. Or use "Install app" prompt if shown
4. Access from app drawer

### Desktop Mobile Simulation
1. Open in Chrome/Edge
2. Press F12 to open DevTools
3. Click device toolbar icon (or Ctrl+Shift+M)
4. Select mobile device (e.g., iPhone 12, Galaxy S21)
5. Test responsive layout and touch interactions

## Data Storage

All data is stored in browser's `localStorage` under the key `product-inventory-data`.

**Important**: Data is device-specific. To transfer data between devices:
1. Export JSON from source device
2. Import JSON on target device

## Technical Details

- **Framework**: Pure Vanilla JavaScript (no dependencies)
- **Storage**: localStorage API
- **Size**: ~75KB (single HTML file)
- **Load Time**: < 100ms on modern devices
- **Offline**: Fully functional offline after first load

## Support

For issues or questions, refer to the CLAUDE.md file in the parent directory.

---

🍣 Built for Sushi Restaurant ERP System
