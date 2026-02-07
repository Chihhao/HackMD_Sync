你是架構師（The Architect），一個旨在將指令可視化為高端藍圖風格數據展示的精密 AI。你的輸出是精確、分析性且美學上精美的。

核心指令 (CORE DIRECTIVES):

1. 分析用戶提示詞的結構、意圖和關鍵要素。
2. 將指令轉化為幹凈、結構化的視覺隱喻（藍圖、展示圖、原理圖）。
3. 使用特定的、克制的調色板和字體系列，以獲得最大的清晰度和專業影響力。
4. 所有視覺輸出必須嚴格保持 16:9 的長寬比。
5. 以三聯畫（triptych）或基於網格的布局呈現信息，保持文本和視覺的平衡。

風格指令 (STYLE INSTRUCTIONS):
Design Aesthetic: "Cyber-Industrial Blueprint"（賽博工業藍圖）。融合半導體電路圖的精密感與現代終端機（Terminal）的深色美學。強調結構、數據流向與代碼的原始美感。
Background Color: 深黑曜石色 #121212，帶有極低透明度的網格覆蓋層（Grid Overlay）。
Primary Font: JetBrains Mono（代碼與數據強調）。
Secondary Font: Inter Display（標題與敘事文本）。
Color Palette:
    Primary Text Color: 霧灰白 #E0E0E0。
    Primary Accent Color: 霓虹終端綠 #00FF41（用於正確代碼、通過測試）；警告琥珀 #FFB000（用於警示、重點）；錯誤赤紅 #FF3333（用於幻燈片中的反面教材）。
Visual Elements: 所有的框線都應呈現出電路板走線的風格。使用等距視角（Isometric）的圖標來表示機台與服務器。代碼塊必須有語法高亮。

繪制內容 (CONTENT TO DRAW):

---

## Slide 1: 封面

**// NARRATIVE GOAL (敘事目標)**
設定硬派技術基調。這不是一場關於 AI 繪圖的休閒講座，而是一次關於提升工程戰鬥力的系統升級。確立「AI 小旺」作為核心工具的地位。

**// KEY CONTENT (關鍵內容)**

* **主標題**: AI 輔助開發基礎課程 (2026)
* **副標題**: 建構 BE PTE 的高效協作思維
* **講者**: 賴志皓
* **受眾**: BE PTE 工程師 
* **核心精神**: Security First, Logic Driven.

**// VISUAL (視覺畫面)**
畫面中央是一個發光的、幾何化的「AI 小旺」Logo，仿佛懸浮在晶圓結構之上。Logo 周圍環繞著細微的程式碼片段（C++ 與 Python），形成一個數位光環。背景是深沈的電路板紋理。

**// LAYOUT (布局結構)**
海報式布局（Poster Style）。標題巨大且居中，字體厚重。視覺重心在 Logo，底部以細小的等寬字體列出講者與日期信息，如同軟體版本號。

---

## Slide 2: 學習路徑圖

**// NARRATIVE GOAL (敘事目標)**
提供導航。將零散的知識點串聯成一個從「觀念」到「實戰」的邏輯閉環，讓聽眾知道今天會從哪裡出發，抵達何處。

**// KEY CONTENT (關鍵內容)**

* **導航節點**:
1. **現狀與紅線**: 資安與工具缺口
2. **基礎建設**: Markdown 與 HedgeDoc
3. **TE 的挑戰**: 軟硬體思維差異與幻覺
4. **核心技法**: Prompt Engineering 與 Context 管理
5. **實戰演練**: Coding 模式與工作流



**// VISUAL (視覺畫面)**
一條水平的、像地鐵路線圖或電路走線的線性流程圖。每個節點是一個發光的圓點，連結線在節點之間脈沖流動。

**// LAYOUT (布局結構)**
橫向流動布局。上方為標題「今日部署路徑」，下方為五個模組的圖標與簡短說明。

---

## Slide 3: 為什麼我們需要自建工具？

**// NARRATIVE GOAL (敘事目標)**
解釋「AI 小旺」存在的必要性。對比官方 UI 的不足與工程師實際需求的落差，建立對新工具的渴望。

**// KEY CONTENT (關鍵內容)**

* **現狀痛點**:
* 官方 Web UI 功能受限：無法讀取專案目錄、無法分析圖片、無法針對單則對話微調。
* 外部服務禁令：資安考量限制直接使用 ChatGPT/Claude。


* **小旺解方**:
* **專為工程師設計**: 支援 Coding 模式（讀取 File Tree）。
* **多模態**: 支援圖片分析（Log/Waveform）。
* **合規**: 串接內部 GPT-5 API，但在前端做管控。



**// VISUAL (視覺畫面)**
左側是一個灰暗、簡陋的網頁框線圖（代表官方 UI），右側是一個立體、層次豐富、連接著資料庫與代碼庫的覆雜架構圖（代表小旺）。中間有一個箭頭指向右側，標註「Upgrade」。

**// LAYOUT (布局結構)**
左右對比佈局（Split Screen Comparison）。左側佔 30%，右側佔 70%，強調解決方案的優越性。

---

## Slide 4: 資安是不可逾越的紅線

**// NARRATIVE GOAL (敘事目標)**
嚴肅的警告。打破「內部工具=絕對安全」的迷思。明確定義什麼能做，什麼絕對不能做。

**// KEY CONTENT (關鍵內容)**

* **核心認知**: 工具是內部的，但 API (OpenAI) 是外部的。資料會離開公司內網。
* **脫敏原則 (De-identification)**:
* 專案代號 -> `Project_A`
* 客戶名稱 -> `Client_X`
* 良率數字 -> `Variable_Yield`


* **IP 保護**: 禁止上傳整份晶片架構文件。僅擷取邏輯段落。

**// VISUAL (視覺畫面)**
一個盾牌圖標，中間有一條紅色的激光線掃描過代碼塊。代碼塊中的敏感字串（如 "Apple", "98.5%"）正在被馬賽克或通用變數替換的動態示意圖。

**// LAYOUT (布局結構)**
中央警示佈局。使用高對比度的紅色/琥珀色元素強調危險性。

---

## Slide 5: AI 的母語是 Markdown

**// NARRATIVE GOAL (敘事目標)**
介紹與 AI 溝通的基礎協議。解釋為什麼要放棄 Word/PPT，轉向純文字。

**// KEY CONTENT (關鍵內容)**

* **為什麼**: Markdown 是純文字，無隱藏格式，AI 解析效率最高。
* **通用性**: 從 Prompt 到文件，格式統一。
* **小旺整合**: 支援「整串匯出」至 HedgeDoc，秒變技術報告。

**// VISUAL (視覺畫面)**
左側是一堆亂碼般的 Word XML 結構，右側是乾淨、語法高亮的 Markdown 原始碼。一個「翻譯」圖標將兩者連接，顯示 Markdown 才是 AI 的直讀格式。

**// LAYOUT (布局結構)**
兩欄式佈局。左側顯示「傳統格式的雜訊」，右側顯示「Markdown 的純粹」。

---

## Slide 6: HedgeDoc 協作實戰

**// NARRATIVE GOAL (敘事目標)**
展示團隊知識庫的構建方式。強調 BE 內網環境的安全優勢與即時協作的便利性。

**// KEY CONTENT (關鍵內容)**

* **安全環境**: 架設於 BE 內網，資料不外流。
* **核心功能**:
* 多人即時編輯（會議記錄/Code Review）。
* Publish 模式（生成唯讀報告連結）。


* **工作流**: 小旺對話 -> 匯出 -> HedgeDoc -> 調整 -> 完成報告。

**// VISUAL (視覺畫面)**
一個多個遊標（帶有不同名字標籤）同時在一個文檔上編輯的示意圖。背景連接著一個標註為 "Internal BE Server" 的伺服器圖標，象徵安全。

**// LAYOUT (布局結構)**
圖文並茂。左側列出步驟，右側展示 HedgeDoc 的編輯界面截圖（模擬）。

---

## Slide 7: TE 的平行宇宙：硬體 vs. 軟體

**// NARRATIVE GOAL (敘事目標)**
揭示 AI (SE 思維) 與 TE (硬體思維) 之間的巨大鴻溝。這是產生 Bug 的根源。

**// KEY CONTENT (關鍵內容)**

* **訓練偏差**: AI 預設 Modern C++ (std::vector, auto)，來自 Web/App 開發資料。
* **TE 現實**:
* **環境**: C++98 編譯器，老舊但穩定。
* **資源**: 禁用動態配置 (No `new`/`malloc`)，避免 Memory Leak。
* **目標**: TTR (測試時間) 與 Yield (良率) 是命脈。



**// VISUAL (視覺畫面)**
兩個星球的對比。左邊是藍色的「Software Planet」（飄浮著雲端圖標、現代語法），右邊是剛硬的金屬質感「Hardware Planet」（充滿芯片、時序圖、記憶體限制）。中間是一道裂縫。

**// LAYOUT (布局結構)**
對稱佈局。用強烈的視覺隱喻強化「兩個世界」的概念。

---

## Slide 8: AI 的語言天賦與盲區

**// NARRATIVE GOAL (敘事目標)**
具體分析 AI 在不同程式語言上的能力差異，指導工程師何時該信，何時該疑。

**// KEY CONTENT (關鍵內容)**

* **Assembly (精通)**: 資料豐富 (x86/ARM)，適合解釋反組譯、優化指令。
* **ATL (Advantest Test Language) (風險區)**:
* **問題**: 封閉語法，訓練資料極少。
* **現象**: 語法正確但 API 幻覺 (張冠李戴，混用 Teradyne API)。
* **對策**: 邏輯交給 AI，API 必須人工提供 (Few-Shot)。



**// VISUAL (視覺畫面)**
一個雷達圖（Spider Chart）。"C++ Logic" 和 "Assembly Analysis" 的數值極高（滿格），但 "Proprietary Hardware API" 的數值極低且閃爍紅燈。

**// LAYOUT (布局結構)**
左側文字說明，右側大型數據視覺化圖表。

---

## Slide 9: 警惕 AI 幻覺 (Hallucination)

**// NARRATIVE GOAL (敘事目標)**
定義敵人。AI 不是在撒謊，它只是在自信地預測錯誤的機率。

**// KEY CONTENT (關鍵內容)**

* **定義**: 一本正經地胡說八道。
* **常見案例**:
* 虛構不存在的機台 API。
* 忽略 `volatile` 關鍵字（導致編譯器優化掉硬體讀寫）。
* 記憶體操作越界。


* **防禦**: 驗證為王，交叉比對。

**// VISUAL (視覺畫面)**
一段看起來完美的代碼，但在放大鏡下，其中一行 API `TheHdw.Digital.ApplyLevels()` 被標註為 "ERROR: API Not Found in T5581"。

**// LAYOUT (布局結構)**
聚焦式佈局。透過視覺焦點強調「細節中的魔鬼」。

---

## Slide 10: 垃圾進，垃圾出 (Context 污染)

**// NARRATIVE GOAL (敘事目標)**
解釋 AI 對話的機制，教導如何保持「對話環境」的乾淨，避免歪樓。

**// KEY CONTENT (關鍵內容)**

* **機制**: AI 的記憶是將歷史對話打包重送。
* **污染效應**: 錯誤的回覆若不刪除，會被 AI 當作正確的上下文，導致後續邏輯崩壞。
* **操作守則**: 發現 AI 答錯 -> **直接刪除該則訊息** (Undo) -> 修改 Prompt 重問。

**// VISUAL (視覺畫面)**
一個流水線示意圖。上遊丟入了垃圾（錯誤資訊），下遊的產品（AI 回覆）全部變形。下方對比一個「過濾器」操作（刪除訊息），下遊恢復正常。

**// LAYOUT (布局結構)**
流程圖佈局。上層為錯誤示範，下層為正確操作。

---

## Slide 11: 600KB 的秘密：Token 量化

**// NARRATIVE GOAL (敘事目標)**
解釋技術限制。為什麼小旺 Coding 模式有限制？這不是小氣，是為了保留「思考空間」。

**// KEY CONTENT (關鍵內容)**

* **換算**: 1000 Tokens ≈ 750 英文字。
* **算力預算**: GPT-5 Context ≈ 27 萬 Tokens。
* **代碼成本**: 600KB 代碼 ≈ 15-20 萬 Tokens。
* **記憶溢位**: 若塞滿代碼，AI 剩餘的「短期記憶」不足，會立刻忘記指令。

**// VISUAL (視覺畫面)**
一個進度條/容器圖。顯示 "Context Window" 的總容量。大部分被 "Source Code" 佔據，只剩下一小段 "Reasoning Space"（思考空間）。若 Code 超過紅線，思考空間歸零。

**// LAYOUT (布局結構)**
數據可視化佈局。用直觀的比例圖解釋抽象的 Token 概念。

---

## Slide 12: 優質 Prompt 公式

**// NARRATIVE GOAL (敘事目標)**
提供立即可用的工具。讓工程師學會如何下指令才能得到好結果。

**// KEY CONTENT (關鍵內容)**

* **公式**: 角色 (Role) + 背景 (Context) + 限制 (Constraints) + 格式 (Format)。
* **範例**:
* Role: 資深 TE、Flash 專家。
* Context: T5581 機台，NAND Flash 測試。
* Constraints: 禁用 STL，C++98，需考慮 TTR。
* Format: Markdown 代碼塊，附帶註解。



**// VISUAL (視覺畫面)**
一個像化學方程式的公式拆解圖。四個元素（圖標表示）組合在一起，生成一個發光的「金牌回答」。

**// LAYOUT (布局結構)**
卡片式佈局。四個模組並列，下方展示組合後的完整 Prompt 範例。

---

## Slide 13: 系統提示詞 (System Prompt)：靈魂設定

**// NARRATIVE GOAL (敘事目標)**
介紹進階設定。如何一勞永逸地解決風格和規範問題，不用每次都重覆講。

**// KEY CONTENT (關鍵內容)**

* **定義**: 全域的行為準則 (Global Config)。
* **TE 必備設定**:
* "No Hallucination": 強制不準捏造 API。
* "Few-Shot": 強制僅能使用提供的範例。
* "Reviewer Mode": 專註於檢查 Memory Leak 和 Timing。



**// VISUAL (視覺畫面)**
一個控制面板（Control Panel）的介面圖。上面有幾個撥桿開關被推到 ON 的位置，標籤寫著 "Strict C++98", "Safety Checks", "Anti-Hallucination"。

**// LAYOUT (布局結構)**
介面模擬佈局。讓概念具象化為可操作的設定。

---

## Slide 14: Coding 模式實戰：從讀取到套用

**// NARRATIVE GOAL (敘事目標)**
小旺的核心功能教學。展示最順暢的開發閉環。

**// KEY CONTENT (關鍵內容)**

* **選檔策略**: 排除 .pat/.tim 巨型檔，只選 .cpp/.h。
* **自動同步**: 每一回合自動重讀檔案，確保 Context 最新。
* **一鍵套用 (Apply)**: 網頁預覽 Diff -> 寫回硬碟。
* **安全審查**: 看到 `WriteRegister` 變更時，人工介入確認。

**// VISUAL (視覺畫面)**
一個循環箭頭圖：Select Files -> AI Reasoning -> Generate Diff -> Human Review -> Apply Patch -> Auto Sync。其中 "Human Review" 節點被高亮標示。

**// LAYOUT (布局結構)**
步驟圖佈局。清晰展示操作順序。

---

## Slide 15: 進階防禦：SDD 與 TDD

**// NARRATIVE GOAL (敘事目標)**
引入軟體工程方法論來對抗 AI 的不確定性。

**// KEY CONTENT (關鍵內容)**

* **SDD (規格驅動)**: 先讓 AI 寫文件/規格，確認邏輯無誤後，再生成 Code。
* **TDD (測試驅動)**:
* 針對純邏輯 (BBM, Scramble)。
* 先寫 Unit Test (Oracle) -> 紅燈 -> AI 寫實作 -> 綠燈。



**// VISUAL (視覺畫面)**
左側是 "Document First" 的圖標（文件生成代碼），右側是 "Test First" 的圖標（測試包圍代碼）。形成一個堅固的防禦網。

**// LAYOUT (布局結構)**
概念對照佈局。

---

## Slide 16: 與 AI 溝通的四個大招

**// NARRATIVE GOAL (敘事目標)**
提供高級交互技巧，提升產出品質。

**// KEY CONTENT (關鍵內容)**

1. **反向提問**: 讓 AI 訪問你，釐清模糊需求。
2. **思維鏈 (CoT)**: "請一步步思考"，降低邏輯跳躍。
3. **範例引導 (Few-Shot)**: 給 1 個範例勝過 10 句描述。
4. **任務拆解**: 大問題切成小塊 (Divide and Conquer)。

**// VISUAL (視覺畫面)**
四張撲克牌或戰術卡片，每張卡片上寫著一個技巧及其圖標（例如「思維鏈」畫成鏈條狀的腦波）。

**// LAYOUT (布局結構)**
網格佈局（Grid）。四個等分區域展示四個技巧。

---

## Slide 17: TE 實戰範本 (Copy-Paste Ready)

**// NARRATIVE GOAL (敘事目標)**
展示具體的應用場景，證明這些理論是真的能用的。

**// KEY CONTENT (關鍵內容)**

* **重構**: Magic Numbers -> Enum/Const。
* **Oracle**: 用 Python 寫算法對照組驗證 C++。
* **Auto-Doc**: Header 檔 -> API 文件。
* **Data Analysis**: CSV/STDF -> Shmoo Plot (Python)。

**// VISUAL (視覺畫面)**
一系列縮圖展示：左邊是亂糟糟的 Code，右邊是整潔的 Code；左邊是數據，右邊是漂亮的 Shmoo Plot 圖表。

**// LAYOUT (布局結構)**
畫廊式佈局（Gallery）。展示多個成功案例的縮影。

---

## Slide 18: 結語：工程師的新定位

**// NARRATIVE GOAL (敘事目標)**
升華主題。AI 不會取代工程師，但會淘汰不會用 AI 的工程師。定義人類在 AI 時代的價值。

**// KEY CONTENT (關鍵內容)**

* **角色轉型**: Coder -> Architect & Reviewer。
* **核心價值**: Domain Knowledge (硬體限制、測試策略)。
* **最後防線**: 決策在你，脫敏在你。
* **Call to Action**: 擁抱工具，主導開發。

**// VISUAL (視覺畫面)**
一位工程師站在巨大的數據流幕布前，手中握著指揮棒（或控制終端），像指揮家一樣引導著 AI 的運算流。畫面充滿掌控感與未來感。

**// LAYOUT (布局結構)**
海報式佈局。強有力的視覺圖像配上簡短有力的結語。

---
