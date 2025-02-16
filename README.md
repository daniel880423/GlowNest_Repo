# GlowNest
GlowNest 是一個針對美髮美容業（如美睫、美甲）的社群與預約平台。用戶可以創建品牌、招募成員，並展示各自的能力與作品。客戶可以輕鬆地瀏覽品牌、預約服務並進行線上付款。

---

## 目標用戶  
- 美睫、美甲、美髮等美容美業消費族群  
- 品牌管理員：創建品牌並招募成員  
- 成員（技師）：展示個人技能與作品  
- 一般用戶：瀏覽品牌、預約服務  

---

## 系統架構  
- **前端技術：** React
- **後端技術：** Node.js (Express.js)
- **資料庫：** MongoDB
- **部署：** Vercel (前端), Railway / Render (後端)
- **第三方服務：**  
  - 金流：綠界、TapPay 或 Stripe  
  - 即時通知：Firebase Cloud Messaging  
  - 圖片上傳：Cloudinary  

---

## 核心功能模組  
### 1. 用戶系統  
- 用戶註冊、登入（Email 驗證、社群登入）  
- 角色分類：  
  - 一般用戶  
  - 品牌管理員  
  - 成員（技師）  
  - 系統管理員  
- 個人資料管理（包含技能標籤、作品集展示）  

### 2. 品牌與社群管理  
- 創建品牌並設置品牌頁面（品牌介紹、Logo、聯絡方式）  
- 招募與管理成員  
- 品牌內部成員展示（顯示技能與作品）  

### 3. 預約與付款系統  
- 客戶可查看可預約時段，並進行預約  
- 預約確認與通知（Email、推播通知）  
- 支援線上付款及訂單管理  
- 取消與更改預約（符合商業邏輯）  

### 4. 簡易社群功能  
- 用戶可追蹤品牌與技師  
- 作品分享與留言互動  
- 評價系統（針對預約後進行評價）  

---

## 後續規劃與擴充功能  
- **營收模式：**  
  - 提供進階品牌功能收取月費  
  - 預約服務收取平台手續費  
- **行銷推廣：**  
  - SEO 優化、社群分享功能  
- **數據分析：**  
  - 後台儀表板（顯示預約數量、收益、用戶活躍度等）  

---

## 資料結構設計

### 1. 用戶 (Users)

| 欄位         | 類型      | 描述                           |
| ------------ | --------- | ------------------------------ |
| _id          | ObjectId | 用戶唯一識別 ID                 |
| name         | String   | 用戶名稱                        |
| email        | String   | 用戶電子郵件                    |
| password     | String   | 密碼，使用 bcrypt 加密         |
| role         | String   | 用戶角色 (['customer', 'member', 'admin']) |
| phone        | String   | 用戶電話                        |
| avatar       | String   | 用戶頭像 URL                    |
| bio          | String   | 個人簡介                        |
| skills       | Array    | 技能標籤 (ex: ['美睫', '美甲', '造型設計']) |
| portfolio    | Array    | 作品集展示                      |
| socialLinks  | Object   | 社群連結 (Instagram, Facebook, Line) |
| favorites    | Array    | 收藏的品牌或技師               |
| createdAt    | Date     | 創建時間                        |
| updatedAt    | Date     | 更新時間                        |


### 2. 品牌 (Brands)

| 欄位         | 類型      | 描述                           |
| ------------ | --------- | ------------------------------ |
| _id          | ObjectId | 品牌唯一識別 ID                 |
| name         | String   | 品牌名稱                        |
| description  | String   | 品牌描述                        |
| logo         | String   | 品牌 Logo 圖片 URL             |
| owner        | ObjectId | 品牌擁有者 (關聯到 Users)      |
| members      | Array    | 成員列表 (包含用戶 ID 和角色)  |
| services     | Array    | 提供的服務 (每個服務包含標題、描述、價格等) |
| socialLinks  | Object   | 品牌社群連結 (Instagram, Facebook, Line) |
| followers    | Array    | 追蹤此品牌的用戶               |
| reviews      | Array    | 品牌的評價                      |
| createdAt    | Date     | 創建時間                        |
| updatedAt    | Date     | 更新時間                        |

### 3. 預約 (Bookings)

| 欄位         | 類型      | 描述                           |
| ------------ | --------- | ------------------------------ |
| _id          | ObjectId | 預約唯一識別 ID                 |
| customer     | ObjectId | 客戶 (關聯到 Users)            |
| brand        | ObjectId | 品牌 (關聯到 Brands)           |
| service      | Object   | 服務資訊 (標題、價格、時長等)  |
| stylist      | ObjectId | 技師 (關聯到 Users)            |
| date         | Date     | 預約時間                        |
| status       | String   | 預約狀態 (['pending', 'confirmed', 'completed', 'canceled']) |
| payment      | Object   | 付款資訊 (方法、狀態、金額等)  |
| notes        | String   | 客戶備註                        |
| createdAt    | Date     | 創建時間                        |
| updatedAt    | Date     | 更新時間                        |


### 4. 社群互動 (Interactions)

| 欄位         | 類型      | 描述                           |
| ------------ | --------- | ------------------------------ |
| _id          | ObjectId | 互動唯一識別 ID                 |
| type         | String   | 互動類型 (['follow', 'like', 'comment', 'review']) |
| userId       | ObjectId | 用戶 ID (操作人，關聯到 Users) |
| targetId     | ObjectId | 目標 ID (關聯到 Users, Brands, Portfolio) |
| targetType   | String   | 目標類型 (['user', 'brand', 'portfolio']) |
| content      | String   | 留言內容或評價                  |
| rating       | Number   | 評價分數 (僅適用於 review)      |
| createdAt    | Date     | 創建時間                        |


### 5. 系統管理 (Admin)

| 欄位         | 類型      | 描述                           |
| ------------ | --------- | ------------------------------ |
| _id          | ObjectId | 管理操作唯一識別 ID             |
| action       | String   | 操作類型 (ex: 'create', 'update', 'delete', 'login') |
| userId       | ObjectId | 操作人 (關聯到 Users)          |
| target       | String   | 操作目標 (ex: 'User', 'Brand', 'Booking') |
| targetId     | ObjectId | 操作目標 ID                    |
| details      | String   | 操作詳情                        |
| createdAt    | Date     | 創建時間                        |

---

## API 設計

### 用戶 (Users)

| 方法    | 路由                       | 描述                 |
| ------- | -------------------------- | -------------------- |
| POST    | `/api/users/register`      | 用戶註冊             |
| POST    | `/api/users/login`         | 用戶登入             |
| GET     | `/api/users/:id`           | 獲取用戶資料         |
| PUT     | `/api/users/:id`           | 更新用戶資料         |

### 品牌 (Brands)

| 方法    | 路由                       | 描述                 |
| ------- | -------------------------- | -------------------- |
| POST    | `/api/brands/create`       | 創建品牌             |
| GET     | `/api/brands/:id`          | 獲取品牌資料         |
| GET     | `/api/brands`              | 獲取所有品牌         |
| PUT     | `/api/brands/:id`          | 更新品牌資料         |
| DELETE  | `/api/brands/:id`          | 刪除品牌             |

### 預約 (Bookings)

| 方法    | 路由                       | 描述                 |
| ------- | -------------------------- | -------------------- |
| POST    | `/api/bookings/create`     | 創建預約             |
| GET     | `/api/bookings/:id`        | 獲取預約資料         |
| PUT     | `/api/bookings/:id`        | 更新預約資料         |
| DELETE  | `/api/bookings/:id`        | 取消預約             |

### 付款 (Payments)

| 方法    | 路由                       | 描述                 |
| ------- | -------------------------- | -------------------- |
| POST    | `/api/payments/stripe`     | 進行 Stripe 付款     |
| POST    | `/api/payments/tappay`     | 進行 TapPay 付款     |

### 社群互動 (Interactions)

| 方法    | 路由                       | 描述                 |
| ------- | -------------------------- | -------------------- |
| POST    | `/api/interactions/follow` | 追蹤品牌或用戶       |
| POST    | `/api/interactions/like`   | 點贊                 |
| POST    | `/api/interactions/comment`| 留言                 |
| POST    | `/api/interactions/review` | 評價                 |

---

## TODO List 與 開發流程

1. **需求確認與原型設計**  
   - 與項目相關人員確認需求，確定功能範圍。
   - 設計網站原型圖，確保每個功能區域清晰且易用。

2. **完成用戶註冊、登入系統**  
   - 實現用戶註冊與登入功能。
   - 設計和實現安全的密碼加密和驗證流程。

3. **後端 API 設計與資料庫建模**  
   - 設計 RESTful API 路由，確定資料庫結構。
   - 使用 MongoDB 建立資料表，處理用戶資料、品牌、服務、預約等。

4. **實現品牌與服務創建功能**  
   - 設計並實現品牌創建、服務管理功能。
   - 讓用戶可以創建品牌、上架服務並設置價格等。

5. **前端頁面設計與互動開發**  
   - 使用 React 設計並開發前端頁面，確保良好的用戶體驗。
   - 整合後端 API 與前端，實現完整的用戶互動流程。

6. **實現預約系統與付款整合**  
   - 設計並實現預約系統，確保用戶能預約服務。
   - 整合金流服務（如 Stripe 或 TapPay）實現線上付款。

7. **實現社群功能（追蹤、留言、點贊）**  
   - 實現用戶之間的互動功能，如追蹤其他用戶、品牌、留言和點贊等。

8. **整合與測試（包含金流、通知系統）**  
   - 完成金流整合，實現付款與通知系統。
   - 測試所有功能確保無誤，包括預約、付款與通知等。

9. **部署至 Vercel（前端）與 Railway（後端）**  
   - 將前端部署至 Vercel，後端部署至 Railway，確保網站正常運行。

10. **上線與維運（考慮使用 Vercel、Netlify、AWS 或 GCP）**  
    - 完成部署，將前端上傳至 Vercel，後端服務部署至 Railway。
    - 監控網站運行，根據需求進行維運和優化。

---
  

## License  
此專案採用 MIT License，詳細內容請參閱 [LICENSE](./LICENSE) 文件。  
