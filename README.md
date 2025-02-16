# GlowNest
Designer By MengPoTsen.

## 專案介紹  
** GlowNest ** 是一個針對美髮美容業（如美睫、美甲）的社群與預約平台。用戶可以創建品牌、招募成員，並展示各自的能力與作品。客戶可以輕鬆地瀏覽品牌、預約服務並進行線上付款。  

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

## 開發流程  
1. **需求確認與原型設計**  
2. **後端 API 設計與資料庫建模**  
3. **前端頁面設計與互動開發**  
4. **整合與測試（包含金流、通知系統）**  
5. **上線與維運（考慮使用 Vercel、Netlify、AWS 或 GCP）**  

---

## 貢獻方式  
歡迎對此專案進行貢獻，請遵循以下步驟：  
1. Fork 此專案  
2. 建立新分支 (`git checkout -b feature/新功能`)  
3. 提交修改 (`git commit -m '新增功能描述'`)  
4. Push 到分支 (`git push origin feature/新功能`)  
5. 發送 Pull Request  

---

## 資料結構設計

### 1. 用戶 (Users)
** js ** 
{
  _id: ObjectId,
  name: String,
  email: String,
  password: String,   // 使用 bcrypt 進行加密
  role: String,       // ['customer', 'member', 'admin']
  phone: String,
  avatar: String,     // 頭像 URL
  bio: String,        // 個人簡介
  skills: [String],   // 技能標籤 (ex: ['美睫', '美甲', '造型設計'])
  portfolio: [        // 作品集展示
    {
      title: String,
      image: String,
      description: String,
      tags: [String]
    }
  ],
  socialLinks: {      // 社群連結
    instagram: String,
    facebook: String,
    line: String
  },
  favorites: [ObjectId],   // 收藏的品牌或技師 (關聯到 Brands 或 Users)
  createdAt: Date,
  updatedAt: Date
}

### 2. 品牌 (Brands)
{
  _id: ObjectId,
  name: String,
  description: String,
  logo: String,           // 品牌 Logo 圖片 URL
  owner: ObjectId,        // 關聯到 Users (品牌管理員)
  members: [              // 成員列表
    {
      userId: ObjectId,   // 關聯到 Users
      role: String,       // ['owner', 'stylist', 'admin']
      status: String      // ['pending', 'active', 'inactive']
    }
  ],
  services: [             // 提供的服務
    {
      title: String,
      description: String,
      price: Number,
      duration: Number,   // 預估服務時長 (分鐘)
      tags: [String]
    }
  ],
  socialLinks: {          // 品牌社群連結
    instagram: String,
    facebook: String,
    line: String
  },
  followers: [ObjectId],  // 關聯到 Users (追蹤此品牌的用戶)
  reviews: [              // 評價
    {
      userId: ObjectId,   // 關聯到 Users
      rating: Number,     // 1 ~ 5 星
      comment: String,
      createdAt: Date
    }
  ],
  createdAt: Date,
  updatedAt: Date
}

### 3. 預約 (Bookings)
{
  _id: ObjectId,
  customer: ObjectId,        // 關聯到 Users (客戶)
  brand: ObjectId,           // 關聯到 Brands
  service: {                 // 預約的服務
    serviceId: ObjectId,     // 關聯到 Brands.services
    title: String,
    price: Number,
    duration: Number
  },
  stylist: ObjectId,         // 關聯到 Users (技師)
  date: Date,                // 預約日期與時間
  status: String,            // ['pending', 'confirmed', 'completed', 'canceled']
  payment: {                 // 付款資訊
    method: String,          // ['credit_card', 'line_pay', 'cash']
    status: String,          // ['unpaid', 'paid', 'refunded']
    transactionId: String,   // 金流平台交易編號
    amount: Number
  },
  notes: String,             // 客戶備註
  createdAt: Date,
  updatedAt: Date
}

### 4. 社群互動 (Interactions)
{
  _id: ObjectId,
  type: String,               // ['follow', 'like', 'comment', 'review']
  userId: ObjectId,           // 關聯到 Users (操作人)
  targetId: ObjectId,         // 關聯到目標 (Users, Brands, Portfolio)
  targetType: String,         // ['user', 'brand', 'portfolio']
  content: String,            // 留言內容或評價
  rating: Number,             // 評價分數 (僅適用於 review)
  createdAt: Date
}

### 5. 系統管理 (Admin)
{
  _id: ObjectId,
  action: String,             // 操作類型 (ex: 'create', 'update', 'delete', 'login')
  userId: ObjectId,           // 操作者 (關聯到 Users)
  target: String,             // 操作目標 (ex: 'User', 'Brand', 'Booking')
  targetId: ObjectId,         // 關聯到操作目標的 ID
  details: String,            // 操作詳情
  createdAt: Date
}

---

## API 設計
### 1. 用戶 (Users)
  POST /api/users/register - 用戶註冊
  POST /api/users/login - 用戶登入
  GET /api/users/:id - 獲取用戶資料
  PUT /api/users/:id - 更新用戶資料
### 2. 品牌 (Brands)
  POST /api/brands/create - 創建品牌
  GET /api/brands/:id - 獲取品牌資料
  GET /api/brands - 獲取所有品牌
  PUT /api/brands/:id - 更新品牌資料
  DELETE /api/brands/:id - 刪除品牌
### 3. 預約 (Bookings)
  POST /api/bookings/create - 創建預約
  GET /api/bookings/:id - 獲取預約資料
  PUT /api/bookings/:id - 更新預約資料
  DELETE /api/bookings/:id - 取消預約
### 4. 付款 (Payments)
  POST /api/payments/stripe - 進行 Stripe 付款
  POST /api/payments/tappay - 進行 TapPay 付款
### 5. 社群互動 (Interactions)
  POST /api/interactions/follow - 追蹤品牌或用戶
  POST /api/interactions/like - 點贊
  POST /api/interactions/comment - 留言
  POST /api/interactions/review - 評價

---

## TODO List
 完成用戶註冊、登入系統
 實現品牌與服務創建功能
 實現預約系統與付款整合
 實現社群功能（追蹤、留言、點贊）
 部署至 Vercel（前端）與 Railway（後端）
  

## License  
此專案採用 MIT License，詳細內容請參閱 [LICENSE](./LICENSE) 文件。  
