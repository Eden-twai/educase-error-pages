# 高市網教育百寶箱錯誤頁面

本資料夾包含教育百寶箱使用的獨立式錯誤頁面：

- `503.html`：網站維護中 / 服務暫時無法使用

## 設計原則

503 採用單一 HTML 檔案設計，盡量降低錯誤或維護狀態下對其他資源的依賴。

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

目前 503 頁延續教育百寶箱原網站的教育平台調性，採用較活潑但仍維持正式感的配色與卡片式版面。

主要品牌色：

```css
--primary: #437f81;
--primary-dark: #2a5152;
```

輔助色：

```css
--secondary: #efbd68;
--coral: #e88973;
```

Header / Footer 背景：

```css
background-color: rgba(29, 36, 42, 0.9);
```

整體採淺色背景、白色維護資訊卡、深色 Header / Footer，並以黃、珊瑚色作為少量視覺點綴。


## 503.html

用途：網站進行系統維護、部署、後台更新，或服務暫時無法使用時顯示。

目前 503 頁面包含：

- 維護中 SVG icon
- `SYSTEM MAINTENANCE` 狀態文字
- 「網站維護中」主標題
- 維護說明
- 預定維護時間
- 回到首頁按鈕
- 高雄市政府教育局頁尾資訊

目前畫面不顯示大型 `503` 數字，因此 HTTP 狀態碼必須由伺服器設定，不能以畫面文字取代。

### 預定維護時間

部署前請將 `503.html` 內的預設時間：

```text
YYYY/MM/DD（週X）HH:MM ～ HH:MM
```

替換為實際預定維護時段。

HTML 中可搜尋：

```html
<strong class="maintenance-window__time">YYYY/MM/DD（週X）HH:MM ～ HH:MM</strong>
```

若維護時間有變動，請同步更新此欄位，避免使用者看到過期資訊。

### 首頁連結

錯誤頁可能由不同伺服器目錄或維護機制載入，為避免相對路徑因部署位置不同而失效，目前首頁連結採完整網址：

```text
https://educase.kh.edu.tw/welcome/
```

除非確認正式部署環境與路由規則，否則不建議改成 `index.html`、`../index.html` 等相對路徑。

### 部署時的重要檢查

**即使 503 頁面本身沒有顯示「503」數字，伺服器實際 HTTP Status 仍必須回傳：**

```text
HTTP 503 Service Unavailable
```

錯誤情況：

```text
畫面：網站維護中
HTTP Status：200 OK
```

這會讓瀏覽器、搜尋引擎與監控系統誤判網站仍正常提供服務。

正確狀態應為：

```text
畫面：網站維護中
HTTP Status：503 Service Unavailable
```

若伺服器環境支援，也可依實際維護時間設定 `Retry-After` Header，例如：

```text
Retry-After: 3600
```

代表建議客戶端約 3600 秒後再次嘗試。

## 部署後驗證

部署完成後，建議至少確認：

1. 維護模式啟用時實際回傳 HTTP 503。
2. 503 頁面在不載入外部 CSS、圖片與 JavaScript 的情況下仍可正常顯示。
3. 503 頁面的預定維護時間已更新為實際時段。
4. Header、Footer、SVG icon、維護時間區塊與按鈕在桌機及手機尺寸下顯示正常。
5. 「回到首頁」可正確連至 `https://educase.kh.edu.tw/welcome/`。
6. 維護結束後，503 設定已移除並恢復正常頁面與 HTTP Status。

