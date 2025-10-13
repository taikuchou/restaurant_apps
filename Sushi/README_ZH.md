# 壽司餐廳出餐管理系統

專為餐廳廚房環境設計的行動優先漸進式網頁應用程式，用於管理壽司製作、出餐追蹤及每日工作流程。

## 功能特色

### 🍣 壽司卷出餐追蹤

- **卷 1 & 卷 2**：追蹤不同壽司品項的每日生產目標與剩餘份數
- 即時庫存管理，低庫存視覺提示
- 點擊展開成分細節與製作備註
- 自動跨工作階段保存資料

### 📝 食譜筆記

- 完整的成分參考資料庫
- 每款壽司卷的詳細製作說明
- 依產品名稱快速查詢

### ✅ 每日工作流程檢查表

- 三階段任務管理（準備前、準備中、準備後）
- 進度追蹤與完成率顯示
- 便當兩小時回查計時器（食品安全合規）
- 每日午夜自動重設

### 🌍 多語言支援

- 繁體中文
- 简体中文
- English
- 即時語言切換，無需重新載入頁面

## 快速開始

### 本地執行

無需建置流程——直接在網頁瀏覽器中開啟 HTML 檔案：

**現代化 SPA 版本（建議使用）：**

```bash
open spa/sushi_assistant.html
```

**傳統多頁面版本：**

```bash
open pages/roll1.html
open pages/roll2.html
open pages/sushinotes.html
open pages/r2_checklist.html
```

### 安裝為 PWA

在 iOS/Android 裝置上：

1. 使用 Safari/Chrome 開啟應用程式
2. 點選「加入主畫面」
3. 從主畫面啟動以獲得全螢幕體驗

## 專案結構

```
Sushi/
├── spa/                           # 現代化 SPA 實作（建議使用）
│   ├── sushi_assistant.html       # 主應用程式
│   ├── sushi_assistant.js         # 應用程式邏輯與狀態管理
│   ├── i18n.js                    # 國際化系統
│   └── styles.css                 # 響應式樣式
│
└── pages/                         # 傳統多頁面實作
    ├── roll1.html / roll1.js      # 卷1 出餐追蹤
    ├── roll2.html / roll2.js      # 卷2 出餐追蹤
    ├── sushinotes.html / sushinotes.js  # 食譜筆記
    ├── r2_checklist.html / r2_checklist.js  # 每日檢查表
    └── styles.css                 # 頁面樣式
```

## 技術堆疊

- **前端**：原生 JavaScript (ES6+)
- **樣式**：CSS3 搭配自訂屬性
- **儲存**：瀏覽器 localStorage
- **架構**：基於元件的 IIFE 模組
- **零依賴**：無需 npm 套件，不需要建置工具

## 資料持久化

所有應用程式資料使用 localStorage 在瀏覽器本地儲存：

- **壽司卷追蹤狀態**：剩餘份數跨工作階段保存
- **檢查表進度**：每日任務於午夜自動重設
- **語言偏好**：記住所選語言
- **日期版本控制**：自動資料遷移與每日重設

**⚠️ 注意**：資料僅存於裝置本地。清除瀏覽器資料將重設所有追蹤記錄。

## 瀏覽器支援

針對以下環境最佳化：

- iOS Safari 12+
- Chrome/Edge（桌面版與行動版）
- 支援 PWA 的現代行動瀏覽器

## 開發指南

### 新增壽司產品

1. 在 `spa/i18n.js` 中更新翻譯鍵值：

```javascript
"product.newRoll": "新壽司卷",
"ing.newIngredient": "新成分",
```

2. 在 `spa/sushi_assistant.js` 中新增成分資料：

```javascript
const INGREDIENTS_DATA = {
  newRoll: {
    type: "ing.type2Inside",
    toppings: ["ing.whiteSesame"],
    fillings: ["ing.cucumber 20g", "ing.salmon 40g"],
  },
};
```

3. 新增至庫存清單：

```javascript
const initialItemsData = [
  { nameKey: "newRoll", target: 10, note: "", ingKey: "newRoll" },
];
```

### 修改檢查表任務

為新任務新增翻譯鍵值：

```javascript
"checklist.task.prep5": "新準備任務",
```

在 `buildData()` 函式中更新任務陣列：

```javascript
prep: [
  i18n.t("checklist.task.prep1"),
  i18n.t("checklist.task.prep5"), // 新任務
];
```

### 除錯

在瀏覽器控制台清除 localStorage：

```javascript
localStorage.clear();
// 或清除特定鍵值：
localStorage.removeItem("roll1-orders-spa");
localStorage.removeItem("checklist-spa-v2.0");
```

## 設計理念

- **行動優先**：針對廚房平板電腦與智慧型手機最佳化
- **離線可用**：無網路連線也能運作
- **觸控友善**：大型觸控目標與滑動手勢
- **雙語介面**：支援多語言廚房員工
- **即時回饋**：操作後立即視覺更新
- **資料安全**：自動儲存防止資料遺失

## 使用情境

本系統專為以下場景設計：

- 餐廳壽司製作工作站
- 廚房庫存追蹤
- 每日工作流程管理
- 食品安全合規（計時器功能）
- 多語言廚房環境
- 商業廚房中的行動裝置使用

## 授權

專有軟體 - 戴谷州 (2026)

## 貢獻

僅供內部使用。如有功能需求或錯誤回報，請聯繫開發團隊。

---

**最後更新**：2025 年 10 月
**版本**：2.0 (SPA) / 3.0 (多頁面)
