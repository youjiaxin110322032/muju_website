# Vercel 部署指南 - MUJU 網站

## 📋 概述

本指南說明如何將 MUJU 網站部署到 Vercel。Vercel 是一個現代化的網站托管平台，提供自動部署、CDN 加速和無服務器功能。

---

## 🚀 快速部署（3 步）

### 第 1 步：連接 GitHub 倉庫

1. 訪問 [Vercel.com](https://vercel.com)
2. 點擊 **"New Project"**
3. 選擇 **"Import Git Repository"**
4. 授權 Vercel 訪問您的 GitHub 帳戶
5. 搜索並選擇 `muju_website` 倉庫

### 第 2 步：配置部署設置

Vercel 會自動檢測到 `vercel.json` 配置文件，無需手動設置：

- **Project Name**: `muju-website`（自動填充）
- **Build Command**: 已在 vercel.json 中配置
- **Output Directory**: 自動檢測

點擊 **"Deploy"** 按鈕。

### 第 3 步：完成部署

部署通常需要 1-2 分鐘。完成後您將看到：

- ✅ 部署完成通知
- 🔗 臨時 Vercel 域名 (例: `muju-website.vercel.app`)
- 📊 部署分析和性能指標

---

## 📊 vercel.json 配置詳解

### 文件結構

```json
{
  "version": 2,
  "name": "muju-website",
  "builds": [ ... ],
  "routes": [ ... ],
  "headers": [ ... ],
  "redirects": [ ... ]
}
```

### 主要配置項

#### 1. **構建設置**
```json
"buildCommand": "echo 'Static site, no build needed'"
```
- 靜態網站無需構建，直接部署

#### 2. **路由配置**
```json
"routes": [
  {
    "src": "/(.*)",
    "dest": "/muju.html",
    "status": 200
  }
]
```
- 所有請求都指向 `muju.html`（單頁應用支持）
- 圖片路由優先級更高，直接提供文件

#### 3. **緩存策略**
```json
"headers": [
  {
    "source": "/photo/(.*)",
    "headers": [
      {
        "key": "Cache-Control",
        "value": "public, max-age=31536000, immutable"
      }
    ]
  }
]
```
- 圖片緩存 1 年（`max-age=31536000` 秒）
- 減少帶寬使用
- 提升加載速度

#### 4. **安全請求頭**
```json
{
  "key": "X-Content-Type-Options",
  "value": "nosniff"
},
{
  "key": "X-Frame-Options",
  "value": "SAMEORIGIN"
},
{
  "key": "X-XSS-Protection",
  "value": "1; mode=block"
}
```
- 防止內容類型嗅探
- 防止點擊劫持
- 防止 XSS 攻擊

#### 5. **URL 清潔化**
```json
"cleanUrls": true,
"trailingSlash": false
```
- 移除 `.html` 擴展名：`/muju.html` → `/`
- 移除尾部斜杠：`/about/` → `/about`

---

## 🔗 配置自訂域名

### 方法 1：通過 Vercel 控制面板

1. 進入 **Project Settings** → **Domains**
2. 點擊 **"Add Domain"**
3. 輸入您的域名 (例: `muju-studio.com`)
4. 按照 DNS 配置說明進行設置

### 方法 2：購買新域名

Vercel 支持通過 **Vercel Domains** 購買和管理域名：

1. 在 Vercel 中點擊 **"Add Domain"**
2. 選擇 **"Buy Domain"**
3. 搜索並購買您的域名
4. 自動配置 DNS

---

## 📈 性能優化

### 1. **自動 CDN 加速**

Vercel 提供全球 CDN，自動加速：
- 靜態資源（HTML、CSS、JS、圖片）
- 自動地理位置優化

### 2. **圖片優化**

Vercel 支持自動圖片優化：
- 自動轉換為 WebP
- 根據設備調整尺寸
- 自動壓縮

### 3. **緩存策略**

已在 `vercel.json` 中配置：
- **圖片**: 1 年緩存（內容永不變化）
- **HTML**: 根據需要重新驗證
- **CDN**: 全球邊界節點緩存

---

## 🔄 持續部署

### 自動部署流程

1. **推送到 GitHub**
   ```bash
   git add .
   git commit -m "更新網站內容"
   git push origin master
   ```

2. **自動觸發部署**
   - Vercel 檢測到 GitHub 推送
   - 自動構建和部署
   - 部署完成後通知

3. **部署状態**
   - 查看實時部署進度
   - 查看構建日誌
   - 查看性能指標

### 禁用自動部署

如果需要手動部署：

1. 進入 **Project Settings** → **Git**
2. 關閉 **"Automatic Deployments"**
3. 使用 Vercel CLI 手動部署

---

## 🛠️ 環境變量設置

如果未來需要添加環境變量（如 API 密鑰）：

### 通過 Vercel 控制面板

1. 進入 **Project Settings** → **Environment Variables**
2. 點擊 **"Add New"**
3. 輸入變量名和值
4. 選擇適用環境（Production、Preview、Development）

### 在代碼中使用

```javascript
// 客戶端無法訪問（需要後端處理）
// 對於靜態網站，建議使用其他方式存儲敏感信息
```

---

## 📊 監控和分析

### Vercel Analytics

在 Vercel 控制面板中查看：

- **頁面加載時間** - 實時性能指標
- **訪問地理分佈** - 來自世界各地的請求
- **邊界緩存命中率** - CDN 效率
- **部署歷史** - 所有部署記錄

### Google Analytics 集成

在 HTML 中添加 Google Analytics：

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_ID');
</script>
```

---

## 🔒 安全性設置

### 已配置的安全功能

✅ **HTTPS 自動啟用** - 所有連接都加密
✅ **安全請求頭** - 防止常見攻擊
✅ **DDoS 防護** - Vercel 提供內置保護
✅ **SSL/TLS 證書** - 自動管理

### 推薦的額外設置

1. **啟用密碼保護**（預覽環境）
   - Project Settings → Deployments → Protect with password

2. **限制部署訪問**
   - 只允許特定 IP 地址部署

3. **定期安全檢查**
   - 使用 GitHub 的安全掃描功能

---

## 🆘 故障排除

### 問題 1：部署失敗

**症狀**: 部署停留在 "Building" 或顯示錯誤

**解決方案**:
1. 查看構建日誌（Build Logs）
2. 檢查 `vercel.json` 語法
3. 確保所有文件都已提交到 GitHub
4. 清除 Vercel 緩存並重新部署

### 問題 2：圖片不加載

**症狀**: 部署後圖片顯示為空

**解決方案**:
1. 檢查文件路徑是否正確
2. 確認 `photo/` 文件夾已上傳
3. 檢查瀏覽器控制台是否有錯誤
4. 手動清除瀏覽器緩存

### 問題 3：自訂域名不工作

**症狀**: 域名指向失敗或超時

**解決方案**:
1. 驗證 DNS 記錄設置正確
2. DNS 生效通常需要 24-48 小時
3. 使用 DNS 檢查工具驗證配置
4. 聯繫域名提供商或 Vercel 支持

### 問題 4：性能問題

**症狀**: 頁面加載緩慢

**解決方案**:
1. 檢查 Vercel Analytics 識別瓶頸
2. 優化圖片大小（使用 WebP）
3. 啟用 Vercel 自動圖片優化
4. 考慮使用 Vercel 的邊界計算功能

---

## 📚 有用的命令

### 使用 Vercel CLI

```bash
# 安裝 Vercel CLI
npm install -g vercel

# 登錄 Vercel
vercel login

# 部署到 Vercel
vercel

# 部署到生產環境
vercel --prod

# 查看部署日誌
vercel logs
```

---

## 🔗 相關資源

- **Vercel 官方文檔**: https://vercel.com/docs
- **部署指南**: https://vercel.com/guides
- **CLI 文檔**: https://vercel.com/cli
- **社區支持**: https://vercel.com/support

---

## ✅ 部署檢查清單

在部署前，確認以下事項：

- [ ] 所有文件已提交到 GitHub
- [ ] `vercel.json` 配置正確
- [ ] 所有圖片路徑正確
- [ ] HTML 文件沒有本地路徑引用
- [ ] 檢查 Console 沒有錯誤

部署後：

- [ ] 訪問 Vercel 部署的 URL
- [ ] 檢查所有頁面和功能
- [ ] 驗證圖片加載
- [ ] 測試響應式設計
- [ ] 檢查性能指標
- [ ] 配置自訂域名（如需要）

---

## 💡 提示和技巧

1. **預覽環境** - Vercel 為每個 PR 創建預覽部署
2. **回滾** - 輕鬆回滾到之前的部署
3. **環境隔離** - 分開管理開發、預覽和生產環境
4. **團隊協作** - 邀請團隊成員進行部署管理
5. **Slack 集成** - 在 Slack 中接收部署通知

---

**最後更新**: 2026-11-10  
**文檔版本**: 1.0

祝您部署順利！🚀
