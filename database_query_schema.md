# 案例資料庫檢索對照表 (Database Query Schema & Intent Mapping)

本文件建立了**使用者設計問題類型**與**資料庫項目 (Ontology & Userend Framework)** 之間的映射關係。當使用者在網頁端或AI對話端提出特定的設計問題時，系統將依據此對照表，精確調用資料庫中的特定欄位（包含第一階段本體抽取與第二階段框架分析）進行回答。

---

## 檢索前置步驟與尺度/環境評估原則 (Pre-retrieval Metadata & Scale/Context Verification Principles)

> [!IMPORTANT]
> **在依據下表對照矩陣進行設計問題歸類、檢索與回答前，AI 必須遵循以下「基本資料評估與約束原則」，以避免忽略案例的原始尺度與環境限制而給出不合理的設計建議：**
>
> 1. **首要步驟：提取與確認案例基本資料 (Extract Baseline Metadata First)**
>    在引用或檢索任何案例進行回答時，AI 必須首先讀取並明示該案例在 `objective_facts` 中的三大基本屬性：
>    *   **地理位置 (Location)**：確認其所屬大洲、國家、城市及城市特質（如高密度都市、山城、港口等）。
>    *   **氣候特徵 (Climate)**：確認其氣候類型（如溫帶、熱帶、寒帶等）。
>    *   **建築規模/面積 (Scale)**：確認其總建築面積範圍（如 0-300m²、2000-10000m² 等）以及量體特徵。
>
> 2. **進行尺度與環境適應性對比 (Scale & Context Adaptation Evaluation)**
>    在提出任何設計建議前，AI 必須主動評估引用案例與使用者設計目標場景之間的尺度及環境限制差異：
>    *   **尺度限制 (Scale Constraints)**：例如，小型住宅（0-300m²）的平面配置、空間劃分、構造與皮層手段，不能無縫直接套用到大型公共建築或高密度商業體（>10000m²）上；反之，巨大跨度的結構系統與大規模量體佈局，也不應硬套給微型建築。
>    *   **氣候與地理限制 (Climate & Geography Constraints)**：寒帶或溫帶地區的封閉式大保溫皮層、雙層呼吸表皮，在套用到熱帶或亞熱帶地區時，必須進行適應性防熱防潮修改；山地高差地形的應對策略不應直接硬塞給平地基地。
>    *   **環境與都市密度限制 (Urban Density Constraints)**：在開闊田園或低密度自然景觀中的配置策略，在套用到高密度都市狹窄街廓時，必須考慮建蔽率、法定退縮邊界及防火間隔限制。
>
> 3. **回答生成與輸出規範 (Response Generation Rules)**
>    *   在基於檢索到的案例生成具體設計手法或配置建議時，**必須先包含一段「案例基礎背景與適用性評估」**（明確說明該案例的地理位置、氣候與面積尺度）。
>    *   AI 必須向使用者具體指出：該案例由於具備特定的 [尺度/氣候/地形] 物理限制，當前的設計建議在套用到使用者的情境時，應如何進行合理的「尺度縮放」或「設計調整」（例如：調整開窗比、增加通風遮陽、簡化柱網模矩等），嚴禁不顧尺度的直接抄襲或給出不合理的建議。

---

## 問題類型與資料庫項目對照矩陣 (Query Matrix)

| 圖面類型 (Drawing Type) | 位置/部位 (Location/Part) | 建築特徵 (Architectural Feature) | 第一階段本體抽取對應項目 (Stage 1 Nodes) | 第二階段 Userend 對應項目 (Stage 2 Fields) | 檢索與回答聚焦重點 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **A. 平面圖類**<br>(Plans: Site & Floor Plans) | **皮 (Envelope)** | **A-1. 皮層與立面設計** | `envelope.facade`, `envelope.opening` (door, window), `envelope.grille/screen` | 9. 元素關係 (外牆與室內) | 外牆厚度、平面開窗開口與格柵/百葉的平面位置。 |
| | **外 (Exterior)** | **A-2. 基地配置與建蔽率關係** | `site.context`, `site.access`, `site.zoning` (setback, BCR/FAR), `building footprint`, `building orientation` | 1. Location (城市特質), 7. 議題展開 (法規限制), 8. 概念閱讀 (配置策略), 9. 元素關係 (主體與基地) | 基地紅線與退縮限制（建蔽率邊界）、建築配置朝向、聯外交通與人車出入口配置。 |
| | | **A-3. 景觀與植被鋪裝關係** | `site.landscape` (hardscape/softscape), `site.vegetation` | 8. 概念閱讀 (景觀配置), 9. 元素關係 (主體與環境) | 室外綠化景觀、植被花園配置、室外步道硬質鋪裝配置。 |
| | | **A-4. 半室外與過渡空間** | `space.semi-outdoor space` (Transition/buffer, pilotis, balcony) | 8. 概念閱讀 (過渡策略), 9. 元素關係 (室內外界面) | 陽台、露台、底層架空（Pilotis）、騎樓及半室外緩衝空間的配置與內外交界處理。 |
| | **內 (Interior)** | **A-5. 空間配置與動線** | `program.functional zone`, `space` (rooms, public/private, corridors, stairs, vertical core, void, courtyard, lobby), `interior.partition`, `Spatial Organization` | 4. program (public/private), 8. 概念閱讀 (平面組織), 9. 元素關係 (室內空間) | 房間佈局、機能分區邊界、水平動線與核心筒配置、天井與平面虛空間。 |
| | | **A-6. 結構與構造系統** | `structure.column`, `structure.structural system` | 5. Structural System, 9. 元素關係 (柱與隔間關係) | 結構柱網格線（柱距與平面對齊）、承重牆平面線。 |
| | | **A-7. 量體與幾何手段** | `form.geometry` (grid/modular), `form.articulation` | 10. 幾何手段 (平面幾何) | 平面輪廓幾何形態（如模矩網格、斜切角幾何）。 |
| | | **A-8. 材料選用與氛圍** | `material` (textures, colors) | 10. 幾何手段 (色彩與材料策略) | 室內地坪材料分割（如平行帶狀鋪貼、材質肌理對比）。 |
| **B. 剖面圖類**<br>(Sections) | **皮 (Envelope)** | **B-1. 皮層與立面設計** | `envelope.facade system`, `envelope.shading device`, `floor / roof / wall` (roof), `envelope.facade` | 9. 元素關係 (垂直皮層) | 外牆垂直斷面、遮陽板垂直悬挑、屋頂排水坡度與斷面。 |
| | **外 (Exterior)** | **B-2. 基地應對與地形** | `site.topography`, `hydrology / water table` | 3. Topography | 剖面地形高差起伏、地下室與原地形及水位面的斷面關係。 |
| | **內 (Interior)** | **B-3. 空間配置與動線** | `space.void`, `space.double height space`, `space.atrium`, `Spatial Organization` (vertical stacking) | 8. 概念閱讀 (剖面分層), 9. 元素關係 (垂直空間) | 樓層垂直分層疊加、垂直動線（斜坡/樓梯剖面）、垂直挑空中庭。 |
| | | **B-4. 結構與構造系統** | `structure` (beam, slab, truss, structural system) | 5. Structural System (結構剖面) | 梁柱框架斷面、樓板厚度與跨度、桁架結構系統。 |
| | | **B-5. 量體與幾何手段** | `form.geometry`, `form.articulation` | 10. 幾何手段 (剖面幾何) | 剖面空間比例、幾何凹凸處理。 |
| | | **B-6. 材料選用與氛圍** | `lighting` (daylighting/light guidance), `material` | 10. 幾何手段 (垂直採光與氛圍) | 垂直導光路徑、高側窗採光、剖面材料反射對垂直光影氣氛的塑造。 |
| **C. 立面圖類**<br>(Elevations) | **皮 (Envelope)** | **C-1. 皮層與立面設計** | `envelope.facade`, `envelope.fenestration`, `envelope.grille/screen`, `material.cladding` | 9. 元素關係 (皮層與外部) | 立面材料分割線、開窗比例與孔洞率、格柵屏蔽與遮陽百葉的面域圖案。 |
| | **外 (Exterior)** | **C-2. 基地應對與周邊** | `site.context` (urban, historical context), `views` | 1. Location, 9. 元素關係 (主體與周邊立面) | 建築立面與周圍古蹟、街道建物、林木天際線的立面銜接關係。 |
| | **內 (Interior)** | **C-3. 結構與構造系統** | `structure.exoskeleton / outer frame` | 5. Structural System (立面結構) | 外露結構骨架、立面可見的結構梁柱網格與模矩。 |
| | | **C-4. 量體與幾何手段** | `form.massing` (tower, podium, freestanding), `form.geometry` | 6. Scale, 10. 幾何手段 (立面幾何) | 裙房與塔樓高度比例、量體虛實對比、立面幾何形體鉸接。 |
| | | **C-5. 材料選用與氛圍** | `material` (concrete, steel, wood, glass, brick, stone, colors) | 10. 幾何手段 (色彩與材料質感) | 外部建材的色彩搭配、材質紋理、反光度，以及陽光照射下的陰影層次與視覺氣氛。 |
| **D. 等軸/詳圖類**<br>(Axonometric / Details) | **皮 (Envelope)** | **D-1. 皮層與立面設計** | `envelope.opening` (window/door frames), `material.cladding` | 5. Structural System (詳圖構造), 9. 元素關係 (皮層接頭) | 幕牆錨固詳圖、窗框與牆體斷面交接防水、外牆覆面板掛件詳圖。 |
| | **外 (Exterior)** | **D-2. 基地應對與配置** | `structure.foundation`, `site.infrastructure` | 5. Structural System (基礎收邊) | 地下基礎防水、防潮散水、室外排水溝與基礎結構交接詳圖。 |
| | **內 (Interior)** | **D-3. 空間配置與動線** | `interior.partition`, `interior.furniture` | 9. 元素關係 (細部收邊) | 移動隔間軌道細部、固定家具安裝、門窗扇滑軌詳圖。 |
| | | **D-4. 結構與構造系統** | `structure` (joints, steel connection detail) | 5. Structural System (鋼構/RC接頭) | 梁柱鋼結構接頭焊縫、預鑄混凝土接縫詳圖。 |
| | | **D-5. 材料選用與氛圍** | `interior.finishes`, `material.texture` | 10. 幾何手段 (材料物理感官) | 異質材料交接收邊（如木與鐵）、面材鋪貼與收口伸縮縫。 |
| **E. 非圖面/價值觀**<br>(Non-Drawing / Values) | **非圖面**<br>(Non-Drawing) | **E-1. 社會環境與設計價值觀** | `issue.sustainability`, `issue.building performance`, `issue.cost`, `issue.building code`, `issue.urban impact`, `issue.cultural significance`, `issue.heritage preservation`, `event.project phase`, `event.Project Timeline` | 2. Climate, 7. 議題展開 (Issue Expansion), 11. 延伸案例 (Extended Cases) | 永續指標、造價預算與時程管理、歷史建物保存態度、法規突破等隱性設計觀。 |

---
*最後更新時間：2026-06-10*
