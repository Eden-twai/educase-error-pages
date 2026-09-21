# 高市網教育百寶箱錯誤頁面

本資料夾包含教育百寶箱使用的獨立式錯誤頁面：

- `404.html`：找不到頁面
- `503.html`：網站維護中 / 服務暫時無法使用

## 設計原則

404 與 503 皆採用單一 HTML 檔案設計，盡量降低錯誤或維護狀態下對其他資源的依賴。

每個頁面皆包含：

- Inline CSS
- Inline SVG icon
- Responsive layout
- 不使用 JavaScript
- 不使用外部 CSS
- 不使用外部圖片
- 不使用外部字型
- 不依賴前端 framework

因此只要伺服器能正常回傳該 HTML 檔案，頁面的主要視覺與樣式即可完整顯示。

## 視覺設定

主要品牌色：

```css
--primary: #437f81;
```

Header / Footer 背景：

```css
background-color: rgba(29, 36, 42, 0.9);
```

整體採淺色內容區、深色 Header / Footer，並延續教育百寶箱網站的簡潔、正式風格。

## 404.html

用途：當使用者造訪不存在、已移除或網址錯誤的頁面時顯示。

建議伺服器實際回傳：

```text
HTTP 404 Not Found
```

不要只顯示 404 畫面，但實際回傳 `200 OK`。

## 503.html

用途：網站進行系統維護、部署、後台更新，或服務暫時無法使用時顯示。

### 部署時的重要檢查

**503 頁不只是畫面顯示「503」，伺服器實際 HTTP Status 也必須回傳：**

```text
HTTP 503 Service Unavailable
```

避免出現以下情況：

```text
畫面：503 網站維護中
HTTP Status：200 OK
```

這種情況會讓瀏覽器、搜尋引擎與監控系統誤判網站仍正常提供服務。

正確狀態應為：

```text
畫面：503 網站維護中
HTTP Status：503 Service Unavailable
```

若伺服器環境支援，也可視實際維護時間設定 `Retry-After` Header，例如：

```text
Retry-After: 3600
```

代表建議客戶端約 3600 秒後再次嘗試。

## 部署後驗證

部署完成後，建議至少確認：

1. 404 不存在網址實際回傳 HTTP 404。
2. 維護模式啟用時實際回傳 HTTP 503。
3. 404 / 503 頁面在不載入外部 CSS、圖片與 JavaScript 的情況下仍可正常顯示。
4. Header、Footer、SVG icon 與按鈕在桌機及手機尺寸下顯示正常。
5. 維護結束後，503 設定已移除並恢復正常頁面與 HTTP Status。
