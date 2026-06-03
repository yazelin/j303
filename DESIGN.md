# J303 — Design System

J303 個人站的設計系統。記錄每個元素的配色、字級、間距、互動與流體引擎參數，作為延伸設計與重建的依據。所有數值對應 `index.html`（單檔實作）。

Live: https://yazelin.github.io/j303/

---

## 1. 主題機制（Theming）

兩套主題用 `body[data-theme="dark"|"light"]` 切換，CSS 全靠這個屬性 override。

| | 暗色 Dark | 淺色 Light |
|---|---|---|
| 代號 | 黑鑲金（black-gold） | 青花（blue-and-white） |
| 溫度 | 暖（紅 / 金 / 琥珀） | 冷（藍 / 深藍 / 青綠） |
| 流體混合模式 | `mix-blend:screen` | `filter:invert(1); mix-blend:multiply` |

**決定順序**（`ThemeToggle`）：
1. 讀 `localStorage('yz-theme')`，有就用。
2. 沒有 → `matchMedia('(prefers-color-scheme: light)')` 跟系統。
3. 使用者點切換鈕後寫回 `localStorage`，永遠記住手動選的。
4. 切換按鈕進場後 `attn` 脈衝 3 次提示可點。

所有主題切換有 `transition: background .6s ease`。

---

## 2. 圖層（z-index 堆疊）

由底到頂：

| z-index | 層 | 內容 |
|---|---|---|
| 0 | `.scene__base` | 主題底色 radial 漸層 |
| 1 | `#charCanvas` | matrix 微光字元 |
| 2 | `#fluidCanvas` | WebGL 流體（名字所在） |
| 3 | `#emberCanvas` | 餘燼粒子 |
| 4 | `.fx-vig` / `.fx-grain` | 暗角 + 顆粒質感 |
| 6 | `#content` | 捲動後的內容區 |
| 10 | `.scene__content` | hero 文字（kicker / 名字 / tag） |
| 20 | `.corner` / `.scrollcue` | 角落標、捲動提示 |
| 30 | `.theme` | 主題切換鈕 |

`html,body{ overflow:hidden }`，捲動由 JS 控 `--heroOp` 與內容層處理。

---

## 3. 色彩系統（角色制）

**核心原則：每個顏色綁一個「角色」，兩個主題用同一套角色，只是換色。**

### 3.1 角色表

| 角色 | 用在哪 | 暗色 | 淺色 |
|---|---|---|---|
| 底 | 背景 | `#16130f`（scene `#1c1712`→`#0b0907`） | `#eef2f6`→`#ccd6df`（body `#d4dee6`、content `#eaeef3`） |
| 主文字 | lead / 內文 | `#ece6da` / `#c7bfb0` | `#19293a` / `#41505d` |
| 標題 | `.sec-h` | `#ece6da` | `#17293a` |
| 主色（結構） | 段標、分類標題、連結、按鈕 | 朱紅 `#cf4438`（鈕底 `#bb352b`、連結 `#d9544a`） | 墨藍 `#225378`（鈕底 `#234e70`） |
| 點睛（強調） | 內文強調字 `.hl` | 金 `#d9ab57` | 青綠 `#2f8c79` |
| 名字 | hero 字標 | 香檳金白 | 深暗藍 |
| 流體氛圍 | 流體 / 光暈 / 餘燼 | 暖（朱 / 琥珀 / 金） | 冷（藍墨） |

### 3.2 規則

- 結構一律主色、強調一律點睛色，兩主題同邏輯。
- 名字**不用主色**（深紅/深藍在名字上偏警示，不適合）。
- 標題一律 solid `color`，不用 gradient（避免 `-webkit-background-clip:text` 殘留變成填滿色塊）。

### 3.3 完整元素色值

**暗色（預設）**

| 元素 | 值 |
|---|---|
| body 底 | `#16130f` |
| scene radial | `circle at 50% 38%, #1c1712 → #0b0907 72%` |
| 段標 `.sec-label` | `#cf4438` |
| 大標 `.sec-h` | `#ece6da` |
| 分類標題 `.cat-h` | `#cf4438`，底線 `rgba(207,68,56,.2)` |
| lead `.about-lead` | `#ece6da` |
| 內文 `.about-body` | `#c7bfb0` |
| 內文連結 | `#d9544a`，底線 `rgba(207,68,56,.35)`；hover `#e2796d` |
| 強調 `.hl` | `#d9ab57`（font-weight 600） |
| 卡片底 | `linear-gradient(165deg, rgba(255,247,235,.05), rgba(255,247,235,.012))` |
| 卡片邊 | `rgba(236,230,218,.1)`；hover 邊 `rgba(207,68,56,.45)` |
| 卡 hover 陰影 | `0 24px 70px rgba(0,0,0,.5), 0 0 50px rgba(192,54,44,.09)` |
| 卡標 h3 | `#f0ebdf` |
| 卡 en 小標 | `rgba(206,192,168,.6)` |
| 卡內文 p | `#c7bfb0` |
| 卡連結 | `#d9544a` |
| 主按鈕 | 字 `#f6efe2`、底 `#bb352b`、陰影 `0 5px 20px rgba(187,53,43,.3)` |
| ghost 連結 | `rgba(220,212,198,.62)`；hover `#e2796d` |
| 籌備中 tag | 字/邊 `#d6a45a` / `rgba(214,164,90,.5)` |
| 產品 h4 / p | `#eaf3f4` / `#c6d1d9` |
| 切換鈕 | 字 `rgba(255,245,235,.82)`、底 `rgba(255,255,255,.07)`、邊 `rgba(232,150,98,.45)`、`--attn rgba(232,150,98,.22)` |
| 暗角 `.fx-vig` | `radial(circle at 50% 42%, transparent 40%, rgba(2,4,8,.62))` |

**淺色 override**

| 元素 | 值 |
|---|---|
| body 底 | `#d4dee6`；scene `circle at 50% 34%, #eef2f6 → #ccd6df 80%` |
| content 底 | `linear(180deg, transparent, #eaeef3 14%)` |
| 段標 | `#225378` |
| 大標 | `#17293a` |
| 分類標題 | `#234e70`，底線 `rgba(34,78,112,.22)` |
| lead / 內文 | `#19293a` / `#41505d` |
| 內文連結 | `#225378`，底線 `rgba(34,83,120,.32)` |
| 強調 `.hl` | `#2f8c79` |
| 卡片 | 底 `rgba(255,255,255,.7)`、邊 `rgba(0,0,0,.1)`；hover 邊 `rgba(34,83,120,.45)`、陰影 `0 24px 60px rgba(0,0,0,.14)` |
| 卡標 / en / p | `#1a2228` / `rgba(58,82,104,.62)` / `#4a5862` |
| 主按鈕 | 字 `#eef4f9`、底 `#234e70`、陰影 `0 4px 14px rgba(34,78,112,.24)` |
| 切換鈕 | 字 `rgba(20,40,60,.82)`、底 `rgba(255,255,255,.55)`、邊 `rgba(34,78,112,.42)`、`--attn rgba(34,78,112,.2)` |
| 暗角 | `radial(circle at 50% 40%, transparent 50%, rgba(110,130,140,.26))` |

---

## 4. 字體（Typography）

載入：Google `Noto Serif TC`(300;500) + `Space Grotesk`(400;500)；`YazeKai`(毛筆) 為本機 AR PL UKai TW subset 內嵌 woff2 base64（只含「林亞澤」三字）。

| 元素 | 字體 | 字級 | 字重 | letter-spacing | line-height |
|---|---|---|---|---|---|
| 名字 `.mark` | YazeKai → Noto Serif TC | `clamp(3.4rem, 9vw, 7.6rem)` | 400 | `.26em` | 1 |
| kicker | Space Grotesk | `.72rem` | — | `.4em` upper | — |
| latin | Space Grotesk | `clamp(.8rem,1.4vw,1rem)` | 400 | `.62em` | — |
| tag | YazeKai → Noto Serif TC | `clamp(.9rem,1.6vw,1.05rem)` | 400 | — | 2 |
| 大標 `.sec-h` | YazeKai → Noto Serif TC | `clamp(2rem,4.5vw,3.4rem)` | 400 | `.06em` | 1.25 |
| 段標 `.sec-label` | Space Grotesk | `.72rem` | — | `.4em` upper | — |
| 分類標題 `.cat-h` | Noto Serif TC | `1.06rem` | 500 | `.06em` | — |
| lead | Noto Serif TC | `clamp(1.15rem,2.3vw,1.7rem)` | 300 | — | 1.9 |
| 內文 | Noto Serif TC | `clamp(1rem,1.5vw,1.12rem)` | 300 | — | 2 |
| 卡標 h3 | Noto Serif TC | `1.4rem` | 500 | `.04em` | — |
| 卡 en | Space Grotesk | `.66rem` | — | `.28em` upper | — |
| 卡內文 | Noto Serif TC | `.96rem` | 300 | — | 1.85 |
| 連結/標籤 | Space Grotesk | `.66–.72rem` | — | `.12–.16em` upper | — |

原則：中文走 Noto Serif TC（宋/楷）顯文氣；英文、標籤、按鈕走 Space Grotesk（mono 感）顯工程感。

---

## 5. 間距與版面

- 內容容器 `.sec`：`max-width:1080px`，padding `clamp(72px,12vh,150px) 28px`（640px 以下 `64px 20px`）。
- 課程 grid：`repeat(auto-fill, minmax(min(100%,460px), 1fr))`，gap 26px。兩欄填滿、單張不撐滿、`auto-fill` 保留空軌。
- 分類塊 `.cat-block` 上距 2.9rem；分類標題下距 1.1rem + 底線。
- 卡片內距 `.bd` `22px 24px 26px`；圓角 20px；ban 比例 `16/7`。
- 產品列 `.prod .row` 上下 22px + 分隔線 `rgba(255,255,255,.08)`。
- 連結群 `.lk` gap 14px，上距 20px。

---

## 6. 元件規格

**卡片 `.card`**：圓角 20px、`overflow:hidden`。transition `transform .4s cubic-bezier(.2,.7,.2,1), border-color .4s, box-shadow .4s`。hover 上浮 `translateY(-7px)` + 主色邊光。banner `<img.ban>` `aspect-ratio:16/7; object-fit:cover`。

**主按鈕（藥丸）**：實心主色、字 `#f6efe2`/`#eef4f9`、圓角 999px、padding `8px 17px`(卡內)或更大、柔陰影。hover `filter:brightness(1.1)`。

**Ghost 連結**：無底、暖灰/冷灰字，hover 轉主色。

**分類標題 `.cat-h`**：serif 500、主色字 + 主色底線。**籌備中 tag**：小膠囊、琥珀金邊字。

**訂閱表單 `.sub-form`**：`flex; flex-wrap; gap 12px; max-width 460px`。輸入框 `flex:1 1 200px`、圓角 999px、padding `13px 20px`、focus 變主色邊 + 底微亮。送出鈕 = 主色藥丸。成功訊息 `.sub-ok` 置中、暗 `#e9cba2` / 淺 `#234e70`。

**切換鈕 `.theme`**：固定左下、圓角 2rem、邊 1.5px、進場 `rise` + `attn` 脈衝 3 次。

**角落 `.corner`**：固定四角、Space Grotesk 小字寬距 uppercase。**捲動提示 `.scrollcue`**：固定底中、`bob` 上下飄、跟 `--heroOp` 一起淡出。

**質感層**：`.fx-grain` 全屏 fractalNoise（opacity .05, mix-blend overlay）；`#content::before` 另一層 noise（opacity .07）+ `#content` 主色微暈 radial。**深色靠材質不靠發光。**

---

## 7. Hero 流體引擎

WebGL 流體（Navier-Stokes，改自 PavelDoGreat 的 MIT 實作）。名字「林亞澤」用流體墨即時寫出，非靜態字。

### 7.1 CFG 參數

```
SIM_RES 128        速度場解析度
DYE_RES 1440       染料(畫面)解析度，高才不糊
DENSITY_DISS 1.5   染料消散，高=字清楚不殘影
VEL_DISS 1.9       速度消散，高=流動快收斂、不打轉
PRESSURE 0.1, PRESSURE_ITER 20
CURL 2.0           渦度，>0 會自我放大；要柔就壓低
SPLAT_RADIUS 0.25, SPLAT_FORCE 7000
COLOR_SPEED 5      游標筆色變化速率
```

### 7.2 渲染管線（每幀）

CURL → 渦度 → 散度 → 壓力(20 iter) → 梯度減 → advection → `stampName`(蓋名字) → `step` → `bloom` → DISP(合成+bloom)。

### 7.3 名字生成（雙溫度的關鍵）

- `nameColor()`：暗色回**香檳金白**（`r=w, g=w*0.94, b=w*0.82`，`w≈0.90–0.95`）；淺色回**暖白低藍**（`r0.95 g0.91 b0.76`）→ 經 `invert` 後變深藍墨。
- `genColor()` 流體氛圍色：暗色光譜「朱紅 0.015 → 朱橘 0.05 → 琥珀金 0.10 → 香檳金 0.125 → 深胭脂 0.98」（HSV）；淺色回暖白低藍 → invert 成涼藍墨浪花。
- **書法自書寫**：makemeahanzi 筆順（strokes + medians），`WRITE_DUR 6.5s`，遮罩沿 median 漸進顯示。
- **活墨 stamp**：寫好的筆畫遮罩每幀蓋印進染料（`STAMP` shader）。

### 7.4 Bloom / 餘燼

- Bloom：CLEAR 取染料 → BLUR ping-pong 6 次 → DISP 加回（`uBloomI 0.8`）。字會發光（暗金、淺藍）。
- 餘燼 `Embers`：上升塵光粒子，additive；暗 `226,178,108`（金琥珀）/ 淺 `60,40,30`。
- matrix 微光 `charCanvas`：暗 `222,168,110` / 淺 `40,44,52`。

### 7.5 字型與 fallback

`YazeKai` = AR PL UKai TW（ukai.ttc face 2）subset 內嵌。⚠️ Google 毛筆字（Ma Shan Zheng / Long Cang）多為簡體、缺正體「亞 / 澤」，正體繁中一定用 UKai subset 或 makemeahanzi。無 WebGL 時 `.mark` 退回顯示漸層字（`.fluid-on` class 控制）。

### 7.6 技術注意

- rAF 不受 vsync 限制時，書寫進度 `writeT` 用**真實時間**累加（首幀 `realDt` 上限 0.1s），否則瞬間寫完。
- 名字位置用 `.mark` **當下螢幕座標**蓋印；不在頂端時若重排會蓋到畫面外 → 每次真正回到頂端（`scrollY<6`）`__rebakeName()` 重蓋。
- 高 `DENSITY_DISS` + 每幀重蓋保持字清晰；`CURL` 壓低保持氛圍柔。

---

## 8. 動態（Motion）

| 名稱 | 用途 | 規格 |
|---|---|---|
| `rise` | 元素進場 | `opacity 0→1 + translateY(14px)→0`，各元素 delay 階梯（0.2s→1.4s） |
| `flow` | （fallback 漸層字流動） | `background-position` 11s linear infinite |
| `attn` | 切換鈕提示 | box-shadow 脈衝，1.9s × 3 次 ease-out |
| `bob` | 捲動提示飄動 | translateY 0↔7px，2.4s ease-in-out infinite |
| `.reveal`→`.in` | 捲動浮現 | IntersectionObserver(threshold .16) 加 class，`transition-delay` 依序錯開 |
| `--heroOp` | hero 捲動淡出 | `clamp(1 - scrollY/(innerHeight*0.62), 0, 1)`，控 canvas/scene/vig/scrollcue 透明度 |
| 招3 墨洗 | 往下捲沖墨 | `window.__wash`，注入向下速度 streak |

---

## 9. 響應式（Responsive）

- `max-width:640px`：`.sec` padding `64px 20px`。
- `max-width:680px`：
  - `.mark` `clamp(3.8rem,16vw,7.6rem)`、letter-spacing `.16em`
  - `.sec-h` `clamp(1.7rem,7vw,2.6rem)`
  - `.courses-grid` 單欄 `1fr`、gap 20px
  - kicker/latin/tag/hint/corner 字級與字距縮小
  - `.scrollcue` 底部上移

---

## 10. 色票（複製用）

```
暗色 黑鑲金
底     #16130f （scene #1c1712 → #0b0907）
文字   #ece6da / #c7bfb0    標題 #ece6da
主色   朱紅 #cf4438（鈕 #bb352b、連結 #d9544a）
點睛   金 #d9ab57
名字   香檳金白  r=w, g=w*0.94, b=w*0.82  (w≈0.90–0.95)
餘燼   226,178,108    微光 222,168,110

淺色 青花
底     #eef2f6 → #ccd6df （body #d4dee6, content #eaeef3）
文字   #19293a / #41505d    標題 #17293a
主色   墨藍 #225378（鈕 #234e70）
點睛   青綠 #2f8c79
名字   深暗藍（染料 r0.95 g0.91 b0.76 → invert）
餘燼   60,40,30
```
