# 無住 MUJU 網站 - 圖片實施指南

## 📸 圖片資源概述

本指南說明如何添加和優化本地圖片資源以替換當前的外部圖片。

---

## 🎨 圖片需求詳細表

### 1. Hero 部分背景圖片

**文件名**: `images/hero-tea-ceremony.jpg`

| 屬性 | 値 |
|------|-----|
| **用途** | 頁面頂部全屏背景 |
| **推薦尺寸** | 2000×1334px (16:9 比例) |
| **檔案大小** | 200-400 KB (優化後) |
| **格式** | JPG / WebP |
| **特性** | 高品質、低飽和度、日式茶道相關 |
| **主題** | 日式茶道儀式、茶具、和室環境 |

**描述**: 這是網站的首個印象。建議使用具有日式美學的圖片，展示傳統茶道環境、茶具或茶道實踐。圖片應該是高品質的，並在應用灰度濾鏡後仍然清晰（因為代碼應用了 `grayscale(100%)`）。

**建議主題**:
- 日式茶室內部
- 茶具特寫（茶碗、茶勺、茶刷）
- 茶道準備過程
- 日式園景（石頭、竹子、水）

---

### 2. Values 部分背景圖片

**文件名**: `images/texture-bg.jpg`

| 屬性 | 値 |
|------|-----|
| **用途** | Values 部分背景紋理 |
| **推薦尺寸** | 2000×1500px |
| **檔案大小** | 150-300 KB (優化後) |
| **格式** | JPG / WebP |
| **特性** | 細微紋理、低對比度、和諧色調 |
| **主題** | 紙張、陶瓷、木材或其他自然紋理 |

**描述**: 用作玻璃形態卡片背景的對比。應該是微妙的紋理，不會分散注意力，應用了灰度和低透明度。

**建議主題**:
- 和紙紋理
- 陶瓷表面
- 木板紋理
- 石頭紋理
- 棉布紋理

---

### 3. Experience 部分產品圖片

**文件名**: `images/ceramic-bowl.jpg`

| 屬性 | 値 |
|------|-----|
| **用途** | 茶碗產品展示圖 |
| **推薦尺寸** | 1200×900px (4:3 比例) |
| **檔案大小** | 200-350 KB (優化後) |
| **格式** | JPG / WebP |
| **特性** | 高質量產品照、清晰細節、黑釉效果 |
| **主題** | 黑釉茶碗（KUROYUYU CHAWAN） |

**描述**: 展示工坊核心產品的高品質照片。鼠標懸停時圖片會取消灰度並放大（實現高級效果）。應該是一個美麗、清晰的產品照片，顯示茶碗的細節和工藝。

**建議主題**:
- 黑釉陶製茶碗
- 高角度或側面視角
- 顯示釉面細節和色澤
- 自然光或專業光線
- 背景應足夠簡潔以凸顯產品

---

## 🛠️ 圖片優化步驟

### 第 1 步：準備原始圖片

1. 確保圖片符合推薦尺寸
2. 使用高品質的原始檔案
3. 確保圖片在灰度濾鏡下仍然清晰可見

### 第 2 步：壓縮和優化

**使用工具**：
- **ImageOptim** (Mac)
- **FileZilla** 或 **TinyPNG** (線上)
- **ImageMagick** (命令列)
- **FFmpeg** (進階用途)

**優化命令示例**：

```bash
# 使用 ImageMagick 調整大小和壓縮
convert input.jpg -resize 2000x1334 -quality 85 output.jpg

# 使用 WebP 轉換
cwebp -q 85 input.jpg -o output.webp
```

### 第 3 步：建立 WebP 替代版本（可選但推薦）

WebP 格式提供更小的檔案大小同時保持品質：

```bash
cwebp -q 85 images/hero-tea-ceremony.jpg -o images/webp/hero-tea-ceremony.webp
cwebp -q 85 images/texture-bg.jpg -o images/webp/texture-bg.webp
cwebp -q 85 images/ceramic-bowl.jpg -o images/webp/ceramic-bowl.webp
```

---

## 📂 文件夾結構

設置以下文件夾結構：

```
網站/
├── images/
│   ├── hero-tea-ceremony.jpg
│   ├── texture-bg.jpg
│   ├── ceramic-bowl.jpg
│   └── webp/
│       ├── hero-tea-ceremony.webp
│       ├── texture-bg.webp
│       └── ceramic-bowl.webp
├── ai_studio_code (2).html
├── OPTIMIZATION_REPORT.md
└── IMAGE_GUIDE.md
```

---

## 🔄 HTML 更新指南

### 方法 1：簡單替換（使用 JPG）

在 `ai_studio_code (2).html` 中找到並替換圖片源：

**Hero 背景**：
```html
<!-- 查找這一行: -->
<img src="https://images.unsplash.com/photo-1577401142514-9b2b5286ba37?q=80&w=2000&auto=format&fit=crop" ...>

<!-- 替換為: -->
<img src="./images/hero-tea-ceremony.jpg" ...>
```

**Values 背景**：
```html
<!-- 查找這一行: -->
<img src="https://images.unsplash.com/photo-1596435348325-1cf553f1d8c1?q=80&w=2000&auto=format&fit=crop" ...>

<!-- 替換為: -->
<img src="./images/texture-bg.jpg" ...>
```

**Experience 產品圖**：
```html
<!-- 查找這一行: -->
<img src="https://images.unsplash.com/photo-1610705198083-774404642fb2?q=80&w=1200&auto=format&fit=crop" ...>

<!-- 替換為: -->
<img src="./images/ceramic-bowl.jpg" ...>
```

### 方法 2：高級替換（支持 WebP）

使用 `<picture>` 元素提供多個格式：

```html
<picture>
    <source srcset="./images/webp/hero-tea-ceremony.webp" type="image/webp">
    <img src="./images/hero-tea-ceremony.jpg" alt="日式茶道儀式" loading="eager" fetchpriority="high" decoding="async" class="w-full h-full object-cover opacity-30 grayscale mix-blend-luminosity">
</picture>
```

---

## ⚡ 性能最佳實踐

### 圖片大小優化

| 位置 | 目標大小 | 説明 |
|------|---------|------|
| Hero | <300 KB | 關鍵內容，應最小化 |
| Values | <250 KB | 背景圖，可接受更小 |
| Experience | <300 KB | 產品展示，質量重要 |

### 格式選擇

1. **JPG / JPEG**：照片和複雜圖像的良好選擇
2. **WebP**：更小的大小（約 25-35% 更小），現代瀏覽器支持
3. **避免使用**：PNG（除非需要透明度）、GIF

### 懶加載設置

HTML 中已配置了 `loading="lazy"`，但 Hero 圖片使用 `loading="eager"` 以確保立即加載。

---

## 📝 實施檢查清單

在部署之前，請確認：

### 圖片準備
- [ ] 準備了所有 3 個必需的圖片
- [ ] 圖片符合推薦尺寸
- [ ] 使用高品質源檔案
- [ ] 圖片在灰度濾鏡下清晰可見

### 文件夾設置
- [ ] 創建 `images/` 文件夾
- [ ] 複製所有圖片文件到正確位置
- [ ] 創建 `images/webp/` 子文件夾（可選）
- [ ] 轉換圖片為 WebP 格式（可選）

### HTML 更新
- [ ] 更新 Hero 圖片源
- [ ] 更新 Values 圖片源
- [ ] 更新 Experience 圖片源
- [ ] 驗證所有圖片的 alt 文字（中文）

### 性能驗證
- [ ] 在瀏覽器中測試頁面加載
- [ ] 檢查在灰度濾鏡下的清晰度
- [ ] 驗證懶加載功能（Experience 圖片應在滾動時加載）
- [ ] 使用 Lighthouse 檢查性能分數
- [ ] 在不同設備上測試（手機、平板、桌面）

### 視覺驗證
- [ ] 圖片未被拉伸或失真
- [ ] 圖片對比度良好
- [ ] 顏色準確（特別是黑釉茶碗）
- [ ] 所有文本可讀（對應背景圖片）

---

## 🎯 推薦的圖片來源

如果沒有自己的圖片，可以從以下來源獲取：

### 免費圖片資源（符合許可證）
- **Unsplash** - https://unsplash.com/ (無需署名)
- **Pexels** - https://www.pexels.com/ (免費，高品質)
- **Pixabay** - https://pixabay.com/ (免費，無限制)
- **Shutterstock** - 付費但高品質

### 日式特定資源
- **日本文化網站** - 搜尋「日式茶道」、「茶具」
- **Pinterest** - 收集設計靈感
- **Getty Images** - 高品質專業照片

---

## 🔗 相關資源

- [Lighthouse 性能測試](https://developers.google.com/web/tools/lighthouse)
- [WebP 轉換工具](https://developers.google.com/speed/webp)
- [圖片優化指南](https://web.dev/image-optimization/)
- [Web 字體最佳實踐](https://web.dev/fonts/)

---

## 💡 提示和技巧

1. **保持一致性**：所有圖片應遵循相同的調色板和風格
2. **測試灰度效果**：檢查圖片在灰度濾鏡下是否仍清晰
3. **使用 CDN**：在生產環境中考慮使用 CDN 加速圖片傳輸
4. **定期更新**：根據季節或活動定期更新圖片
5. **備份原始文件**：保存高分辨率原始文件以供未來編輯

---

## ❓ 常見問題

**Q: 如何改變 Experience 部分圖片的灰度？**
A: 在 CSS 中修改 `grayscale(100%)` 值。0% = 全色彩，100% = 全灰度。

**Q: 圖片可以跨域加載嗎？**
A: 可以，但本地圖片通常性能更好。如果使用外部 CDN，請確保設置正確的 CORS 頭。

**Q: 如何進一步優化性能？**
A: 考慮使用圖片懶加載庫、WebP 格式、響應式圖片尺寸和 CDN 服務。

**Q: Hero 圖片需要多高的質量？**
A: 由於應用了灰度和透明度，可以使用中等質量的壓縮。目標是 200-300 KB。

---

**最後更新**: 2026-11-10  
**文檔版本**: 1.0
