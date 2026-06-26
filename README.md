# 建築案例設計輔助工具 (AI-assisted Architectural Design Inspiration System)

本專案是一個基於當代比利時先鋒建築學概念與 **Google Gemini API** 的設計重構與引導式 AI 對話系統。

本工具旨在打破傳統台灣建築案例分析過度偏向感性、詩意描述的局限，將其轉化為客觀物理事實與結構化的**「目標-策略 (Goal-Strategy)」**幾何特徵，並透過 AI 代理人輔助設計師進行案例比對與方案重構。

---

## 🔍 專案核心動機與研究背景

1. **台灣與比利時的語境對照**：
   台灣的建築教育與實務分析常充斥著感性修辭（例如「與風對話的牆」、「光影的詩意」），使得學習者難以捉摸背後具體的幾何與物理構造因果。反觀比利時的先鋒事務所（如 OFFICE KGDVS, XDGA, AJDVIV 等），常以非常簡單的幾何、暴露的結構以及廉價工業面材，直接回應基地的物理和法規挑戰。
2. **AI 能否學習建築知識**：
   我們思考：這種清晰的幾何構成與構造因果關係，是否是 AI 能學習並轉譯的建築知識？因此，我們透過定義建築本體字典，萃取出 20 個經典案例的結構化知識庫，讓 AI 能夠以「比利時的批判性凝視」審視並修改我們的設計。

---

## 🛠️ 核心檔案結構與定位

本專案將主客觀資料與邏輯元件進行高度模組化配置：

### 1. `/data/` 資料與規範目錄
* 📖 **[ontology.json](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/data/ontology.json)** *(「建築學概念語意字典」)*：定義了 5 大頂層範疇（`building`, `site`, `participant`, `issue`, `event`）以及子範疇之階層關係，指引 AI 如何將非結構性的文本句段拆解並對齊統一的語意節點。
* 🛡️ **[schema.json](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/data/schema.json)** *(「資料庫格式驗證器」)*：相較於語意字典，此檔案專用於驗證與約束物理事實 facts（如氣候區、規模面積、主結構系統）的實體 JSON 數據格式，防範 AI 的欄位格式幻覺。
* 📝 **[framework.md](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/data/framework.md)**：包含雙階段特徵萃取工作流、拒絕幻覺機制（允許空值 `null`）、以及主觀 **Goal-Strategy** 概念重組分流的技術設計規範。
* 📊 **[cases_database.json](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/data/cases_database.json)**：已建構完成的 20 個比利時當代落成案例結構化主資料庫。
* 🌐 **[ontology_visualizer.html](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/data/ontology_visualizer.html)**：用於視覺化展示本體階層關係的互動式網頁工具。

### 2. `/src/` 前端互動應用目錄
* 💻 **[index.html](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/index.html)**：工具的前端對話介面主頁。
* 🎨 **[style.css](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/src/style.css)**：現代暗色調玻璃摩登風格的 UI 樣式表。
* 🧠 **[app.js](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/src/app.js)** & **[ai-helper.js](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/src/ai-helper.js)**：負責對接 Gemini API，並在前端實作「對話引導協議 (Interactive Chat Protocol)」的核心邏輯。
* ⚙️ **[system_prompt_md.js](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/src/system_prompt_md.js)**：定義後端 AI 進行 Triage 分流判定與設計建議重組的 System Prompt。

### 3. `/reports/` 期末報告目錄
* 📃 **[final_report.md](file:///C:/Users/StanLee/Documents/antigravity/keen-hypatia/reports/final_report.md)**：本專案的期末完整開發報告，記錄完整的開發脈絡與實作細節。

---

## 🔀 系統運作架構 (System Architecture)

當使用者丟入自己的設計提案時，系統會依循以下工作流進行引導、校驗、分流並產生設計重構建議：

```mermaid
graph TD
    A[使用者輸入：案例名稱與初步構想] --> B[對話引導協議：系統要求選擇五大情境]
    
    B -->|情境多選選擇| C[第一階段：特徵預處理與對齊]
    C --> D{Schema 驗證與 Ontology 字典對照}
    
    D --> E[第二階段：概念性分流判定 Triage]
    
    E -->|分支 A: 具備概念可比性| F1[比利時先例投影重構建議]
    E -->|分支 B: 物理與尺度全面脫鉤| F2[比利時批判性凝視與通用幾何優化]
    
    F1 --> G1[氣候與尺度的哲學轉譯]
    F1 --> G2[【目標-策略 (Goal-Strategy)】重組對比表]
    
    F2 --> H1[當代比利時「反裝飾、暴露構造」批判]
    F2 --> H2[通用空間幾何與動線優化建議]
    
    G1 & G2 & H1 & H2 --> I[產出最終設計建議報告]
```

### 💬 對話引導協議五大情境
1. **情境 1**：圖面與局部細部研究 (Drawings & Details)
2. **情境 2**：空間配置與幾何秩序 (Spatial Order & Geometry)
3. **情境 3**：結構系統與材料物質性 (Structure & Materiality)
4. **情境 4**：都市法規與社會課題 (Regulations & Social Issues)
5. **情境 5**：基地紋理與氣候環境適應 (Site & Climatic Adaptation)

---

## ⚙️ 核心開發規則與機制 (Constraints & Guardrails)

* 🚫 **無幻覺防禦機制**：在第一階段抽取時，若原始文獻中未提及某項本體節點（例如 zoning 退縮限制），該欄位一律標記為 `null` (允許空白)，禁止 AI 憑空捏造資訊。
* 🧱 **雙軌設定定位**：
  * **`ontology.json` (語意字典)**：解決詞彙對齊問題，教導 AI 如何從複雜段落中識別建築學專用實體。
  * **`schema.json` (格式驗證器)**：解決格式結構問題，使用 JSON Schema 強制約束數值格式。
* 🌪️ **氣候哲學轉譯**：當比利時溫帶被動式手法（如雙層溫室表皮）投影至亞熱帶時，AI 會自動將其轉譯為輕質遮陽沖孔網與煙囪效應排熱挑空，而非生硬複製。

---

## 🚀 本地快速開始與運行步驟

### 1. 克隆專案與準備
```bash
# 複製本專案儲存庫
git clone <your-repository-url>
cd keen-hypatia
```

### 2. 本地伺服器啟動
由於前端腳本包含模組化的 ES Module 導入（`import`/`export`），建議您透過本地 Web Server 啟動專案，避免發生瀏覽器的 CORS 攔截：
* **方法 A (推薦)**：使用 VS Code，安裝外掛程式 `Live Server`，並在專案根目錄按右鍵選擇 `Open with Live Server`。
* **方法 B (Python)**：
  ```bash
  python -m http.server 8000
  ```
  在瀏覽器開啟 [http://localhost:8000](http://localhost:8000)。
* **方法 C (直接雙擊)**：在部分解除網頁 CORS 限制的環境下，亦可直接雙擊 `index.html` 進行訪問。

### 3. 配置 API 金鑰
開啟網頁後，請在介面頂部的金鑰配置欄位輸入您的 **Google Gemini API Key**，即可開始與 AI 進行互動與設計對話。
