# 居研所前端系統（Vue 3）

本專案為資展會全端班結訓專題 —— **租屋媒合平台「居研所」** 的前端部分，使用 Vue 3 + Pinia + Axios + Vue Router 開發，整合後端 API 提供房東與房客操作介面，包含聊天室、刊登房源、快速回應、會員系統等功能。

---

##  專案架構

- `Vue 3 + Composition API`
- `Pinia` 狀態管理
- `Axios` 串接後端 API
- `Vue Router` 前後台路由切換
- `Bootstrap / Tailwind` UI 風格
- `SignalR` 即時通訊（聊天室）

---

##  開發環境建置

```bash
# 安裝依賴
npm install

# 本機開發（預設 port 5173）
npm run dev

# 打包建置
npm run build
```

---

##  目錄結構簡介

```
src/
  assets/         # 靜態資源（圖片、樣式等）
  components/     # 可複用元件（依功能細分）
    ├── common/       # 通用組件（Navbar, Footer, Modal, Loading...）
    ├── cards/        # 卡片元件（房源卡片、通知卡片等）
    ├── chat/         # 聊天室專用元件
    ├── forms/        # 表單元件（搜尋欄、房東表單等）
  views/          # 各頁面元件（對應路由）
  router/         # Vue Router 設定
  stores/         # Pinia 狀態管理
  App.vue         # 入口元件
  main.js         # 專案進入點
```

---

##  主要元件一覽

| 元件名稱 | 說明 |
|----------|------|
| Navbar.vue        | 全站共用導覽列 |
| Footer.vue        | 全站共用頁腳 |
| FullScreenLoading.vue | 全站 loading 動畫 |
| Carousel.vue      | 輪播圖 |
| SearchBar.vue     | 搜尋欄 |
| PropertyCard.vue  | 房源卡片 |
| Badge.vue / BadgeList.vue | 標籤與徽章 |
| StepItem.vue      | 使用步驟單一項 |
| ChatWindow.vue    | 聊天室主對話框 |
| ChatInput.vue     | 訊息輸入框、快速回覆、emoji、圖片上傳 |
| QARobot.vue       | 智能客服與智慧提醒 |
| Pagination.vue    | 分頁器 |
| Alert.vue         | 提醒視窗 |
| Avatar.vue        | 頭像框 |

---

##  主要功能特色

- 房源卡片動畫、標籤、收藏功能
- 多條件搜尋：地區、關鍵字、房型等
- 即時聊天室：訊息、圖片、emoji、快速回覆、檢舉、刪除
- 房東房源管理：刊登、編輯、下架、購買曝光方案（VIP1/2/3）
- 金流串接：綠界 ECPay 信用卡付款流程
- 公告輪播：首頁最新公告、通知徽章
- 智慧提醒與 QARobot 智能客服功能
- 響應式設計：桌機 / 手機支援、骨架 loading 與動畫

---

##  技術棧

**前端框架**
- Vue 3 + Composition API
- Pinia 狀態管理
- Vue Router
- Axios
- Vite 開發環境

**UI / CSS**
- Bootstrap 5 / Tailwind CSS

**圖表與地圖**
- Chart.js（圖表）
- Leaflet（地圖）

**即時功能**
- SignalR（聊天室）

---

##  API 串接說明

本專案前端透過 Axios 串接 ASP.NET Core MVC 後端所提供的 RESTful API，涵蓋以下主要功能模組：

| 功能模組 | 串接說明 |
|----------|----------|
|  身分驗證 | 串接登入、註冊、驗證碼發送、Session 檢查等 API。登入成功後將帳號與角色資訊寫入 Pinia 狀態與 localStorage，同步控制頁面顯示與功能權限。 |
|  房源系統 | 串接房東房源刊登、修改、儲存草稿、下架等 API，前端支援步驟表單、草稿暫存與上傳圖片預覽等互動流程。房客端則串接房源搜尋、收藏與房源詳情 API。 |
|  即時聊天 | 使用 SignalR 與後端建立 WebSocket 連線，實作聊天室訊息即時更新、圖片上傳、聊天室封鎖提示、已讀狀態同步等功能。 |
|  金流流程 | 串接綠界金流（ECPay）後端 API，送出訂單建立請求，並處理付款完成後的回調（Callback）與付款狀態查詢，支援立即付款與稍後付款兩種情境。 |
|  公告與提醒 | 串接公告與站內通知 API，支援首頁公告輪播、訊息提醒徽章、快速導頁與讀取紀錄同步等功能。 |
|  檢舉模組 | 串接聊天室訊息檢舉 API，實作檢舉按鈕、分類標籤、自動標記處理狀態等功能。檢舉資料會同步顯示於後台管理系統。 |

---

###  權限與驗證機制

- 登入後使用者資料（如帳號、角色、身分）會寫入 Pinia 狀態與 localStorage，並於刷新後重新載入。
- 所有需要登入才能進入的頁面（例如聊天室、刊登房源）皆在 router.beforeEach 檢查是否具備合法 Session。
- API 攔截器（Axios Interceptor）統一處理登入失效、自動跳轉登入頁、顯示錯誤訊息等狀況。
- 若 API 回傳黑名單或被封鎖訊息，前端會顯示封鎖提示視窗，並導向首頁或客服聯繫頁。

---

###  使用者體驗與錯誤處理

- 所有 API 呼叫皆使用 try/catch 包裹，錯誤統一透過 Alert.vue 彈窗顯示。
- 金流流程包含 loading 動畫與付款中提示，避免使用者重複點擊或誤判交易失敗。
- 聊天圖片訊息支援格式與大小驗證，並加入上傳預覽與上傳失敗備援。
- 若使用者未登入或權限不足時點擊受限功能，會主動顯示登入提示或導向登入頁面。

---

 [前往後端倉庫](https://github.com/Rain-lin-rain/GeeYeangSoreAdmin)

---

## 📎 相關說明文件

- [元件使用說明](src/如何使用Vue元件.md)
- [元件開發規範](src/元件開發規範.md)
- [資料夾結構說明](src/資料夾結構說明.md)

---

