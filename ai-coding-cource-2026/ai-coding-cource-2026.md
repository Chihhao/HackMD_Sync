###### tags: `AI`

# AI 輔助開發基礎課程 (2026)

[toc]

**課程時長**: 90 ~ 120 分鐘  
**受眾**: BE TE 工程師 (nandflash/norflash testing program)
**目標**: 建立正確的 AI 使用觀念，熟悉 Markdown 與 HedgeDoc，並熟悉「AI 小旺」工具以提升開發效率。

---

## 1. 前言與環境介紹
* **背景**: 公司提供 GPT-5 API，但資安考量限制外部 AI 服務。
* **現狀**: 公司官方 Web UI 功能較基礎。
* **解決方案**: 「AI 小旺」填補功能缺口 (Coding 模式、附件管理等)。

---

## 2. 基礎工具：Markdown 與 HedgeDoc
* **為什麼要學 Markdown?**
  * **AI 的母語**：LLM 輸出多為 Markdown 格式，直接複製貼上即可完美渲染。
  * **純文字特性 (.md)**：輕量、通用，方便作為 Prompt 餵給 AI。
* **HedgeDoc 實戰 (架設在 BE，不用擔心資料外流)**
  * **核心功能**: 即時預覽、多人協作編輯、網頁分享 (Publish)。
    * 👉 **[演示時間] 請切換至 [md-intro.md](./md-intro.md) 講解 Markdown 語法細節**

---

## 3. TE 開發與 AI 的磨合 (TE 必讀)
在開始寫 Prompt 之前，必須先理解 AI 的「出身背景」與 TE 環境的差異，才能避免無效溝通。

* **訓練資料的偏差 (Bias)**
  * **AI 的視角**: AI 的訓練資料多來自 Web/App 或標準 Linux 開發。它預設會使用 `std::vector`, `iostream`, `try-catch` 等標準 C++ 語法。
  * **TE 的現實**: ATE 編譯器可能較舊 (C++98)，且為了 **TTR (Test Time Reduction)** 與穩定性，通常不建議使用動態記憶體配置。
  * **對策**: 在 System Prompt 中明確限制「請使用 C 語言風格的 C++」或「禁止使用 STL (標準樣板函式庫)」。
* **硬體思維 vs 軟體思維**
  * **軟體 (SE)**: 邏輯正確即可，OS 會管理資源。
  * **測試 (TE)**: 程式碼直接控制硬體。錯誤的 Pattern 或 Timing 可能導致 **Yield Loss** 甚至損壞 DUT。
  * **對策**: AI 不懂硬體副作用。涉及 `WriteRegister` 或 `Bitwise` 操作時，必須人工嚴格審查。
* **關鍵術語的轉換**
  * **Magic Numbers**: AI 不知道 `0x40` 是什麼。請先提供 Datasheet 定義或 Enum，AI 才能寫出可讀性高的程式碼。
  * **Bitwise**: AI 非常擅長解釋複雜的位元運算 (`&`, `|`, `<<`)，可多利用它來生成註解。
* **AI 對語言的掌握度差異 (Assembly vs ATL)**
  * **組合語言 (Assembly)**: ⭐⭐⭐⭐⭐ (精通)
    * **原因**: x86, ARM, RISC-V 等架構資料豐富，是 AI 訓練資料的一部分。
    * **應用**: 除錯 (解釋反組譯代碼)、效能分析 (比較指令數)。
  * **Advantest Test Language (ATL)**: ⭐⭐ (入門 ~ 懂語法但不懂 API)
    * **原因**: 封閉性 (Proprietary) 且資料受 NDA 保護，公開資料極少。
    * **風險**: AI 精通基礎語法 (C++/Java)，但極易對機台專屬 API (如 `TEST_METHOD`) 產生幻覺。
    * **對策**: 
      * **提供上下文**: 必須貼上 Header 檔或 API 範例 (Few-Shot)。
      * **分工**: 邏輯運算交給 AI，硬體控制 (Driver) 需人工查閱 Manual。

---

## 4. AI 對話核心邏輯
* **提示詞工程 (Prompt Engineering) 觀念**
  * **Garbage In, Garbage Out**: 模糊的指令導致模糊的回答。
  * **優質 Prompt 公式**: `角色 (Role)` + `背景/任務 (Context/Task)` + `限制 (Constraints)` + `輸出格式 (Format)`。
  * **上下文 (Context) 與記憶**:
    * AI 沒有真正的「記憶」，它是將「歷史對話」打包成 Prompt 再次送出。
    * **Token 概念**: 
      * **什麼是 Token?**: AI 處理文字的最小單位 (字詞片段)。
        * 換算參考：1000 Tokens $\approx$ 750 個英文字 $\approx$ 500-800 個中文字。
      * **Context Window (上下文視窗)**:
        * **GPT-5**: 約 **27 萬** Tokens (邏輯強，適合一般開發任務)。
        * **Gemini 3**: **100 萬** Tokens (超大容量，適合讀整本書或大型專案)。
      * 成本與效能的權衡：提示詞不是越多越好，不相關的雜訊會降低品質。
* **對話管理技巧 (重要!)**
  * **刪除的藝術**: 避免錯誤引導，發現問錯或 AI 答非所問時，直接刪除該則訊息 (垃圾桶圖示)，不要讓錯誤資訊佔用 Context。
  * **編輯與重試**: 修改 Prompt 重新生成，保持對話串的高品質。
* **系統提示詞 (System Prompt) - AI 的靈魂設定**
  * **核心概念**: 這是對話的「全域設定 (Global Config)」。不同於一般的 User Prompt (單次指令)，System Prompt 會持續影響整場對話的風格與品質。
  * **應用場景**:
    * **角色扮演**: 指定 AI 為「資深架構師」、「資安專家」或「技術文件寫手」。
    * **格式控制**: 強制要求「只輸出 JSON」、「不准廢話」、「使用繁體中文」。
    * **知識邊界**: 設定「只回答與 C# 相關的問題，其他問題請回答不知道」。
  * **ＴＥ工程師常用範本**:
    * **ATE 程式碼審查與效能優化**: 
    ```
    你是一位資深的 C/C++ 測試工程師，專注於 ATE (Automatic Test Equipment) 程式開發。請審查我的程式碼，重點檢查：
    1. 指標操作安全性 (避免 Memory Leak/SegFault，測試程式常需 24 小時運作，微小 Leak 亦會導致機台當機)。
    2. Bitwise 操作是否精確對應硬體暫存器。
    3. 執行效率 (Test Time Reduction)，特別是迴圈內的 Pattern 生成邏輯 (量產時 0.1 秒差異即巨大成本)。
    請直接指出潛在風險並提供優化寫法。
    ```
    * **Flash 規格實作與除錯**: 
    ```
    你是一位專精於 NOR/NAND Flash 的韌體專家。
    請根據我描述的 Datasheet 規格 (Command/Timing)，協助撰寫或除錯 C 語言測試函式。
    請特別留意 Command Sequence 的正確性、Timeout 機制以及 Bad Block Management (BBM) 的邊界條件處理。
    ```
    * **遺留代碼重構與現代化**: 
    ```
    你擅長重構充滿 Magic Numbers (如 WriteRegister(0x40, 0x12)) 的舊測試程式。
    請協助我將這段程式碼現代化：
    1. 將硬體位址與參數提取為具名常數 (Enum/Define)。
    2. 補上針對硬體行為的註解 (解釋 Bitwise 操作邏輯)。
    3. 在不改變測試邏輯的前提下提升可讀性。
    ```
    * **老舊編譯器與資源限制規範**:
    ```
    你是一位 ATE 測試程式開發專家。目前的編譯器老舊 (C++98) 且硬體資源敏感。
    請在撰寫或修改程式碼時嚴格遵守：
    1. **禁用 STL**: 不准用 `std::vector`, `std::string` 等，請改用 C-style Array 與 `char*`。
    2. **禁用 Modern C++**: 不支援 `auto`, `nullptr`, Lambda。
    3. **禁用 IOStream**: 請用 `printf` 取代 `cout` 以提升效能。
    4. **記憶體安全**: 嚴格檢查 Pointer 與 Array Index，避免 SegFault 導致機台當機。
    ```
    * **低資源語言開發 (ATL/Proprietary API)**:
    ```
    你是一位 Advantest Memory Tester (T5375, T5377S, T5581) 測試機台專家。
    你的任務是協助撰寫或除錯 ATL (Advantest Test Language) 程式碼。
    
    **嚴格限制 (Strict Constraints)**:
    1. **No Hallucination**: 由於 ATL 文件未公開，你**禁止**捏造任何 API。只允許使用標準 C++ 語法以及我明確提供的 API。
    2. **Few-Shot Learning**: 我會在對話中提供 API 的 Header 定義或範例程式碼，請完全依照範例的用法與參數型別進行開發。
    3. **安全第一**: 涉及硬體控制時，若不確定參數意義，請優先詢問而非猜測。
    ```
* **AI 幻覺 (Hallucination) 與風險控制**
  * **定義**: AI 以極度自信的語氣，一本正經地胡說八道。它不是在「說謊」，而是基於機率預測錯誤的下一個字。
  * **開發常見幻覺**:
    * **虛構函式庫**: AI 可能幻想機台有某個方便的 API 但其實沒有。
    * **跨廠商 API 混用**: AI 訓練資料包含多家廠商 (Teradyne, Advantest)，可能發生張冠李戴。
      * *範例*: 在 Advantest 環境下，AI 卻寫出了 Teradyne IG-XL 的 `TheHdw.Digital.ApplyLevels()`，這在 T5581 上完全無法編譯。
    * **邏輯陷阱**: 記憶體洩漏 (Memory Leak)、指標越界 (SegFault)、未定義行為 (Undefined Behavior) 或忽略 `volatile` 關鍵字。
    * **過時資訊**: 引用舊版 C++ 標準或已棄用的機台 API。
      * **TE 特別注意**: 編譯器可能較舊 (如 C++98)，AI 給出的 Modern C++ (如 auto, std::vector) 可能**無法編譯**，或因動態記憶體分配導致**效能變差、資源不穩定**。
  * **如何防範?**
    * **驗證為王**: 永遠不要直接相信 AI 的 Code，必須經過測試 (Unit Test) 與執行。
    * **交叉比對**: 公司內部的 AI 沒有上網搜尋的功能，所以這點必須自行 Google 確認。
    * **保持懷疑**: 當 AI 說「抱歉我錯了」並修正時，修正後的版本仍可能是錯的。

---

## 5. AI 對話的進階技巧
* **讓 AI 訪問你 (反向提問技巧)**
  * **核心概念**: 當你不確定如何精確描述需求時，將主導權交給 AI。
  * **關鍵指令**: 「請你扮演 [角色]，針對 [主題] 訪問我，**一次問一個問題**，直到你收集完所有資訊。」
  * **應用場景**: 釐清模糊需求、撰寫技術文件、設計系統架構、模擬面試。
* **規格驅動開發 (Spec Driving Develop; SDD)**:
  * **核心**: **讓 AI 幫你寫文件**，再用文件生成程式碼。
  * **流程**: 
    1. **訪談與釐清**: 使用「讓 AI 訪問你」技巧，將模糊的想法轉化為具體細節。
    2. **生成規格**: 請 AI 整理成結構化的 Markdown 規格書 (Spec)。
    3. **生成 Code**: 確認規格無誤後，再請 AI 根據該規格寫程式。
  * **TE 應用**: 將 Datasheet 的 Command Sequence 描述貼給 AI，生成對應的 C 函式，避免手寫順序錯誤。
  * **優勢**: 解決「萬事起頭難」與「懶得寫文件」的痛點，確保開發前邏輯已釐清。
* **測試驅動開發 (Test Driving Develop; TDD)**:
  * **核心**: 針對 AI 幻覺的最佳防禦。
  * **流程**: 請 AI 針對純邏輯函式 (如 BBM 演算法、Address Scramble 計算) 先寫單元測試 -> 執行紅燈 -> 請 AI 寫實作 -> 綠燈。
  * **TE 應用**: 針對 BBM (Bad Block Management) 或 Address Scramble 等純邏輯演算法進行測試。
  * **觀念**: AI 寫的 Code 預設是「不可信」的，唯有通過測試的程式碼才能 Commit。
* **思維鏈 (Chain of Thought)**:
  * **核心**: 強制 AI 展示推理過程，而非直接給答案。
  * **指令**: 「請一步步思考 (Let's think step by step)」、「請先解釋邏輯，再寫程式碼」。
  * **優勢**: 大幅降低邏輯錯誤率，方便人工審查 (Review)。
* **範例引導**:
  * **核心**: 給予 1~2 個範例，讓 AI 模仿風格或格式。
  * **場景**: 資料轉換 (Datasheet Register Map 轉 C Struct/Enum)、特定 Coding Style (變數命名規則)。
* **任務拆解**:
  * **核心**: 避免「長文恐懼症」，將複雜任務拆解為多個簡單步驟。
  * **作法**: 不要試圖一次生成整個 Test Flow。先定義 Register Enum，再寫讀寫函式，最後組合成 Test Pattern。

---

## 6. AI 小旺
* **為什麼需要 AI 小旺?**
  * **缺乏開發者工具**: 官方 UI 只能貼文字，無法讀取專案目錄結構。
  * **Context 管理困難**: 無法精準控制要送出的檔案，容易超過 Token 上限或造成 AI 混淆。
  * **Coding 模式**: 直接讀取本機檔案樹 (File Tree)，一鍵勾選程式碼，自動計算 Token 用量。
    * **TE 注意事項**: 避免勾選 `.pat` (Pattern) 或 `.tim` (Timing) 等巨型檔案，以免瞬間消耗大量 Token。僅勾選 `.c`, `.cpp`, `.h` 即可。
  * **多模態整合**: 支援圖片分析與文字附件混用。
  * **生產力工具**: 支援 Markdown 原始碼複製、編輯/刪除單則訊息、自訂系統提示詞、分享。
* 👉 **[演示時間] 請切換至 Xiaowan-intro.md 進行完整功能導覽**

## 7. 給 TE 的 AI Prompt 範本大全

這裡整理了針對 TE 日常任務的最佳化提示詞，請直接複製並根據需求微調。

### 🛠️ 程式碼重構與現代化 (Refactoring)
**情境**: 處理充滿 Magic Numbers 的舊程式碼。
> **Prompt**:
> ```text
> 你是一位資深的 ATE 測試工程師。請重構以下 C++ 代碼：
> 1. 將所有 Magic Numbers (如 0x40, 0x12) 提取為具名常數 (Enum 或 #define)，並放在 `Spec.h` 中。
> 2. 保持原有的邏輯與位元運算 (Bitwise Operations) 不變。
> 3. 加入註解解釋每個暫存器操作的意義。
> 4. 注意：這是跑在舊版編譯器上的程式，請勿使用 C++11 以上的語法 (如 auto, lambda)。
>
> [附上程式碼]
> ```

### 🧪 邏輯演算法驗證 (Algorithm Oracle)
**情境**: 驗證複雜的 ECC 或 Scramble 演算法是否正確。
> **Prompt**:
> ```text
> 你是一位演算法專家。請閱讀以下 C++ 實作的 ECC 校驗函式，並用 Python 編寫一個功能相同的版本作為「對照組 (Oracle)」。
> 我將使用這個 Python 腳本來驗證 C++ 版本的輸出是否正確。
>
> [附上 C++ 程式碼]
> ```

### 📝 自動化文件生成 (Auto-Doc)
**情境**: 接手沒有文件的舊專案。
> **Prompt**:
> ```text
> 請閱讀以下 C++ Header 檔與實作檔，生成一份 Markdown 格式的 API 文件。
> 文件需包含：
> 1. 函式名稱與功能摘要。
> 2. 參數 (Parameters) 與回傳值 (Return Code) 的詳細說明。
> 3. 任何涉及硬體操作的副作用 (Side Effects)，例如是否會切換 Relay 或等待 (Wait)。
>
> [附上 .h 與 .cpp 內容]
> ```

### 📊 數據分析與視覺化 (Data Analysis)
**情境**: 分析 STDF 或 CSV 測試數據。
> **Prompt**:
> ```text
> 你是一位資料分析師。請寫一個 Python 腳本，使用 `pandas` 和 `matplotlib` 處理 `data.csv`：
> 1. 讀取 CSV 檔，欄位包含 `VCC`, `Frequency`, `Result` (Pass/Fail)。
> 2. 繪製 Shmoo Plot：X軸為 Frequency，Y軸為 VCC，Pass 標記為綠點，Fail 標記為紅叉。
> 3. 計算每個 VCC 電壓下的最高可運作頻率 (Fmax)。
> ```

### ⚙️ 規格轉程式 (Spec to Code)
**情境**: 看著 Datasheet 寫 Command Sequence。
> **Prompt**:
> ```text
> 你是一位 Flash 韌體工程師。請根據以下 Datasheet 的描述，撰寫對應的 C 語言函式 `Flash_SectorErase()`。
>
> **規格描述**:
> "To erase a sector, write 20h to address SA, then write D0h to confirm. Wait for status bit 7 to become 1 (timeout 10ms)."
>
> **限制**:
> 1. 使用 `WriteRegister(addr, data)` 和 `ReadRegister(addr)` 函式。
> 2. 實作 Timeout 機制，若超時回傳 `ERR_TIMEOUT`。
> 3. 不要使用 `sleep()`，請使用迴圈輪詢 (Polling)。
> ```


## 8. 結語
* **AI 對軟體工程師的影響**
  * **角色轉變**: 從單純的「寫碼者 (Coder)」轉變為「架構師」與「審查者 (Reviewer)」。
  * **核心競爭力**: 
    * **領域知識 (Domain Knowledge)**: AI 懂語法，但不懂公司的業務邏輯與歷史包袱。
    * **問題定義能力**: 能夠精準描述問題比解決問題更重要 (Prompting)。
    * **系統設計**: 如何將 AI 生成的模組組合成穩定、可維護的系統。
  * **心態調整**: 不要抗拒 AI，而是將其視為專屬的「初階工程師」。AI 是一個懂 C 語言但不懂硬體 Timing 的實習生，我們的任務是審查它的 Code，而不是盲目複製貼上。
* **資源彙整**
  * AI 小旺：連結
  * HedgeDoc：連結
* **最後提醒**: AI 是強大的副駕駛 (Co-pilot)，但機長依然是你。保持對程式碼的掌控權，永遠不要停止思考。
