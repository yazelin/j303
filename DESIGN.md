# J303 — Design System

J303 個人站的設計系統：配色、字體、版面與互動規格，作為延伸設計的依據。

Live: https://yazelin.github.io/j303/

---

## 1. 雙主題（Dual-Theme）

兩套溫度，不是單純亮/暗開關。

| | 暗色 Dark | 淺色 Light |
|---|---|---|
| 代號 | 黑鑲金（black-gold） | 青花（blue-and-white） |
| 溫度 | 暖（紅 / 金 / 琥珀） | 冷（藍 / 深藍 / 青綠） |
| 底 | 暖墨黑 | 冷藍灰水面 |
| 名字字標 | 香檳金白 | 深暗藍 |

主題邏輯：首次進站跟系統 `prefers-color-scheme`；手動切換後存 `localStorage('yz-theme')`，之後記住手動選的。

---

## 2. 色彩系統（角色制）

核心原則：**每個顏色綁一個「角色」，兩個主題用同一套角色，只是換色。**

| 角色 | 用在哪 | 暗色 | 淺色 |
|---|---|---|---|
| 底 | 背景 | `#16130f` | `#eef2f6` → `#ccd6df` |
| 主文字 | lead / 內文 | `#ece6da` / `#c7bfb0` | `#19293a` / `#41505d` |
| 標題 | `.sec-h` | `#ece6da` | `#17293a` |
| 主色（結構） | 段標、分類標題、連結、按鈕 | 朱紅 `#cf4438`（鈕底 `#bb352b`） | 墨藍 `#225378`（鈕底 `#234e70`） |
| 點睛（強調） | 內文強調關鍵字 `.hl` | 金 `#d9ab57` | 青綠 `#2f8c79` |
| 名字 | hero 字標 | 香檳金白 | 深暗藍 |
| 流體氛圍 | hero 流體 / 光暈 / 餘燼 | 暖（朱 / 琥珀 / 金） | 冷（藍墨） |

規則：

- 結構一律用主色、強調一律用點睛色，兩個主題同邏輯。
- 名字素雅，**不用主色**（深紅/深藍在名字上偏警示，不適合）。
- 標題一律 solid `color`，不用 gradient（避免 `background-clip:text` 殘留變色塊）。

---

## 3. Hero — 流體字標

hero 是 WebGL 流體（Navier-Stokes，改自 PavelDoGreat 的 MIT 實作）。名字「林亞澤」用流體墨即時寫出，非靜態字。

- **書法自書寫**：makemeahanzi 筆順（strokes + medians）按真實筆順渲染（`WRITE_DUR` 6.5s）。
- **游標即毛筆**：滑鼠攪墨，慢=粗、快=細。
- **活墨 stamp**：筆畫遮罩每幀蓋印進染料 + bloom 輝光 + 餘燼粒子。
- **雙溫度墨**：暗色 `mix-blend:screen`，名字香檳金白；淺色 `invert(1)+multiply` 水墨，染料調「暖白、低藍」→ 反相成深藍墨。
- **字型** `YazeKai` = AR PL UKai TW（ukai.ttc face 2）subset 內嵌（正體繁中毛筆；Google 毛筆字多為簡體，缺「亞 / 澤」）。

技術注意：

- rAF 不受 vsync 限制時，書寫進度用**真實時間**累加（首幀上限 `0.1s`），否則瞬間寫完。
- 每次回到頂端（`scrollY<6`）重蓋名字遮罩，避免捲動後錯位。
- 高 `DENSITY_DISS` + 每幀重蓋，保持字清晰。

---

## 4. 字體層級

| 用途 | 字體 | 特徵 |
|---|---|---|
| 名字字標 | `YazeKai`（毛筆）→ `Noto Serif TC` | 行楷 |
| 大標 / 內文 | `Noto Serif TC` | 300–500 |
| 段標 / 標籤 | `Space Grotesk` | 小字、寬字距、uppercase |

中文走 serif 顯文氣、英文/標籤走 Space Grotesk 顯工程感。

---

## 5. 版面與元件

- **結構**：hero → 關於 → 課程牆 → 產品 → 訂閱 → 頁尾。
- **課程牆資料驅動**：`COURSES` 陣列 + `CATS` 分類，`#coursesRoot` 用 JS 渲染、分類分塊。加課/加分類 = 改資料，不動版面。
- **Grid**：`repeat(auto-fill, minmax(min(100%,460px),1fr))`，兩欄填滿、單張不撐滿、窄螢幕單欄。
- **卡片**：漸層底 + 主色 hover 邊光。
- **按鈕**：實心主色藥丸（圓角 `999px`），hover `brightness(1.1)`。
- **質感層**：極淡 fractalNoise 顆粒（`opacity .06–.07, mix-blend overlay`）+ 主色微暈。深色靠材質不靠發光。
- **動態**：書法進場、`IntersectionObserver` reveal、`--heroOp` 捲動淡出、切換鈕 `attn` 脈衝。

---

## 6. 色票（複製用）

```
暗色 黑鑲金
底     #16130f （scene #1c1712 → #0b0907）
文字   #ece6da / #c7bfb0    標題 #ece6da
主色   朱紅 #cf4438（鈕 #bb352b、連結 #d9544a）
點睛   金 #d9ab57
名字   香檳金白  r=w, g=w*0.94, b=w*0.82  (w≈0.90–0.95)
餘燼   226,178,108

淺色 青花
底     #eef2f6 → #ccd6df （body #d4dee6, content #eaeef3）
文字   #19293a / #41505d    標題 #17293a
主色   墨藍 #225378（鈕 #234e70）
點睛   青綠 #2f8c79
名字   深暗藍（染料 r0.95 g0.91 b0.76 → invert）
```
