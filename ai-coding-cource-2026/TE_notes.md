# 給 SE 講師的 TE 領域知識補給包

這份筆記整理了軟體工程師 (SE) 在教授測試工程師 (TE) AI 輔助開發課程時，需要了解的關鍵領域知識與術語，協助講師建立共感並提供更精準的範例。

## 1. 開發環境與運行目標的差異

*   **SE 的世界**：程式跑在 Server、Cloud 或手機上。資源（記憶體、CPU）通常相對充裕，OS 會幫你管理很多事。
*   **TE 的世界 (ATE)**：
    *   **ATE (Automatic Test Equipment)**：程式是跑在像冰箱一樣大的「機台」上（如 Advantest V93000, Teradyne J750）。
    *   **DUT (Device Under Test)**：被測試的晶片。
    *   **核心差異**：TE 寫的 C/C++ 程式是用來控制機台的「針腳 (Pin)」，去對晶片送電訊號。
    *   **AI 教學重點**：提醒他們 AI 訓練資料多來自標準軟體開發，所以 AI 給出的 Code 常常會用標準庫（如 `std::vector`, `iostream`），但在 ATE 的老舊編譯器或特殊環境下可能**不支援**或**效率太差**。

## 2. 程式語言特性的「眉角」

TE 的程式碼通常有以下特徵，這是教 Prompt Engineering 時可以強調的：

*   **C/C++ 混合體**：很多測試程式是 20 年前寫的 C code，後來混用了 C++。
*   **Magic Numbers (魔術數字)**：
    *   *現象*：程式碼充滿了 `WriteRegister(0x40, 0x12);`。
    *   *痛點*：沒人知道 `0x40` 是什麼，只有查 Datasheet 才知道。
    *   *AI 應用*：這是 AI 最強的地方。教他們用 AI 把「魔術數字」重構為「具名常數 (Enum/Define)」（如課程大綱提到的 Legacy Refactor）。
*   **Bitwise Operation (位元運算)**：
    *   *現象*：大量的 `&`, `|`, `<<`, `>>`。因為硬體暫存器（Register）的每一個 Bit 都有意義。
    *   *AI 應用*：AI 非常擅長解釋複雜的位元運算邏輯，可以請 AI 幫忙寫註解。

## 3. Flash 測試專有名詞 (NAND/NOR)

針對 Flash TE 受眾的關鍵詞彙：

*   **Datasheet (規格書)**：晶片的說明書。TE 開發時必須一邊看 PDF 一邊寫 Code。
    *   *AI 應用*：教他們把 Datasheet 的文字複製給 AI，讓 AI 根據規格寫 Code（SDD 模式）。
*   **Command Sequence (指令序列)**：對 Flash 讀寫不是呼叫一個 API 就好，通常要送一串指令（例如：先送 00h，再送 Address，再送 30h）。
    *   *風險*：順序錯一個，晶片就沒反應。
*   **BBM (Bad Block Management, 壞塊管理)**：
    *   *概念*：Flash 晶片出廠時允許有壞掉的區塊。TE 的程式必須能識別並標記這些壞塊，不能讓使用者存資料進去。
    *   *AI 應用*：這是邏輯最複雜、最容易出 Bug 的地方，適合用 TDD（測試驅動開發）來讓 AI 寫單元測試。
*   **Pattern**：指的不是 Design Pattern，而是「測試向量 (Test Vector)」，即一連串預定義好的 0 與 1 的訊號波形。

## 4. 他們最在乎的 KPI (績效指標)

了解他們的 KPI，才能告訴他們 AI 如何幫他們「省事」：

*   **TTR (Test Time Reduction, 測試時間縮減)**：
    *   測試一顆晶片多花 0.1 秒，量產幾百萬顆時就是巨大的成本。
    *   *AI 提示*：在請 AI 優化程式碼時，Prompt 必須包含「請考慮執行效率」、「減少迴圈內的冗餘計算」。
*   **Yield (良率)**：
    *   程式寫錯可能導致把好 IC 判成壞的 (Overkill)，或把壞 IC 判成好的 (Underkill)。
    *   *AI 提示*：強調 AI 的 Code Review 功能，幫忙檢查邊界條件（Boundary Check），避免誤判。

## 5. 針對課程講義的具體建議

*   **在講「AI 幻覺」時**：
    *   *SE 觀點*：AI 會捏造不存在的 Python Library。
    *   *TE 轉譯*：AI 可能會捏造不存在的機台 API（例如幻想 V93000 有某個方便的函式，但其實沒有），或者引用了錯誤的 C++ 標準（ATE 編譯器可能還停在 C++98）。

*   **在講「系統提示詞」時**：
    *   補充說明：為什麼要檢查 **Memory Leak**？因為測試程式通常是 24 小時不間斷運行，一點點 Leak 跑幾天後就會讓機台當機 (Crash)，導致產線停擺。

*   **在講「Coding 模式」時**：
    *   強調 **副檔名白名單**。TE 的專案裡可能會有 `.pat` (Pattern file) 或 `.tim` (Timing file)，這些通常很大且是純文字，如果不小心餵給 AI 會瞬間爆 Token。教他們要小心勾選 `.c`, `.cpp`, `.h` 即可。

## 6. 總結：給 TE 的一句話

> 「我知道大家的程式碼是跑在昂貴的 Tester 上，而不是寬容的 Linux Server 上。AI 寫的 Code 雖然邏輯通常是對的，但它不懂硬體 Timing 和機台限制。所以，**請把 AI 當作一個懂 C 語言但不懂硬體的實習生**，你們的任務是審查它的 Code，而不是盲目複製貼上。」
