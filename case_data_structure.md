# 案例資料庫單一案例資料結構 (Single Precedent Data Structure)

本文件詳細說明先例資料庫（如 `cases_database.json` 與各先例的 `.json` 檔案）中，單一建築案例的 JSON 資料結構與欄位定義。

---

## 一、 最外層結構 (Top-Level Structure)

每個案例皆為一個 JSON 物件，包含以下基本欄位：

```json
{
  "project_id": "案例標識符 (String, Slug 格式)",
  "project_name": "案例中文/英文名稱 (String)",
  "architect": "設計單位/建築師 (String)",
  "year": "建造/完工年份 (String)",
  "url": "案例官方/介紹網址 (String)",
  "objective_facts": {},
  "subjective_analysis": {},
  "ontology_extraction": {}
}
```

---

## 二、 客觀事實結構 (Objective Facts)

`objective_facts` 包含案例的物理屬性與基本背景資料，固定為以下 6 個欄位：

| 欄位名稱 (Key) | 資料型態 (Type) | 欄位定義與說明 |
| :--- | :--- | :--- |
| **`1. Location`** | Object | 包含以下屬性：<br>• `continent`: 洲別 (如 `"歐洲 (Europe)"`) <br>• `country`: 國家 (如 `"比利時 (Belgium)"`) <br>• `city`: 城市 (如 `"布魯塞爾 (Brussels)"`) <br>• `city_character`: 城市特質與基地周邊環境背景說明 |
| **`2. Climate`** | String | 氣候類型 (如 `"溫帶"`, `"亞熱帶"`)，通常會附帶具體氣候說明（例如：`"溫帶 (溫帶海洋性氣候 - Cfb，由地理位置合理推斷)"`） |
| **`3. Topography`** | String | 地形特徵，如 `"平地"`, `"丘陵"`, `"山地"`, `"河邊"`, `"海邊"` |
| **`4. Building type`** | String | 建築介入類型，如 `"New construct"` (新建), `"Revise"` (改建), `"Expansion"` (增建) |
| **`5. Structural System`** | String | 結構系統，如 `"RC"`, `"Steel"`, `"Wood"`, `"Other"` |
| **`6. Scale`** | String | 建築面積規模，如 `"0-300m2"`, `"300-2000 m2"`, `"2000-10000 m2"`, `"10000 up"` |

---

## 三、 主觀分析結構 (Subjective Analysis)

`subjective_analysis` 包含基於當代比利時建築設計哲學對案例進行的二維重構分析，包含以下 5 個欄位：

### 1. `7. 議題展開 (Issue Expansion)`
*   **型態**：Object
*   **內容**：
    *   `theme` (String): 說明案例的核心設計挑戰與議題詮釋。
    *   `tags` (Array of Strings): 用於檢索與分類的設計標籤。

### 2. `8. 概念閱讀 (Concept Reading)`
*   **型態**：Object
*   **內容**：
    *   `concept_one_sentence` (String): 以一句話精煉總結案例的空間概念。
    *   `goal` (String): 建築師預期達到的空間目標。
    *   `strategy` (String): 包含 `【有形策略】` 與 `【無形策略】` 的具體空間手法說明。

### 3. `9. 元素關係 (Element Relationships)`
*   **型態**：Object
*   **內容**：
    *   `main_elements` (Array of Strings): 案例中的主要實體與環境/無形元素。
    *   `main_element_relationships` (Array of Objects): 元素間的對話與互動關係。每個物件包含：
        *   `elements` (Array of Strings): 參與此關係的元素。
        *   `goal` (String): 該關係欲達到的目標。
        *   `strategy` (String): 達此目標的設計策略。
    *   `interior_element_relationships` (Object/String): 室內元素關係（若無可留白或以說明字串呈現）。若為物件，格式同 main 關係：
        *   `elements` (Array of Strings)
        *   `goal` (String)
        *   `strategy` (String)
    *   `geometric_forms_of_elements` (String): 說明各個元素的幾何型態特徵。

### 4. `10. 幾何手段 (Geometric Tactics)`
*   **型態**：Object
*   **內容**：
    *   `goal` (String): 幾何操作所帶來的五官感受與空間氣氛影響。
    *   `strategy` (String): 技術、材料與幾何操作的具體策略。

### 5. `11. 延伸案例 (Extended Cases)`
*   **型態**：Array of Objects
*   **內容**：列出與本案相關聯的延伸對照案例，每個案例物件包含：
    *   `project_name` (String): 延伸案例名稱。
    *   `architect` (String): 延伸案例設計單位。
    *   `relation_type` (String): 關聯類型 (如 `"相似"`, `"對比"`)。
    *   `issue_expansion` (Object): 包含 `goal` 與 `strategy` 的議題說明。
    *   `concept_reading` (Object): 包含 `concept_one_sentence` 與 `strategy` 的概念。

---

## 四、 本體抽取結構 (Ontology Extraction)

`ontology_extraction` 是 Stage 1 從文本中直接抽取出的特徵字典，其結構與 `ontology.json` 對應，區分為五大範疇。

每個抽取出的細項屬性若有資料，皆會被包裝成 `{ "value": "...", "example_sentence": "..." }` 物件。若無則為 `null`。

```json
{
  "building": {
    "program": { "value": "...", "example_sentence": "..." } 或 null,
    "form": { "value": "...", "example_sentence": "..." } 或 null,
    "material": { "value": "...", "example_sentence": "..." } 或 null,
    "structure": { "value": "...", "example_sentence": "..." } 或 null,
    "space": { "value": "...", "example_sentence": "..." } 或 null,
    "envelope": { "value": "...", "example_sentence": "..." } 或 null,
    "floor_roof_wall": { "value": "...", "example_sentence": "..." } 或 null,
    "building_type": { "value": "...", "example_sentence": "..." } 或 null,
    "design_concept": { "value": "...", "example_sentence": "..." } 或 null,
    "building_system": { "value": "...", "example_sentence": "..." } 或 null,
    "interior": { "value": "...", "example_sentence": "..." } 或 null,
    "other_attributes": { "value": "...", "example_sentence": "..." } 或 null
  },
  "site": {
    "context": { "value": "...", "example_sentence": "..." } 或 null,
    "topography": { "value": "...", "example_sentence": "..." } 或 null,
    "climate": { "value": "...", "example_sentence": "..." } 或 null,
    "hydrology": { "value": "...", "example_sentence": "..." } 或 null,
    "soil": { "value": "...", "example_sentence": "..." } 或 null,
    "vegetation": { "value": "...", "example_sentence": "..." } 或 null,
    "access": { "value": "...", "example_sentence": "..." } 或 null,
    "landscape": { "value": "...", "example_sentence": "..." } 或 null,
    "zoning": { "value": "...", "example_sentence": "..." } 或 null,
    "other_site_attributes": { "value": "...", "example_sentence": "..." } 或 null
  },
  "participant": {
    "architect": { "value": "...", "example_sentence": "..." } 或 null,
    "client": { "value": "...", "example_sentence": "..." } 或 null,
    "engineer": { "value": "...", "example_sentence": "..." } 或 null,
    "user": { "value": "...", "example_sentence": "..." } 或 null,
    "contractor": { "value": "...", "example_sentence": "..." } 或 null,
    "developer": { "value": "...", "example_sentence": "..." } 或 null,
    "consultant": { "value": "...", "example_sentence": "..." } 或 null,
    "community": { "value": "...", "example_sentence": "..." } 或 null,
    "stakeholder": { "value": "...", "example_sentence": "..." } 或 null,
    "project_manager": { "value": "...", "example_sentence": "..." } 或 null,
    "regulatory_body": { "value": "...", "example_sentence": "..." } 或 null,
    "urban_planner": { "value": "...", "example_sentence": "..." } 或 null
  },
  "issue": {
    "sustainability": { "value": "...", "example_sentence": "..." } 或 null,
    "cost": { "value": "...", "example_sentence": "..." } 或 null,
    "functionality": { "value": "...", "example_sentence": "..." } 或 null,
    "building_performance": { "value": "...", "example_sentence": "..." } 或 null,
    "accessibility": { "value": "...", "example_sentence": "..." } 或 null,
    "constructability": { "value": "...", "example_sentence": "..." } 或 null,
    "safety": { "value": "...", "example_sentence": "..." } 或 null,
    "aesthetics": { "value": "...", "example_sentence": "..." } 或 null,
    "urban_impact": { "value": "...", "example_sentence": "..." } 或 null,
    "adaptability": { "value": "...", "example_sentence": "..." } 或 null,
    "building_code": { "value": "...", "example_sentence": "..." } 或 null,
    "user_experience": { "value": "...", "example_sentence": "..." } 或 null,
    "resilience": { "value": "...", "example_sentence": "..." } 或 null,
    "cultural_significance": { "value": "...", "example_sentence": "..." } 或 null,
    "maintenance": { "value": "...", "example_sentence": "..." } 或 null,
    "affordability": { "value": "...", "example_sentence": "..." } 或 null,
    "heritage_preservation": { "value": "...", "example_sentence": "..." } 或 null,
    "social_equity": { "value": "...", "example_sentence": "..." } 或 null,
    "urban_regeneration": { "value": "...", "example_sentence": "..." } 或 null,
    "public_health": { "value": "...", "example_sentence": "..." } 或 null
  },
  "event": {
    "site_analysis": { "value": "...", "example_sentence": "..." } 或 null,
    "feasibility_study": { "value": "...", "example_sentence": "..." } 或 null,
    "competition": { "value": "...", "example_sentence": "..." } 或 null,
    "design_review": { "value": "...", "example_sentence": "..." } 或 null,
    "groundbreaking": { "value": "...", "example_sentence": "..." } 或 null,
    "construction": { "value": "...", "example_sentence": "..." } 或 null,
    "completion_date": { "value": "...", "example_sentence": "..." } 或 null,
    "operation": { "value": "...", "example_sentence": "..." } 或 null,
    "post_construction": { "value": "...", "example_sentence": "..." } 或 null,
    "post_occupancy_evaluation": { "value": "...", "example_sentence": "..." } 或 null,
    "renovation": { "value": "...", "example_sentence": "..." } 或 null,
    "demolition": { "value": "...", "example_sentence": "..." } 或 null,
    "building_lifecycle": { "value": "...", "example_sentence": "..." } 或 null,
    "project_timeline": { "value": "...", "example_sentence": "..." } 或 null,
    "project_funding": { "value": "...", "example_sentence": "..." } 或 null,
    "public_engagement": { "value": "...", "example_sentence": "..." } 或 null
  }
}
```

*註：在實際的先例 JSON 中，若某項分類（例如 `building`）下有更細顆粒度的自定義欄位（如在 `site.context` 底下劃分了 `urban_context` 與 `historical_context`），結構亦會沿用此遞迴包裝。*
