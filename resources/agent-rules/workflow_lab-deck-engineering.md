---
category: workflow
purpose: 資安實戰教材與技術簡報三位一體研發生命週期（Triangular Pedagogy Lifecycle）與工程規範
combine: 可與 design_slide-deck-standards 搭配
---

# 資安實戰教材與簡報研發工程規範 (Cybersecurity Lab Deck & Handout Engineering Lifecycle)

> 本規範確立「資安實戰教材、學員手冊與技術簡報」的完整研發閉環。
> 任何資安主題（CVE 漏洞剖析、滲透測試實戰、靶場演練）均應遵循此「三位一體」生命週期，嚴禁跳過思考階段直接刻畫投影片。

---

## 核心哲學：教材三位一體 (The Triangular Pedagogy Principle)

一門高質量的資安實戰課程，其交付物並非單一的投影片，而是**三位一體的知識系統**：

```text
               ┌──────────────────────────────┐
               │    教師講稿 / 課堂教案        │
               │   (*_instructor_guide.md)    │
               │   時間配速 · 互動提問 · 防呆雷區  │
               └──────────────┬───────────────┘
                              │ 詞彙與脈絡 1:1 對齊
               ▲              ▼              ▲
               │                             │
┌──────────────┴──────────────┐               │
│      學員實作手冊            │               │ 講者提示 (Notes)
│    (*_lab_handout.md)       │               │ 萃取承接
│  實機SOP · Base64圖 · 檢核題 │               │
└──────────────┬──────────────┘               │
               │ 驗證證據鏈                   ▼
               │ 1:1 實機對焦 ┌──────────────────────────────┐
               └─────────────►│    大字互動簡報 (Open Slide)   │
                              │       (slides/.../index.tsx)  │
                              │   15頁因果骨架 · 英雄視覺 · 實體裁切│
                              └──────────────────────────────┘
```

* **投影片（大字心智模型）**：給遠端/現場投影看，講述「為什麼（Why）」，絕不放操作細節。
* **學員手冊（實作操作地圖）**：給學員螢幕前對照，講述「怎麼做（How）」，包含完整命令、實機截圖與 Exit Tickets。
* **教師講稿（課堂節奏大腦）**：給講師提詞，講述「怎麼教（Pedagogy）」，包含時間分配、提問設計與現場排錯話術。

---

## 研發生命週期四大階段 (Four-Phase Lifecycle)

### Phase 1 · 視角與因果定錨 (Mental Model & Causal Scoping)
在動筆寫任何代碼或排版前，必須先完成三項核心定錨：

1. **嚴格切分「練習者視角」vs「攻擊者視角」**：
   - **練習者視角（課程主線）**：學員已在 CDX / 靶場清單看到 CVE 名稱。任務不是通靈盲猜漏洞，而是**「理解與驗證漏洞成立條件，區分純讀檔與 RCE 的差異」**。切忌要求學生憑空猜測目錄。
   - **攻擊者視角（對照線）**：面對未知網站，必須經過 Fingerprinting ➔ Version ID ➔ Known-CVE 搜尋 ➔ 條件驗證。此段 recon 作為結尾對照思考，不喧賓奪主。
2. **前置概念採 Just-in-Time 補齊**：
   - 嚴禁開場填鴨 HTTP、CGI、URL Encoding 全套概念。
   - 建立「碰到了才補」的因果表格：
     - 遇到 `/cgi-bin/` ➔ 才補 Apache URL namespace vs Filesystem mapping。
     - 遇到 `.%2e` ➔ 才補 URL 雙重解碼與 Path Normalization 瑕疵。
     - 遇到 `/bin/sh` ➔ 才補 `ScriptAlias` 與 CGI 執行權限。
     - 遇到 POST Body ➔ 才補 CGI stdin/stdout 與 HTTP 標頭要求。
3. **安全邊界與危害分流定調**：
   - 清楚劃分 `Alias`（資訊洩漏/讀檔）與 `ScriptAlias`（指令執行/RCE）的分水嶺。
   - 清楚劃分 `daemon`（外網突破立足點）與 `root`（本地提權下半場）的權限真實差距。

---

### Phase 2 · 教材三位一體設計（純文字階段）
此階段全程在 Markdown 進行，將技術因果轉換為三種交付物：

1. **教師指導手冊 (`*_instructor_guide.md`)**：
   - **時間配速規劃**：例如 20 分鐘精準配速（0~2min 視角切分、2~5min 正常架構、5~9min 越界讀檔、9~13min 協定500、13~17min RCE反彈、17~20min 邊界對照）。
   - **互動提問設計**：「這時候問學生什麼問題？期待學生回答什麼？」。
   - **防呆禁區（Don'ts）**：明確標註「不要一開始丟 PoC」、「不要把 403 說成漏洞證據」。
2. **學員實作手冊 (`*_lab_handout.md`)**：
   - **15 步實機操作 SOP**：從登入靶場、確認 IP、測試連通、探測別名、到反彈 Shell。
   - **Base64 單檔內嵌機制**：所有實機截圖轉為 Base64 Data URI 定義於手冊底部（`[img_step_X]: data:image/png;base64,...`），確保單檔分發時離線可用、永不掉圖。
   - **隨堂檢驗（Exit Tickets）**：10 題核心觀念開放式問答，驗收學員理解深度。
3. **簡報結構大綱 (`*_slides.md`)**：
   - 提煉 15 頁核心骨架草稿，每一頁只保留「標題、單一因果、核心代碼或對照卡」。

---

### Phase 3 · 簡報視覺工程與實裝 (Open Slide Implementation)
純粹承接 Phase 1 & 2 的因果鏈，進行極致聚焦的視覺實裝：

1. **恪守單一焦點與英雄視覺（Single Focus & Amplify Hero）**：
   - 嚴禁為了填補留白而擅自塞入次要表格或排錯條目（**拒絕 Content Bloat**）。
   - 空間留白時，一律透過「放大主角」解決：核心卡片給予 `minHeight: 280~360px`，大間距 `gap: 32~48px`，卡片字級 `30~34px`，代碼 `24~26px`。
2. **實體裁切標準（Physical Crop Asset Standard）**：
   - 實機終端截圖**嚴禁使用 CSS `translate` / 3000px 放大黑客寫法**（易受全域 `max-width: 100%` 壓制造成終端推飛、露出大片白邊與開發者工具）。
   - 必須透過腳本將截圖實體裁切為「終端機本體視窗」（包含標題列與回顯內容），另存為 `*-crop.png`。
   - 簡報容器統一使用標準語法：
     ```tsx
     <div style={{ height: '562px', background: c.termBg, borderRadius: 18, overflow: 'hidden', display: 'flex', alignItems: 'center', justifyContent: 'center' }}>
       <img src={imgCrop} alt="..." style={{ width: '100%', height: '100%', objectFit: 'contain', display: 'block' }} />
     </div>
     ```
3. **講者提示（Speaker Notes）直接萃取**：
   - 每一頁投影片的 `notes` 必須直接從 `instructor_guide.md` 萃取，包含「提問問題、學生易犯錯誤、教學口訣」，使簡報自帶講稿靈魂。

---

### Phase 4 · 自動化審查與文檔退場 (Audit & Decommission)
1. **執行自動化工具審計**：
   - 執行 `Common/Script-List/automation/slide-deck-auditor`：檢查零死空間、零黑邊陷阱、語意色彩正確、零 AI 浮誇贅詞。
   - 執行 `npm run lint:slides && npm run build`：確保 15 頁 React 投影片順利編譯打包。
2. **文檔退場標準**：
   - 投影片完成實裝與驗收後，作為草稿的 `*_slides.md` 已功成身退，可直接刪除以維護目錄簡潔。
   - 學生手冊 (`*_lab_handout.md`) 與 教師講稿 (`*_instructor_guide.md`) 為教學核心資產，必須永久保留。
   - 原始截圖目錄（`image/`）在確認手冊 Base64 與簡報 `assets/` 完備後，可選擇性封存或移除。

---

## 嚴格禁止的四大反模式 (Anti-Patterns)

| 反模式名稱 | 典型違規現象 | 正確解法 |
| :--- | :--- | :--- |
| **視角混淆陷阱** | 在已知 CVE 的靶場實習中，要求學員像黑箱滲透者一樣盲猜 `/cgi-bin/` 或背诵 PoC。 | 堅持練習者主線：直指 CVE 原理驗證；黑箱 recon 僅作為總結對照。 |
| **過度塞字膨脹** | 看到簡報下半部有留白，反射性塞入排錯對照表、Docker 參數表，簡報變講義。 | 恪守單一焦點，放大主角卡片高度（`minHeight: 300px`）與間距（`gap: 40px`）。 |
| **截圖位移漂移** | 用 CSS `translate: '-550px'` 放大截圖，結果圖片被 `max-width` 壓扁，終端推飛出螢幕。 | 實體裁切為 `*-crop.png`，外層容器固定高度，圖片 `objectFit: 'contain'`。 |
| **開場名詞填鴨** | Slide 2 或第 1 章節一口氣介紹 CGI、RFC 3875、URL Hex、Apache 模組架構。 | Just-in-Time：遇到該步驟才補該知識，每頁只講一個 Why。 |

---

## 完工檢驗清單 (Definition of Done)

- [ ] **三位一體具備**：學生手冊、教師講稿、Open-Slide 投影片俱全且名詞 1:1 對齊。
- [ ] **手冊單檔可用**：實作截圖均已轉 Base64 內嵌於手冊底部，無外部路徑依賴。
- [ ] **簡報實體裁切**：所有截圖均為專用 `-crop.png`，無 CSS translate 魔法黑客。
- [ ] **講者備忘完整**：Open-Slide 每一頁均具備由教案轉化的 `notes`。
- [ ] **自動審計通過**：`slide_deck_auditor.py` 與 `npm run build` 雙雙通過 0 警告 0 錯誤。
