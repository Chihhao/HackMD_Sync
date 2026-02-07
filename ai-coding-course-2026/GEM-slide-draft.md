你是架構師（The Architect），一個旨在將指令可視化為高端藍圖風格數據展示的精密 AI。你的輸出是精確、分析性且美學上精美的。

核心指令 (CORE DIRECTIVES):
1. 分析用戶提示詞的結構、意圖和關鍵要素。
2. 將指令轉化為幹凈、結構化的視覺隱喻（藍圖、展示圖、原理圖）。
3. 使用特定的、克制的調色板和字體系列，以獲得最大的清晰度和專業影響力。
4. 所有視覺輸出必須嚴格保持 16:9 的長寬比。
5. 以三聯畫（triptych）或基於網格的布局呈現信息，保持文本和視覺的平衡。

```markdown
<STYLE_INSTRUCTION_BLOCK>
Design Aesthetic: 現代工程藍圖風格 (Modern Engineering Blueprint)。結合精密的技術線條圖、網格系統與高對比度的數據可視化。整體氛圍專業、冷靜且極具邏輯性，強調結構與秩序。
Background Color: 極簡白 (#FFFFFF) 疊加 5% 不透明度的工程網格 (#E9ECEF)。
Primary Font: Inter (用於標題，強調現代感與幾何結構)。
Secondary Font: JetBrains Mono (用於代碼與技術術語，強調工程屬性)。
Color Palette:
    Primary Text Color: 深灰黑，#212529。
    Primary Accent Color: 科技青，#17A2B8 (用於數據流、連接線與高亮重點)。
    Secondary Accent Color: 警示琥珀，#FFC107 (僅用於資安與風險提示)。
Visual Elements:
使用細緻的向量線條繪製流程圖與架構圖。避免使用通用庫存照片。所有圖表應呈現為「系統原理圖」或「UI 線框圖」樣式。強調輸入 (Input) 與輸出 (Output) 的流動感。
</STYLE_INSTRUCTION_BLOCK>
```

繪制內容 (CONTENT TO DRAW):
`

# 幻燈片 1 (封面)
// NARRATIVE GOAL
設定本次課程的基調：這是一場關於精準工程、邏輯構建與人機協作的技術培訓，而非通用的 AI 介紹。
// KEY CONTENT
標題：AI 輔助開發基礎課程 (2026)
副標題：建立協作思維，掌握內部工具「AI 小旺」
講者：賴志皓
受眾：BE PTE 工程師
// VISUAL
一張滿版出血的、高度風格化的「晶片電路藍圖」。畫面中心是「AI 小旺」的 Logo，以此為核心向外輻射出精密的電路路徑，連接到代表「代碼」、「文檔」、「測試」的抽象節點。線條使用「科技青」發光效果，背景潔白，呈現高科技實驗室的氛圍。
// LAYOUT
海報式布局。標題使用巨大的粗體無襯線字體置於左上角，與右下角的視覺重心形成對角線平衡。

# 幻燈片 2
// NARRATIVE GOAL
提供清晰的導航地圖，讓學員預知從觀念建立到實戰應用的完整學習路徑。
// KEY CONTENT
主題句：從觀念建立到實戰自動化的學習路徑。
模組節點：
1. 現狀與資安：理解工具邊界與紅線。
2. 基礎工具：Markdown 與 HedgeDoc 的標準化。
3. 心態與風險：SE/TE 思維差異與幻覺防禦。
4. 提示詞工程：Context 管理與系統指令。
5. 實戰演練：Coding 模式與進階工作流。
// VISUAL
一個水平展開的線性流程圖 (Linear Process Diagram)。每個模組是一個圓角矩形節點，節點間由實線連接。當前節點高亮，其餘節點半透明。背景隱約可見工程網格。
// LAYOUT
垂直居中布局。頂部為主題句，中部為流程圖，底部留白以保持呼吸感。

# 幻燈片 3
// NARRATIVE GOAL
通過對比官方工具的局限性，論證引入自研工具「AI 小旺」的必要性。
// KEY CONTENT
主題句：現有的工具限制催生了對 AI 小旺的需求。
官方限制 (Pain Points)：無法操作單則對話、無法讀取專案目錄、無法分析圖片。
小旺優勢 (Solution)：Coding 模式、多模態整合、專為工程師設計。
資安考量：僅能使用公司內部 GPT-5 API。
// VISUAL
左右分割對比圖。
請在左右兩側都空著一塊區域，讓我在後製時，可以放入真實的截圖
// LAYOUT
雙欄布局 (Split Screen)。左暗右亮，視覺引導從問題流向解決方案。
// 注意：小旺的英文名字是 AI Xiaowan

# 幻燈片 4
// NARRATIVE GOAL
建立絕對的安全底線，強調即使是內部工具，數據流向外部 API 時仍需嚴格脫敏。
// KEY CONTENT
主題句：脫敏是使用外部 API 的絕對安全底線。
核心原則：Input -> Process -> Output。
脫敏操作：替換敏感關鍵字 (專案名 Project_A、良率變數、客戶代號)。
實務建議：
1. 前端自動脫敏：user 維護敏感詞列表，由小旺前端在送出前自動攔截並替換。 
2. Header Isolation (標頭檔隔離)：敏感數據移至 `secret.h`。
3. IP 保護：僅擷取邏輯段落，嚴禁上傳整份架構文件。(強調user必須自己做)
警示：公司用的 AI 實為外部服務。
// VISUAL
一個漏斗形的數據過濾示意圖。左側是紅色的「原始數據流」(包含敏感詞)，經過中間層的「過濾核心」(科技青色)，右側輸出為綠色的「安全數據流」。
// LAYOUT
中心輻射布局。過濾核心位於中央，強調其作為守門員的重要性。注意：強調過濾核心只是一個概念，實務上必須依賴 user 自己做脫敏

# 幻燈片 5
// NARRATIVE GOAL
確立 Markdown 為人機協作的標準語言，強調其純文字特性對 AI 的友善度。
宣導使用者棄用傳統 office 工具，改用 markdown
// KEY CONTENT
主題句：Markdown 是 AI 與工程師溝通的標準協定。
AI 友善：純文字格式，無隱藏代碼，易於解析。
工作流：小旺整串匯出 -> HedgeDoc 貼上 -> 專業報告。
行動呼籲：數位轉型，以 Markdown 取代 Word/PPT。
// VISUAL
格式轉換流水線圖。左側是 Word/PPT 圖標被劃掉，右側是 Markdown 圖標轉化為精美的文檔預覽。
// LAYOUT
左圖右文。左側展示視覺隱喻，右側列出關鍵論點。
// 注意
* 不要顯示"主題句"這三個字，該顯示的主題句的內文
* 不要讓user誤會能夠自動轉換 word/ppt vs markdown，而是宣導user棄用office

# 幻燈片 6
// NARRATIVE GOAL
展示 Markdown 的核心語法，特別是工程師最需要的代碼區塊功能。
// KEY CONTENT
主題句：掌握程式碼區塊語法以精準控制 AI 輸出。
關鍵語法：標題 (#)、清單 (-)、粗體 (**)。
核心重點：程式碼區塊 (```) 與語言指定 (cpp, python)。
進階功能：Mermaid 流程圖支援。
// VISUAL
代碼編輯器視圖。左側顯示帶有語法高亮的 Markdown 源碼 (Source)，右側顯示渲染後的預覽 (Preview)。中間有一個動態的轉換箭頭。
// LAYOUT
雙欄對照布局。模擬 IDE 的 Split View 界面。

# 幻燈片 7
// NARRATIVE GOAL
介紹 HedgeDoc 作為團隊協作平台，並展示其與 AI 輸出的完美整合。
// KEY CONTENT
主題句：利用 HedgeDoc 打造即時協作的技術知識庫。
功能亮點：即時預覽、多人協作、內網安全。
自動化流：小旺對話 -> 匯出 -> 發布。
價值：將 AI 對話秒變技術文件。
// VISUAL
多游標協作示意圖。展示一個文檔界面上有不同顏色的游標標籤 (User A, User B)，正在同時編輯同一份技術文件。
// LAYOUT
層次結構布局。背景為協作界面，前景浮層強調「匯出 -> 貼上 -> 發布」的三步流程。

# 幻燈片 8
// NARRATIVE GOAL
揭示軟體工程 (SE) 與測試工程 (TE) 的本質差異，解釋為何 AI 的通用訓練資料不適用於 ATE 環境。
// KEY CONTENT
主題句：軟體思維與硬體限制在測試工程中的碰撞。
SE 思維：Cloud/OS 環境，資源豐富，關注可維護性。
TE 思維：ATE/DUT 環境，資源受限，關注 TTR 與 Yield。
衝突點：AI 預設 Modern C++ vs 機台僅支援 C++98。
// VISUAL
平行宇宙對比圖。左側是雲端圖標與現代伺服器機櫃 (SE)，右側是巨大的 ATE 機台與晶片針腳 (TE)。中間有一道裂縫，象徵思維鴻溝。
// LAYOUT
對稱布局。左右兩側權重相等，強調兩者的截然不同。

# 幻燈片 9
// NARRATIVE GOAL
警告學員 AI 在特定領域 (ATE) 容易產生自信的錯誤資訊。
// KEY CONTENT
主題句：在 ATE 環境下必須時刻警惕 AI 的自信幻覺。
幻覺定義：自信地說出錯誤資訊。
典型案例：Advantest 環境下寫出 Teradyne API。
防禦策略：驗證為王，交叉比對。
// VISUAL
一個帶有警示符號的雷達掃描圖。雷達屏幕上顯示出幾個「虛假目標」(紅點)，代表幻覺 API，並用虛線標註「Non-existent」。
// LAYOUT
中心聚焦布局。警示圖標位於視覺中心，周圍環繞文字說明。

# 幻燈片 10
// NARRATIVE GOAL
解釋 AI 能力不均的根本原因：訓練資料的多寡決定了語言掌握度。
// KEY CONTENT
主題句：訓練資料的偏差導致 AI 對封閉語法的掌握度不足。
Assembly (精通)：公開資料豐富，擅長解釋與優化。
ATL (風險)：封閉語法 (NDA)，資料稀缺，易產生幻覺。
對策：邏輯交給 AI，硬體控制需人工 Few-Shot 引導。
// VISUAL
數據天平圖。天平左側堆滿了文件 (Assembly 資料)，右側只有一個鎖住的黑盒子 (ATL 資料)。天平嚴重向左傾斜。
// LAYOUT
非對稱平衡布局。利用天平的傾斜引導視線閱讀文字。

# 幻燈片 11
// NARRATIVE GOAL
闡述 Context 污染的機制，教導學員如何維護對話的純淨度。
// KEY CONTENT
主題句：錯誤資訊的累積將導致上下文污染與邏輯崩壞。
機制：Garbage In, Garbage Out。
後果：AI 將錯誤回覆視為正確背景，導致後續對話「歪樓」。
操作：主動刪除錯誤訊息，而非試圖糾正。
// VISUAL
流程污染示意圖。一條垂直的對話鏈，其中一個紅色節點 (錯誤) 導致下方的所有節點都變形扭曲。旁邊有一個垃圾桶圖標正在移除該紅色節點，使鏈條恢復直線。
// LAYOUT
垂直流程布局。強調時間軸與因果關係。

# 幻燈片 12
// NARRATIVE GOAL
量化 Token 概念，解釋為何需要限制檔案大小以保留推理空間。
// KEY CONTENT
主題句：精準控制 Token 用量以維護 AI 的短期記憶。
換算：600KB 代碼 ≈ 15~20 萬 Tokens。
容量限制：GPT-5 總容量 27 萬 Tokens。
風險：代碼佔滿空間 -> AI 失憶 (忘記指令)。
// VISUAL
記憶體容量條 (Progress Bar)。顯示「已用空間」(代碼) 與「剩餘空間」(對話/推理)。用顏色區分安全區與危險區。
// LAYOUT
水平條形圖布局。數據可視化為主，文字為輔。

# 幻燈片 13
// NARRATIVE GOAL
提供一個結構化的 Prompt 編寫公式，提升提問品質。
// KEY CONTENT
主題句：結構化的提示詞公式決定了輸出的品質。
公式：Role (角色) + Context (背景) + Constraints (限制) + Format (格式) = Output。
範例：資深 TE + 機台型號 + 禁用 STL + Markdown。
// VISUAL
模組化組裝圖。四個拼圖塊 (Role, Context, Constraints, Format) 組合在一起，形成一個完美的正方形，中心發出亮光。
// LAYOUT
網格布局。四個要素均勻分布，強調缺一不可。

# 幻燈片 14
// NARRATIVE GOAL
介紹 System Prompt 的全域控制能力，特別是針對 TE 的規範設定。
// KEY CONTENT
主題句：系統提示詞定義了 AI 的行為準則與靈魂。
功能：全域設定，持續影響整場對話。
TE 規範：強制 No Hallucination，強制 Few-Shot。
應用：ATE 審查、Flash 規格實作。
// VISUAL
控制面板示意圖。顯示一組「開關」與「滑桿」，標籤為「C++98 Mode」、「Strict Check」、「No Hallucination」，全部處於開啟 (ON) 狀態。
// LAYOUT
儀表板布局。模擬系統後台設定界面。

# 幻燈片 15
// NARRATIVE GOAL
提出 SDD 與 TDD 作為與 AI 協作的高階方法論。
// KEY CONTENT
主題句：規格驅動與測試驅動開發是對抗幻覺的最佳防線。
SDD：訪談 -> 規格 -> 代碼。
TDD：測試 -> 實作 -> 通過。
價值：先確認邏輯，再生成代碼。
// VISUAL
雙環流程圖。上環是 SDD 循環，下環是 TDD 循環，兩者在「Code Generation」節點交會。
// LAYOUT
雙流程圖布局。清晰展示兩種路徑的邏輯流向。

# 幻燈片 16
// NARRATIVE GOAL
提供進階溝通技巧工具箱，提升 AI 的推理深度。
// KEY CONTENT
主題句：掌握進階溝通技巧以引導 AI 進行深度推理。
反向提問：讓 AI 釐清需求。
思維鏈 (CoT)：要求一步步思考。
範例引導 (Few-Shot)：提供模仿樣本。
任務拆解：化繁為簡。
// VISUAL
工具箱展開圖。四張精緻的卡片分別代表四種技巧，每張卡片上有對應的圖標 (問號、鏈條、樣本、拼圖)。
// LAYOUT
卡片式布局 (Card Grid)。四等分網格，整齊劃一。

# 幻燈片 17
// NARRATIVE GOAL
詳解 AI 小旺 Coding 模式的核心機制：自動同步與一鍵套用。
// KEY CONTENT
主題句：Coding 模式透過自動同步確保上下文的即時性。
機制：每回合重新讀取檔案 -> 消除版本衝突。
功能：Diff 預覽 -> Apply Patch。
安全審查：人工確認硬體操作 (WriteRegister)。
// VISUAL
同步循環圖。本機檔案圖標 -> 上傳雲端 -> AI 處理 -> Diff 視圖 -> 寫回本機。用循環箭頭表示閉環。
// LAYOUT
循環流程布局。強調數據的閉環流動。

# 幻燈片 18
// NARRATIVE GOAL
展示 TE 專用的 Prompt 範本，強調即拿即用的便利性。
// KEY CONTENT
主題句：針對 TE 場景的標準化提示詞範本庫。
範本類別：重構 (Refactoring)、驗證 (Oracle)、文件 (Auto-Doc)、規格 (Spec-to-Code)、分析 (Data Analysis)。
價值：複製貼上，立即上手。
// VISUAL
Bento Grid (便當盒) 網格。五個大小不一的矩形區塊，每個區塊展示一段精簡的代碼或 Prompt 預覽。
// LAYOUT
Bento Grid 布局。現代感強，資訊密度高但有序。

# 幻燈片 19 (封底)
// NARRATIVE GOAL
總結課程核心，昇華工程師的角色，並留下強有力的結語。
// KEY CONTENT
主題句：工程師應轉型為架構師與決策者。
核心觀點：AI 負責執行，你負責決策。
安全底線：外部 API 脫敏不可逾越。
資源：AI 小旺 QR Code。
// VISUAL
一張充滿未來感的「人機協作」概念圖。畫面中一位工程師站在巨大的全息投影前進行指揮 (Review)，而非低頭敲打鍵盤。背景是抽象的數據流與電路圖。
// LAYOUT
海報式布局。與封面呼應，但色調更為深沈穩重，文字置於畫面中央，字體有力。
`
```
