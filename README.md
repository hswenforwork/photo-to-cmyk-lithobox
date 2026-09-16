# 照片轉線稿＋CMYK分層＋3D浮雕工具

離線可用的純前端網頁工具：上傳一張生活照片，輸出3D列印彩色燈箱需要的全部檔案——灰階高度圖（3D列印浮雕用）、線稿（PNG＋SVG，熄燈裝飾層用）、C/M/Y三張調色透明片，以及3D列印用的STL模型。內建AI主體偵測（本機執行，人物／動物輪廓辨識），照片全程不會上傳到任何伺服器。

## 部署到 GitHub Pages

1. 在 GitHub 建立一個新的 repository（公開或私有皆可，公開才能用免費的 GitHub Pages）。
2. 把這個資料夾裡的所有檔案（`index.html` 與整個 `assets/` 資料夾）上傳到 repo 的根目錄。
   - 可以直接在 GitHub 網頁介面用「Add file → Upload files」把檔案拖進去，或用 `git push` 上傳。
   - `assets/` 資料夾裡的檔案（AI模型、onnxruntime-web、fflate）**務必整個一起上傳**，工具需要它們才能運作。
3. 到 repo 的 **Settings → Pages**，Source 選擇 `Deploy from a branch`，Branch 選擇你放檔案的分支（通常是 `main`）與資料夾 `/ (root)`，儲存。
4. 等待一兩分鐘，GitHub 會給一個網址（通常是 `https://<你的帳號>.github.io/<repo名稱>/`），開啟即可使用。

## 檔案結構

```
index.html          — 工具主頁面（單一自包含頁面，可直接雙擊在本機瀏覽器開啟測試）
assets/
  fflate.umd.js      — 用於打包ZIP下載的函式庫（MIT授權）
  ort.min.js         — onnxruntime-web（AI模型推論引擎，wasm版本，Apache-2.0授權）
  ort-wasm-simd-threaded.wasm / .mjs — 上述引擎的執行檔
  u2netp.onnx        — AI主體偵測模型（U^2-Net輕量版，來自 danielgatis/rembg 專案，Apache-2.0授權）
```

## 隱私與離線說明

- 照片只在使用者自己的瀏覽器裡處理（Canvas運算＋本機AI推論），不會上傳到任何伺服器。
- 唯一的網路連線是「首次載入時，瀏覽器向同一個網站下載 assets/ 裡的程式庫與AI模型檔案」（約19MB，只會下載一次，之後瀏覽器會快取），這與照片內容完全無關。
- 頁面已設定 Content-Security-Policy 限制對外連線僅能是同網站內的資源，技術性防止任何第三方連線。

## 已知限制 / 之後可能的加強方向

- AI主體偵測目前使用通用的顯著物體偵測模型（非針對特定類別訓練），對大部分人物、寵物（貓、狗等）效果良好，但無法保證100%準確，遇到複雜背景或多個主體時建議搭配手動筆刷微調。
- 3D模型匯出目前只提供 STL（相容性最廣，Cura／PrusaSlicer／Bambu Studio皆可直接匯入）；3MF格式較複雜，暫未提供。
- 建議實際列印/雷射雕刻/3D列印前，先用「即時合成預覽」抓大方向，實際效果仍需以實體測試微調各項參數（尤其CMY濃度與灰階厚度換算）。
