# NEO 角色 Brief — 2D 半身解說角色（貓狗 4–6 隻）

> 這份是「角色聖經」＋交付規格。目標不是畫一張插畫，而是**一套能換 skin 出不同角色的代碼骨架**，用於解說片（講 agent 分工、心理題目）下半身固定鏡位。

---

## 0. 上次做錯了什麼（先讀，避免重犯）

上一次交件有兩個硬傷，這次必須避開：

1. **附件走題**：`角色視覺設定與製作計劃書.docx` 寫的是一個完全不同的項目 ——「15 分鐘美語教育 Podcast、2–8 歲兒童、Bluey/Peppa Pig 風、Teddy/Momo 六個角色」。
   → 本次**不要交 docx**。改交本 repo 內的 `CHARACTER-BIBLE.md`（角色聖經，見 §6），只寫本 brief 指定的 4–6 隻貓狗。
2. **漏交 `skins.json`**：`index.html` / `gallery.html` / `verify.html` 三頁都會 `fetch("./skins.json")`，缺檔即白屏。
   → `skins.json` 是**必交檔**，不是可選附件。

---

## 1. 用途與場景

- 用於 1080×1920 直屏解說片（少數 16:9）。
- 角色只出現**半身**（頭＋肩＋手），固定站在畫面左下或右下，佔畫面高度約 55–65%。
- 主要內容是「講嘢」：講者輪流發言、指方向、雙手比劃、沉思、表達情緒。
- **不需要**：走路、轉身奔跑、全身、換裝、戰鬥動作、3D 視角。

---

## 2. 角色陣容（4 core + 2 optional human）

命名固定，不得改 id。所有角色共用**同一個 rig**，差異只在 `skins.json`。

| id | 名 | 角色定位 | 物種 | 耳型 | 尾 | 配件 | bodyW | 主色（毛／衣） |
|---|---|---|---|---|---|---|---|---|
| `cat-01` | 阿問 | 主持／提問 | cat | pointy | curl | glasses | 0.82 | 深藍毛 `#1F2A44`／青綠毛衣 `#2E6F6A` |
| `cat-02` | 阿做 | 執行／助手 | cat | pointy | straight | headset | 0.98 | 橙棕毛 `#D9713B`／lime 背心 `#A8D93B` |
| `dog-01` | 阿檢 | 分析／審核 | dog | flop | fluffy | scarf | 1.24 | 灰藍毛 `#7C8FA3`／白襯衫 `#F2F4F7` |
| `dog-02` | 阿感 | 情緒／觀察 | dog | round | curly | none | 1.08 | 米金毛 `#D9B98A`／啡針織 `#8A5B3D` |
| `human-01` | 教授 | 權威／長者 | human | — | — | glasses | 1.18 | 白髮 `#E8E4DC`／深灰西裝 `#2B2F38` |
| `human-02` | 阿青 | 實習／少年 | human | — | — | none | 0.80 | 深藍短髮 `#221F33`／牛仔藍 hoodie `#4A6FA5` |

**剪影可辨要求**：把畫面縮到 220px 寬，六隻仍要一眼分得出。可辨靠四樣：**耳型、體寬、配件、主色**（四項至少三項不同）。

---

## 3. Rig 契約（不可改）

```js
// rig.js —— 全域只准一個 buildRig
export const VIEW_BOX = "-220 -440 480 465";   // x: -220..260, y: -440..25, y=0 = 腳底
export function buildRig(svgElement, initialSkin) { /* ... */ return rig; }
rig.renderAt(t, expression = "neutral", turn = 0, pose = "idle"); // 純函數：同 t 同參數 → 同畫面
```

- `renderAt` 必須是**確定性**：只依賴 `t` 與參數，不准用 `Math.random()`／`Date.now()`／`setInterval`／`requestAnimationFrame` 內部計時。
- 動作以「時間函數」計算（呼吸用 sin、眨眼用時間表），不要用 CSS animation／transition。
- **不准用 CSS `filter`／`transform` 造假**（例如用 scaleX 假轉身）——所有形變要在 SVG 幾何層做。
- 只准一個 `buildRig` 定義（`grep -c "function buildRig"` 必須 = 1）。
- 不得載入任何外部資源：零 `http(s)://`、零 CDN、零 web font、零圖片檔。全部 SVG 幾何。
- 風格：flat vector，**平塗、無漸變、無陰影**（背景 radial glow 例外）。

---

## 4. `skins.json` 規格（27 欄，缺一即不合法）

```json
{
  "id": "cat-01",
  "name": "阿問",
  "slot": "core",
  "species": "cat",
  "earType": "pointy",
  "tailType": "curl",
  "accessory": "glasses",
  "bodyW": 0.82,
  "bodyH": 0.94,
  "headScale": 0.92,
  "eyeGap": 80,
  "eyeY": -64,
  "eyeR": 27,
  "mouthY": 14,
  "blush": 0.5,
  "whiskers": true,
  "muzzle": "small",
  "cFur": "#1F2A44",
  "cFur2": "#2B3A5C",
  "cSkin": "#F6D9C0",
  "cShirt": "#2E6F6A",
  "cShirt2": "#245751",
  "cAccent": "#A8D93B",
  "cEye": "#1A1A1F",
  "cNose": "#3B3B45",
  "cLine": "#101015",
  "turnDefault": 0,
  "notes": "主持／提問"
}
```

欄位可選值：

| 欄 | 可選值 |
|---|---|
| `slot` | `core` / `support` |
| `species` | `cat` / `dog` / `human` |
| `earType` | `pointy` / `flop` / `round` / `none` |
| `tailType` | `curl` / `straight` / `fluffy` / `curly` / `none` |
| `accessory` | `glasses` / `headset` / `scarf` / `cap` / `tie` / `none` |
| `muzzle` | `small` / `wide` / `none` |
| `bodyW` / `bodyH` | 0.7–1.3（1.0 = 標準） |
| `headScale` | 0.85–1.15 |
| `eyeGap` | 68–96 |
| `eyeR` | 22–30 |
| `blush` | 0–1 |

`id` 一旦公開**不得改名**（下游片場已按 id 引用）。

---

## 5. 表情與動作（本次重點）

### 5.1 六個表情 preset —— 必須三通道同時變，不准只改嘴

| preset | 眉 | 眼 | 嘴 | 其他 |
|---|---|---|---|---|
| `neutral` | 平 | 正常 | 微笑閉嘴 | — |
| `happy` | 上揚 | 正常，眼尾上 | 大開口 | 腮紅 ×1.3 |
| `curious` | 一高一低 | 眼放大 1.1，瞳孔偏上 | 小圓嘴 | — |
| `thinking` | 內壓下斜 | 上眼瞼垂 40% | 平嘴／小直線 | 視線偏上 |
| `concerned` | 八字眉 | 正常，眼球偏側 | 小開口 | 眉距收窄 |
| `awkward` | 一平一斜 | 一眼瞇 | 波浪嘴 | 腮紅偏移一側 |

> 驗收會逐個截圖比對：**「眉角度＋高度」「眼大小＋眼瞼」「嘴型」三件必須同時肉眼可辨**。

### 5.2 五個 pose

`idle`（手自然垂）· `point`（單手指向）· `both-up`（雙手齊舉）· `explain`（一手掌向上、一手輔助）· `think`（一手托腮）

手臂必須由**同一個 `buildArm(side)`** 產生（`side` = 1 / −1），左右手對稱、手腕不遮臉、不穿模。

### 5.3 `turn`（0–1，幾何壓縮，不是 CSS filter）

```
bodyScaleX = 1 - 0.22 * turn
headShiftX = 16 * turn
earShiftX  = -6 * turn
```

`turn=1` 要肉眼明顯（3/4 轉向感），但**不准**用 `filter: blur()`／`transform: scale()` 冒充。

---

## 6. 交付物（6 個檔，缺一不可）

| 檔 | 要求 |
|---|---|
| `rig.js` | 單一 `buildRig`、`export { buildRig, VIEW_BOX }`、`renderAt(t, expression, turn, pose)`、零外部依賴 |
| `skins.json` | array，≥6 條（4 core + 2 human），27 欄齊 |
| `index.html` | 1080×1920 單角色展示，可用 query `?id=cat-01&t=3.2&expr=thinking` |
| `gallery.html` | 3×2 網格、`?core=1` 只顯示 core、t 滑桿 |
| `verify.html` | 支援 `?t ?expr ?pose ?turn ?only`；全部 render 完成後設 `document.title = "done"` |
| `CHARACTER-BIBLE.md` | **取代 docx**：每隻角色 5–8 行（定位、剪影特徵、配件、主色、常用表情、一句示範對白） |

---

## 7. 自我驗收（交件前自己跑一次，缺一不算完成）

1. `node --check rig.js` 通過。
2. `grep -c "function buildRig" rig.js` = 1。
3. `grep -c "http" rig.js index.html gallery.html verify.html` 全 0。
4. `grep -Ec "Math\.random|Date\.now|setInterval" rig.js` = 0。
5. `skins.json` 每條 27 欄齊、id 與本 brief 一致。
6. 六個表情逐個截圖，肉眼分得出（尤其 `thinking` vs `concerned`）。
7. `?pose=both-up` 雙手齊舉、無穿模。
8. `?turn=1` 與 `?turn=0` 明顯不同。
9. 同一 `t` 連截兩次，md5 相同（確定性）。
10. 縮到 220px 寬，六隻剪影仍可分。

---

## 8. 收回後的落地流程（我方）

靜態閘 → 本地 `http.server` + 無頭截圖 → 逐項視覺覆核 → 寫 `REPORT-驗收.md`（過／不過＋缺口）→ 接入 HyperFrames 出片（`renderAt(t, expr, turn, pose)` 逐幀驅動）。

> 一句話規格：**一個 SVG 骨架 + 一條 skins.json = 六個角色 + 六個表情 + 五個動作，全部確定性、零外部依賴。**
