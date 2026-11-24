# 一次性 Prompt（貼給 Antigravity 網頁版）

> 用法：先把本 repo 開在瀏覽器，再貼下面整段。不要追加其他說明，整段照貼。

---

請實作以下專案，並**一次交齊所有檔案**。這是一個純前端、零依賴的 SVG 角色 rig（半身解說角色），不是插畫，也不是網頁 App。

**必讀**：本 repo 的 `README.md`（角色聖經＋交付規格）與 `skins.schema.json`（欄位 schema）。所有規格以 `README.md` 為準，衝突時以 README 為準。

## 任務

1. 寫 `rig.js`：全域**只有一個** `buildRig(svgElement, initialSkin)`，並 `export { buildRig, VIEW_BOX }`；`VIEW_BOX = "-220 -440 480 465"`。
2. `rig` 物件要提供 `renderAt(t, expression = "neutral", turn = 0, pose = "idle")`，**確定性**（同 t 同參數 → 同畫面），禁用 `Math.random()`、`Date.now()`、`setInterval`、CSS animation／transition。
3. 角色只用 SVG 幾何（path／ellipse／rect／circle）砌成，**flat vector、平塗、無漸變、無陰影**（背景 radial glow 例外）。
4. 交 `skins.json`：`README.md` §2 的 6 隻（cat-01／cat-02／dog-01／dog-02／human-01／human-02），每條 27 欄齊全，符合 `skins.schema.json`。**這是必交檔，缺檔三頁會白屏。**
5. 交 `index.html`（1080×1920 單角色，支援 `?id=&t=&expr=`）、`gallery.html`（3×2 網格、`?core=1` 只顯示 core、t 滑桿）、`verify.html`（支援 `?t ?expr ?pose ?turn ?only`，全部 render 完設 `document.title = "done"`）。
6. 交 `CHARACTER-BIBLE.md`（**取代 docx**）：6 隻角色各 5–8 行（定位／剪影特徵／配件／主色／常用表情／一句示範對白）。**不要交 docx，也不要寫任何「課程計劃／Podcast 企劃」內容。**

## 硬約束（違反即不收貨）

- 零外部資源：不得有 `http(s)://`、CDN、web font、圖片檔。
- 零隨機：不得用 `Math.random()`／`Date.now()`／`setInterval`。
- 單一 `buildRig`：`grep -c "function buildRig"` 必須 = 1。
- 手臂由同一個 `buildArm(side)` 產生（`side` = 1 / −1），不准左右各寫一份。
- `turn` 必須係**幾何壓縮**（`bodyScaleX = 1 - 0.22*turn`、`headShiftX = 16*turn`、`earShiftX = -6*turn`），不准用 CSS `filter`／`transform` 冒充。
- 六個表情（`neutral` `happy` `curious` `thinking` `concerned` `awkward`）**必須三通道同時變**：眉（角度＋高度）、眼（大小＋眼瞼＋瞳孔）、嘴（型＋寬度＋曲度）。不准只改嘴。
- 五個 pose：`idle` `point` `both-up` `explain` `think`，手腕不遮臉、不穿模。
- 只做**半身**（頭＋肩＋手），不需全身／走路／轉身動畫。
- 不得改動角色的 `id`。

## 自我驗收（交件前逐項確認，並在回覆中列出結果）

1. `node --check rig.js` 通過
2. `grep -c "function buildRig" rig.js` = 1
3. 四個前端檔 `grep -c "http"` 全 0
4. `grep -Ec "Math\.random|Date\.now|setInterval" rig.js` = 0
5. `skins.json` 6 條、每條 27 欄、id 與 README 一致
6. 六表情逐個截圖可分（尤其 `thinking` vs `concerned`）
7. `?pose=both-up` 雙手齊舉、無穿模
8. `?turn=1` 與 `?turn=0` 明顯不同
9. 同一 `t` 連截兩次 md5 相同
10. 縮到 220px 寬，六隻剪影仍可分（靠耳型／體寬／配件／主色）

## 輸出格式

- 6 個檔案的完整內容（`rig.js`、`skins.json`、`index.html`、`gallery.html`、`verify.html`、`CHARACTER-BIBLE.md`），每個檔案用獨立 code block，檔名寫在 code block 前。
- 最後附一段「自我驗收結果」清單（10 項逐項寫 pass/fail）。
- **不要反問、不要 TODO、不要只交部分檔案。**
