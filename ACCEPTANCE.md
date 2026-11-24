# 驗收清單（我方收回後逐項跑）

交件目錄假設：`antigravity-out-vN-YYYYMMDD/src/`

## A. 靜態閘（機器）

| # | 命令 | 期望 |
|---|---|---|
| A1 | `node --check rig.js` | `SYNTAX_OK` |
| A2 | `grep -c "function buildRig" rig.js` | `1` |
| A3 | `grep -c "http" rig.js index.html gallery.html verify.html` | 全 `0` |
| A4 | `grep -Ec "Math\.random\|Date\.now\|setInterval" rig.js` | `0` |
| A5 | `grep -c "export const VIEW_BOX" rig.js` | `1` |
| A6 | `grep -c "function buildArm" rig.js` | `1` |
| A7 | `grep -c "turn" rig.js` | ≥ 10（證明有實作） |
| A8 | `python -c "import json;d=json.load(open('skins.json'));print(len(d))"` | `6`（或 7 含 support） |
| A9 | 每條 skins 對 `skins.schema.json` 校驗 | 全 pass、無缺欄 |
| A10 | 三頁都有 `fetch("./skins.json")` 且檔案存在 | pass |

## B. 執行閘（本地 server + 無頭截圖）

起服務：`python -m http.server 8804 --directory src`
截圖：`chrome --headless=new --window-size=1400,900 --virtual-time-budget=4000 --screenshot=...`

| # | URL | 檢查 |
|---|---|---|
| B1 | `verify.html?t=3.2` | 6 隻齊、無白格 |
| B2 | `verify.html?t=3.2&expr=thinking` | 眼半垂＋眉內壓＋平嘴（三通道） |
| B3 | `verify.html?t=3.2&expr=concerned` | 八字眉＋小開口（與 B2 明顯不同） |
| B4 | `verify.html?t=3.2&expr=happy` | 大開口＋腮紅加深 |
| B5 | `verify.html?t=3.2&pose=both-up` | 雙手齊舉、手腕不遮臉、無穿模 |
| B6 | `verify.html?t=3.2&pose=point` | 單手指向、手掌方向正確 |
| B7 | `verify.html?t=3.2&turn=1` | 與 turn=0 明顯不同（軀幹收窄＋五官右移） |
| B8 | `verify.html?t=3.2&only=dog-03`（如有 support） | 有 render，`slot=support` 標籤正確 |
| B9 | `index.html?id=cat-01&t=3.2` | 單角色正常 |
| B10 | `gallery.html?core=1` | 只顯示 core 4 隻＋2 human |

## C. 確定性閘

| # | 做法 | 期望 |
|---|---|---|
| C1 | 同 URL 連截兩次，`md5sum` 比對 | 相同 |
| C2 | 隔 5 秒再截一次 | 相同（證明無 wall-clock 依賴） |

## D. 視覺閘（人眼）

| # | 檢查 |
|---|---|
| D1 | 縮到 220px 寬，6 隻剪影可分（耳型／體寬／配件／主色 ≥3 項不同） |
| D2 | 六表情並排貼圖，肉眼一望分得出全部六個 |
| D3 | 無多餘裝飾（陰影／漸變／emoji／水印） |
| D4 | 嘴型跟表情一致，無「笑臉配八字眉」這類矛盾 |
| D5 | 耳朵、尾巴、配件在不同角色間無穿模 |

## E. 附件閘

| # | 檢查 |
|---|---|
| E1 | `CHARACTER-BIBLE.md` 存在，6 隻各 5–8 行、內容對得上本 brief（**不是**課程／Podcast 企劃） |
| E2 | 無 docx（如交了 docx，必須內容正確，否則列缺口） |
| E3 | 未改動已有 `id` |

## F. 結論格式

`REPORT-vN-驗收.md` 必須包含：

1. 結論一句（`PASS` / `PASS 帶缺口` / `FAIL`）
2. A/B/C 表：逐項 `pass`／`fail`＋證據（命令輸出、md5、截圖檔名）
3. D/E 逐項人眼判斷
4. 缺口清單（每項寫「影響」＋「修法」＋「誰做」）
5. 我方補建檔（如有）明確標「我方補」
6. 可執行建議二選一（採用／重生）
