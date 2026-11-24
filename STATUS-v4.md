# STATUS-v4 — 交付驗收結果與待修清單（2026-11-24）

> 給 Antigravity 讀：這是**上一輪（v4）的實測結果**。看完直接照 §2「要你做的事」做，唔需要重做已 PASS 的部分。
> 權威基線：**zip 版** `neo-char-brief-src-2026-11-24.zip`（= `rig.js` 55,935 bytes，有深色描邊）。
> 我方工作目錄（唔需要你訪問）：`antigravity-out-v4-20261124\zip-src\src\`。

---

## 1. 結論一句

**代碼 PASS（可以直接用嚟出片）；附件 FAIL（docx 第二次仍然走題，已棄用）。**

---

## 2. 要你做的事（按優先次序）

### P0 — 唔好再交 docx
- `角色視覺設定與製作計劃書.docx` **第二次仍然**寫「長期美語教育頻道／15 分鐘 Visual Podcast／2–8 歲兒童同家長／Bluey、Peppa Pig」。
- 我哋要嘅係：**4–6 隻貓狗（可選 2 個人類）半身角色，用嚟講 agent 分工／心理題目**。
- 已改為交 `CHARACTER-BIBLE.md`（你今次有交、123 行、內容正確）→ **以後只交 md**，唔交 docx。

### P1 — 補 `dog-03 阿記`（optional，但想要）
- v4 zip 只交 6 隻（`cat-01`／`cat-02`／`dog-01`／`dog-02`／`human-01`／`human-02`），冇 `dog-03`。
- 規格（照 README §2 第六隻之前版本）：`slot:"support"`、`species:"dog"`、`earType:"round"`、`tailType:"curly"`、`accessory:"scarf"`、`bodyW 1.02`、`bodyH 0.94`、`headScale 0.88`、`eyeGap 76`、`eyeY -64`、`eyeR 25`、`mouthY 14`、`blush 0.7`、`cFur "#5B4B8A"`、`cShirt "#7A5FB8"`。

### P2 — 欄名已對齊（我方已改 schema，你唔需要改 code）
- 你實際用嘅 28 個欄位（`cFur/cFur2/cSkin/cShirt/cShirt2/cAccent/cEye/cNose/cLine`）**已經成為正式 schema**。
- 我方之前 draft 嘅 `cBody/cHair/cPants/cShoe/cSkin2` **作廢**，`skins.schema.json` 已更新對齊你嘅實作。
- 你之後加新角色，**直接照現有 6 隻嘅欄位寫**即可。

### P3 — 已知限制（唔需要即刻修，但唔好當成 bug）
- 只半身、單手可動：唔支援走路／轉身 360。
- `turn` 係幾何壓縮（`bodyScaleX = 1 - 0.22*turn`、`headShiftX = 16*turn`、`earShiftX = -6*turn`），觀感係「側身」唔係真 3D 轉身 — 已接受。

---

## 3. 已 PASS（唔需要重做）

| 檢查 | 結果 | 證據 |
|---|---|---|
| `node --check rig.js` | PASS | 語法 OK |
| 單一 `buildRig` | PASS | `grep -c "function buildRig"` = 1 |
| 非確定性來源 | PASS | `Math.random` / `Date.now` / `setInterval` 全 0 |
| 外部資源 | PASS | `grep -c http` = 0 |
| `VIEW_BOX` 匯出 | PASS | `rig.js` 有 `export const VIEW_BOX = "-220 -440 480 465"` |
| 6 隻齊全 | PASS | 全部 core slot |
| 統一描邊風格 | PASS | 6 隻一致 ink outline |
| `thinking` 三通道 | PASS | 眉壓＋眼半垂＋嘴平，肉眼可分 |
| `both-up` | PASS | 6 隻雙手齊舉、手掌有手指線、無穿模 |
| `turn=1` | PASS | 軀幹收窄、五官右移、耳距壓縮、面部微側 |
| 確定性 | PASS | `verify.html?t=3.2&expr=default` 連截兩次 md5 = `6799f47df0232462979aa6b7bfa2f76b` |
| 220px 剪影 | PASS | 6 隻仍分得出（耳型／體寬／配件／主色） |

---

## 4. 下一輪交付格式（照呢個做）

- 交 **zip**：`neo-char-brief-src-<YYYY-MM-DD>.zip`，內含 `src/` 六個檔：`rig.js`、`skins.json`、`index.html`、`gallery.html`、`verify.html`、`CHARACTER-BIBLE.md`。
- 唔好交 docx、唔好交截圖、唔好交 `NEO_成品展示.md`（我方自己會跑）。
- 附一段 10 行內嘅自我驗收 pass/fail（照 `ACCEPTANCE.md` §A–§D 精簡版）。

---

## 5. 我方下一步（你唔需要做）

用 zip 版 v4 接 HyperFrames，出 **10 秒 6 角色 demo**（每隻講一句），驗證「換 skin 就換角色」呢個賣點。
