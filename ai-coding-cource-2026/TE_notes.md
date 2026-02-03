# 給 SE 的 TE 領域知識補給

這份文件旨在協助軟體工程師 (SE) 深入理解測試工程師 (TE) 的工作領域、術語與痛點，以便更有效地進行跨部門合作、工具開發或 AI 導入。

## 1. 核心差異：SE 與 TE 的平行宇宙

理解這些差異是與 TE 溝通的基礎。TE 的程式碼不是跑在雲端，而是跑在昂貴的精密儀器上。

| 特性 | 軟體工程 (SE) | 測試工程 (TE) |
| :--- | :--- | :--- |
| **運行環境** | Server, Cloud, Mobile (OS 資源豐富) | **ATE (Automatic Test Equipment)** <br> (如 Advantest V93000, Teradyne J750，體積如冰箱大) |
| **操作對象** | 資料庫, API, UI | **DUT (Device Under Test)**, 針腳 (Pin), 硬體暫存器 |
| **核心 KPI** | 可維護性, 擴充性, 使用者體驗 | **TTR (測試時間)**, **Yield (良率)**, 穩定性 |
| **資源管理** | OS 負責記憶體回收 (GC/RAII) | **手動管理** (避免動態配置，微小 Leak 會導致機台當機) |
| **錯誤處理** | Try-Catch, Retry 機制 | **Return Code**, 嚴格檢查 (Exception 可能導致硬體狀態殘留) |

### 關鍵 KPI 解析
*   **TTR (Test Time Reduction)**：測試一顆晶片多花 0.1 秒，量產幾百萬顆時就是巨大的成本。
    *   *SE Note*: AI 優化程式碼時，必須考慮執行效率，減少迴圈內的冗餘計算。
*   **Yield (良率)**：程式寫錯可能導致 **Overkill** (誤殺良品) 或 **Underkill** (放過壞品)。
    *   *SE Note*: AI 需協助檢查邊界條件 (Boundary Check) 以避免誤判。

## 2. 程式碼特徵與物理限制

TE 的程式庫通常累積了數十年的歷史，且受限於機台編譯器與物理定律。

### 語言特性
*   **Legacy C/C++ 混合體**：許多程式是 20 年前寫的 C code，後來混用了 C++。編譯器可能還停在 **C++98**。
    *   *SE Note*: AI 預設生成的 Modern C++ (auto, lambda, smart pointers) 往往無法編譯。
*   **Magic Numbers (魔術數字)**：程式碼中充斥著 `WriteRegister(0x40, 0x12);`。
    *   *原因*: 直接對應 Datasheet 的 Register Address。
    *   *機會*: 這是 AI 重構的最佳切入點 (轉為 Enum/Define)。
*   **Bitwise Operation (位元運算)**：大量的 `&`, `|`, `<<`, `>>` 用於操作暫存器。

### 物理限制 (AI 的盲區)
*   **Hot Switching (熱切換)**：在有大電流/高電壓通過時切換繼電器 (Relay)，會燒毀探針卡。
    *   *風險*: AI 為了優化程式結構，可能會擅自移動 Relay 切換指令的位置。
*   **Settling Time (穩定時間)**：電壓設定後需要時間穩定。
    *   *風險*: AI 常常會覺得 `wait(5ms)` 是無用的效能損耗而建議刪除，TE 必須嚴格把關。
*   **Pattern Memory (向量記憶體)**：儲存 0/1 波形的專用記憶體，極度昂貴且有限。
    *   *SE Note*: AI 產生 Pattern 時傾向 "Unroll Loop" (展開迴圈)，這會導致 Pattern Memory 溢位 (Out of Memory)。
    *   *解法*: 必須引導 AI 使用機台支援的 "Repeat" 或 "Subroutine" 指令來壓縮 Pattern 大小。

## 3. 關鍵領域術語手冊 (Terminology)

### Flash 特有術語
*   **Datasheet (規格書)**：晶片的說明書。TE 開發時必須一邊看 PDF 一邊寫 Code。
*   **Command Sequence (指令序列)**：對 Flash 讀寫需要送一串指令（如：`00h` -> `Address` -> `30h`）。順序錯一個，晶片就沒反應。
*   **BBM (Bad Block Management)**：Flash 允許壞塊存在，程式需能識別並標記壞塊。這是邏輯最複雜、最適合 AI 寫單元測試 (TDD) 的地方。
*   **Pattern**：指的不是 Design Pattern，而是「測試向量 (Test Vector)」，即一連串預定義好的 0 與 1 的訊號波形。

### 硬體控制抽象化
*   **Pin Group**：將功能相近的 Pin (如 `DQ0`~`DQ7`) 定義成群組，方便統一操作。
*   **Level Set**：定義 Pin Group 的高電壓 (`VIH`) 與低電壓 (`VIL`)。
*   **Timing Set**：定義時脈週期 (`Period`)、訊號邊緣 (`Edge`)、採樣點 (`Strobe`)。
    *   *AI 提示*: 當 Prompt 包含這些具體術語時，AI 比較不容易產生幻覺，更容易理解任務是關於硬體控制，而不是純軟體計算。

### 測試流程與架構
*   **Test Flow**：由一連串測試項目 (Test Items) 組成的流程 (如 `Open/Short` -> `Func` -> `AC` -> `Power`)。
*   **Multi-site Testing**：為了產能，一台 Tester 同時測多顆 DUT (x8, x16)。程式碼需處理 `site_loop`。
    *   *AI 提示*：提醒 AI「這段程式碼會在 multi-site 環境下執行，請確保所有硬體操作與結果判斷都是 per-site 的」。
*   **Project Structure**：
    *   `Spec.h`: 定義規格與 Limits。
    *   `TestFlow.cpp`: 主流程。
    *   `TestFunctions/`: 獨立測試項目實作。
    *   *AI 應用*: 在進行 Code Review 或重構時，可以提供這個架構給 AI，讓它遵循專案的慣例，例如「請將這個函式的所有 Magic Number 提取到 `Spec.h` 中，並以 `LIMIT_` 作為前綴」。

### 生產與數據
*   **WS (Wafer Sort) vs FT (Final Test)**：晶圓階段測試 (WS) vs 封裝後測試 (FT)。WS 會有 Wafer Map 與 Inking (標記壞點) 需求。
*   **Char (特性分析) vs Prod (量產)**：
    *   **Char**: 收集海量數據，不在乎時間 (Shmoo Plot)。
    *   **Prod**: 分秒必爭，只求 Pass/Fail。
*   **Binning**：測試結果分類。Bin 1 為良品，其他為不同失效模式。
    *   *AI 應用*：請 AI 審查程式碼的邏輯分支，確保所有 `return` 路徑都有指派 Bin 值，防止發生 "No Bin" 錯誤導致機台 Alarm。
*   **STDF (Standard Test Data Format)**：業界標準二進位測試資料格式。
    *   *AI 應用*: 這是 AI 的絕佳應用場景。可以請 AI 寫 Python 腳本 (使用 `pystdf` 函式庫) 來解析 STDF，例如：「寫一個 Python 腳本，讀取 `data.stdf`，計算 'FUNC_TEST' 這個測試項目的良率，並畫出量測值的分布直方圖」。
*   **Guardband**：為了品質，測試條件比規格書更嚴格 (如 VDD ±5% vs ±10%)。
    *   *AI 提示*：在請 AI 寫規格檢查函式時，提醒它將上下限數值設為可調整的變數 (Variables)，而不是寫死 (Hard-code)，以便 TE 後續調整 Guardband。
*   **Correlation**：新舊程式或不同機台間的結果比對，是量產前的最後防線。
    *   *痛點*: 手動比對上千顆 DUT 的測試紀錄 (Log) 既耗時又容易出錯。
    *   *AI 應用*: 請 AI 寫 Python/Perl 腳本，自動比對兩個 STDF 或 CSV 檔案，抓出 Binning 或量測值不符的 DUT。
*   **Handler/Prober**：負責搬運晶片的機械設備，需透過 GPIB 等介面通訊。
    *   *AI 盲區*: AI 不認識 `GPIB_WRITE` 這類專有通訊指令，也無法理解其時序的重要性。這部分通常是資深工程師處理好的底層驅動。
*   **Shmoo Plot (特性圖)**
    *   *概念*：一種經典的特性分析手法。透過掃描兩個或多個參數（例如 `VCC` vs `Frequency`），並在座標圖上標示 Pass/Fail 結果，來找出晶片最穩定的工作區間。
    *   *AI 應用*：TE 經常需要寫腳本來自動執行 Shmoo 測試與收集數據。可以請 AI 生成 Python 或 Perl 腳本來控制測試、解析 Log，甚至是用 `matplotlib` 繪製 Shmoo Plot。
*   **Data Logging (數據紀錄)**
    *   *概念*：所有測試結果（Pass/Fail、量測值）都必須被記錄下來，以供後續分析。業界標準格式是 STDF，但很多時候也會用 CSV。
    *   *AI 應用*：這是 AI 的強項。可以請 AI 寫腳本來解析、統計、視覺化測試資料，幫助 TE 快速從上百萬筆數據中找出異常。

## 4. AI 導入與 SE 賦能攻略

作為 SE，你可以透過工具與觀念引導，解決 TE 的痛點並規避 AI 風險。

### ⚠️ AI 幻覺與風險控制
*   **虛構 API**：AI 可能幻想 V93000 有某個方便的 `std::vector` 支援，或混淆 Teradyne 與 Advantest 的語法。
*   **記憶體洩漏 (Memory Leak)**：測試程式通常是 24 小時不間斷運行。AI 習慣的 `new`/`malloc` 若無嚴格釋放，微小的 Leak 也會導致機台當機 (Crash)。
*   **檔案誤用**：TE 專案包含巨大的 `.pat` (Pattern) 或 `.tim` (Timing) 純文字檔。
    *   *Action*: 設定工具的 **副檔名白名單**，只允許 `.c`, `.cpp`, `.h`，避免爆 Token。

### 💡 AI 應用場景 (High Value)
1.  **Legacy Code 重構**：將 Magic Numbers 提取為 `Spec.h` 中的具名常數。
2.  **演算法驗證 (Oracle)**：Flash 測試常涉及複雜的 **ECC** 或 **Scramble** 演算法。請 AI 用 Python 寫出對應邏輯作為「對照組」，驗證 C++ 實作的正確性。
3.  **自動化文件 (Auto-Doc)**：讀取 C++ Header 檔與實作，自動生成 Markdown 格式的 API 文件，解釋每個 Test Function 的測試目的與參數意義。
4.  **數據分析**：利用 Python (`pystdf`) 解析 STDF，繪製 **Shmoo Plot**、良率分佈圖，或進行 **Correlation** 比對。
5.  **規格轉程式 (SDD)**：將 Datasheet 文字貼給 AI，生成 **Command Sequence** 程式碼。

### 🛠️ SE 工程賦能 (Best Practices)
除了 AI，SE 的核心能力也能解決 TE 的痛點：

*   **Offline Simulation (離線模擬)**
    *   *痛點*: 機台時間昂貴，TE 只能半夜排隊除錯。
    *   *解法*: 引入 **Mocking** 觀念，建立 "Mock Hardware Library" 模擬 `WriteRegister` 或 `MeasureVoltage` 的行為（例如回傳隨機值或固定值），讓 TE 能在自己的筆電上編譯並驗證邏輯，不用搶機台。
        ```cpp
        // Mocking 範例 (Header-only 簡單切換，適合老舊編譯器)
        #ifdef OFFLINE_SIMULATION
            // 模擬硬體行為 (Mock)
            void WriteRegister(int addr, int data) {
                printf("[MOCK] Write Addr: 0x%X, Data: 0x%X\n", addr, data);
            }
        #else
            // 真實機台 API
            #include "ate_driver.h"
        #endif
        ```
*   **Git 與二進位檔策略**
    *   *痛點*: TE 習慣用 "Copy Folder" 備份，因為專案包含大量二進位檔 (.pat, .stdf) 會塞爆 Git。
    *   *解法*: 協助設定 `.gitignore` 精準排除巨型檔案，或開發 Python Script 自動將二進位檔備份到 NAS，只對 Source Code 做 Git 版控。
*   **CI/CD 與靜態分析 (Linter)**
    *   *痛點*: TE 往往要等到上機台編譯 (Compile on Tester) 才知道有語法錯誤，浪費機台時間。
    *   *解法*: 幫 TE 建立簡單的 Pre-commit Hook 或 CI Pipeline。雖然無法連結 ATE Library，但可以跑 `g++ -fsyntax-only` 或 `cppcheck`，提前抓出變數未宣告、括號不對稱等低級錯誤。
*   **防禦性程式設計**
    *   *差異*: ATE 上 Exception 可能導致危險。
    *   *解法*: 推廣 **Return Code** 模式與 Error Propagation，並請 AI 協助檢查是否所有硬體操作函式都有判斷回傳值，確保硬體狀態能被 Reset。

## 5. 總結

> 「AI 是一個精通 C 語言、但完全不懂硬體時序 (Timing) 的實習生。」

在協助 TE 導入 AI 或開發工具時，請始終記得：**邏輯正確不代表硬體行為正確**。我們的目標是利用軟體技術 (AI, Mocking, DevOps) 來加速開發，但最終的硬體把關仍需依賴 TE 的專業知識。
