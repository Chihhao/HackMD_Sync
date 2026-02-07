# Coding Mode Demo Script: Mini Wafer Map Viewer (Interactive SDD Workflow)

這個 Demo 展示如何利用 **SDD (Spec Driven Development)** 流程，並結合 **反向提問 (Reverse Questioning)** 技巧，讓 AI 引導使用者釐清需求，最後才生成程式碼。

## 準備工作
1.  建立一個空資料夾 `wafer-demo`。
2.  在裡面建立三個空檔案：
    *   `index.html`
    *   `style.css`
    *   `script.js`
3.  開啟 AI 小旺 Coding Mode，勾選上述三個檔案。

---

## Round 1: 需求訪談 (Requirement Interview)
**目標**：展示「反向提問」技巧。當需求模糊時，讓 AI 主動提問，而不是使用者絞盡腦汁想細節。

**Prompt**:
> 我想要開發一個 **「Mini Wafer Map Viewer」** 的網頁小工具。
> 請你扮演資深軟體架構師，針對這個模糊的需求，**向我提出 3 個關鍵問題**，以釐清功能細節。
> 請一次問一個問題，不要一次列出所有問題。

*(操作建議：AI 會先問第一個問題。為了節省時間，我們在 Round 2 會直接回答所有潛在問題，強制進入規格生成階段。)*

---

## Round 2: 生成規格 (Spec Generation)
**目標**：回答 AI 的問題，並要求產出規格書。這展示了 Context 的連續性與 AI 的歸納能力。

**Prompt**:
> 這裡回答你的問題：
> 1. **大小**：30x30 的矩陣。wafer是圓的，最左上角的座標是(11,11)
> 2. **數據**：隨機生成模擬數據 (0=Pass, 1=Fail)。
> 3. **互動**：滑鼠移上去要顯示座標 (X, Y)。
>
> 請根據以上資訊，幫我撰寫一份 **技術規格書 (Technical Spec)**，包含檔案結構與邏輯描述。

---

## Round 3: 規格修正 (Spec Iteration)
**目標**：展示在寫 Code 之前，先在規格層面進行迭代是成本最低的。

**Prompt**:
> 規格看起來不錯，但我漏了一個需求。
> 請在規格中加入：**「下方需顯示即時良率 (Yield %)」**。
> 請更新規格書。

---

## Round 4: 根據規格實作 (Implementation)
**目標**：讓 AI 根據剛才確認的規格，一次性生成所有程式碼。

**Prompt**:
> 規格確認沒問題。
> 請根據這份最新的規格，幫我實作 `index.html`, `style.css`, `script.js` 的完整程式碼。

*(操作建議：AI 生成後，使用 Apply Patch 或複製貼上，開啟 index.html 展示成果。此時應該會看到一個基本的紅綠方塊陣列。)*

---

## Round 5: 迭代與重構 (Refinement)
**目標**：展示 AI 理解現有代碼並進行修改的能力 (Coding Mode 的強項)。

**Prompt**:
> 這個介面有點太亮了，工程師喜歡深色模式。
>
> 請幫我修改 `style.css` 和 `script.js`：
> 1.  **UI 風格**：改為 **Dark Mode** (深灰背景、圓形晶粒、霓虹光暈效果)。
> 2.  **功能增強**：點擊任何一個 "Fail" (紅色) 的晶粒，它會變成 "Pass" (綠色)，並自動重新計算下方的 Yield 數值 (模擬手動修補)。

*(操作建議：展示 Apply Patch 的 Diff 視圖，強調 AI 只修改了必要的 CSS 和 JS。重新整理頁面，測試點擊修補功能，視覺效果會非常強烈。)*
