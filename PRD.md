# 智慧供應鏈金融平台 — V2 Framework

> 期中 checkpoint 修訂版。針對老師 comment（**缺 Roadmap / what department / rollout plan**）並全面注入課程三大主軸：**Conway's Law（Week 3）/ GDS 模式（Week 2）/ Schutz Framework + 人系統 vs 工程系統（Week 9）**。

---

## I. 策略範疇與 MVP 定義 (Strategic Scoping & MVP Definition)

### 1.1 宏觀願景 (Grand Challenge)
將傳統中小企業（SME）貸款模式轉型為「智慧供應鏈金融平台」。導入計量金融與替代數據，克服中小企業融資的兩大瓶頸：**極高的徵信成本**與**不透明的財務數據**。

### 1.2 MVP 拆解（**依 GDS 四問重新排序**）

依「First Two = Impact, Second Two = Risk」原則重排：MVP 3 ➝ MVP 1 ➝ MVP 2。

| 排序 | MVP | 核心功能 | Greenfield? | 機構複雜度 | 政治風險 |
|---|---|---|---|---|---|
| **第一棒** | **MVP 3 — 動態存貨價值監控（VaR）** | Streamlit 即時計算存貨抵押率 | **純 Greenfield** | **最低** | **最低** |
| 第二棒 | MVP 1 — 應收帳款動態定價系統 | 逆向保理 + 風險貼水動態定價 | 半 Greenfield | 中 | 中 |
| 第三棒 | MVP 2 — 替代數據智能信用評分引擎 | 用 SCM 數據生成 PD/EL | Brownfield | 最高 | 最高（直接挑戰風控核心） |

**排序原則：** 先以 MVP 3 累積 credibility 與政治資本（GDS 原則 — *Delivery is the best form of marketing*），再進攻 MVP 1，最後才碰風控部門最敏感的 MVP 2。

### 1.3 用戶定位（**重新定義 First User**）
- **North Star 用戶：** SME 借款人（最終受益方）
- **First User：** 銀行內部核貸專員 / 風險審查員 / 存貨融資承辦人
- **理由：** GDS 明示 *Most Reliable Sources are operational staff closest to the public*。第一線人員知道現況哪裡壞、有什麼 workaround；他們可訪談、可成為內部 champion，這同時解 Schutz 的 Inclusion 問題。

#### 痛點對應
| 角色 | 痛點 |
|---|---|
| SME（North Star） | 申貸流程冗長、缺乏傳統擔保品、現金流斷裂 |
| 核貸專員（First User） | 人工資料蒐集冗繁、缺數據量化依據、決策壓力大 |
| 風險審查員 | 缺替代數據、無工具評估高波動抵押品 |

---

## II. 現況與組織診斷 (Current-State & Organizational Diagnosis)

### 2.1 組織數位基線
傳統金融機構在 SME 貸款業務上數位成熟度偏低：高度依賴人工紙本、靜態擔保品估價、依賴標準化財報（多數 SME 缺此類報表）。

### 2.2 組織孤島診斷（**用 Conway's Law 重新解釋**）

「企金部 / 風控部 / IT 部 各自為政」**不是病因，是 Conway's Law 的必然結果**——**現有的孤島組織必然產出孤島系統**。

> 課程 Week 3 明示：21 世紀大多數 IT 導入「essentially digitized existing departmental silos rather than breaking them down」。

**結論：** 沒有先解決組織結構問題，只導入新技術只會把舊孤島數位化。本平台的根本動作不是寫程式，是**重新設計團隊邊界**（見 §IV.2）。

---

## III. 系統架構與流程重塑 (Proposed Architecture & Process Redesign)

### 3.1 概念性系統架構（**採 Bezos Mandate 設計原則**）

- **Service Interfaces Only：** 所有模組（VaR / 定價 / 信評）透過 API 對話，無 backdoor，無共享記憶體
- **Externalizable Design：** 所有 API 設計時即假設未來會對外開放（給核心買方、第三方 fintech）
- **數據源：** SCM 營運數據（訂單完成率、瑕疵率）、ERP 交易數據（發票、採購單）、市場價格 API
- **AI / 智慧邏輯：** 邏輯迴歸（替代信評）+ 計量金融（VaR、風險貼水公式）

### 3.2 流程重塑 (AS-IS vs TO-BE)
- **As-Is：** 人工蒐集文件 → 主觀人工審查 → 固定利率或拒絕 → 漫長等待
- **To-Be：** 自動化 API 數據匯入 → 演算法評分（PD/EL/VaR）→ 動態定價 → 即時初步核貸決策（KYC/AML 仍由原核心系統處理）

---

## IV. 交付治理與執行路線圖 (Delivery Governance & Execution Roadmap)

> **本章為老師 comment 三大缺項的回應重點。**

### 4.1 First Project 選擇
依 §1.2 的 GDS 四問檢驗，**第一棒 = MVP 3（動態存貨價值監控）**。理由：純 greenfield、機構複雜度最低、視覺衝擊強（Streamlit dashboard 即時 VaR 走勢）、可作為 13 週 alpha demo 累積政治資本。

### 4.2 部門設計（**回應「what department」**）

**核心原則：** 依 Conway's Law，要破除孤島系統先破除孤島組織。但避免整個銀行重組（Week 9 警告：純結構重組會引爆 *Holy War*），改採 **Two-Pizza Cross-functional Squad + Embedded Risk Officer** 模式。

#### Squad 編制（7 人，two-pizza 合格）

| 角色 | 來源部門 | 關鍵職責 |
|---|---|---|
| Product Manager | 企金部 | 對外面孔、產品願景、用戶代言 |
| Delivery Manager | 數位轉型辦公室 | 排除組織障礙、跨部門協調 |
| **Embedded Risk Officer**（嵌入式風控官） | **風控部** | **持有完整風險決策授權** — 課程 Week 9 的 Integrating Role |
| Quant Modeler | 風控部 / 外聘 | PD / EL / VaR 計算邏輯 |
| Lead Developer | IT 部 | Python / Streamlit / API |
| Data Engineer | IT 部 | SCM / ERP / 市場數據介接 |
| User Researcher + Content Designer（兼） | 外聘 / 內部調動 | 訪談核貸專員與 SME |

#### 治理機制 — System Forces Behavior（Week 9 策略 #2）
- **Shared Release KPI：** MVP 上線指標由 Squad 集體擁有，不由企金背績效、風控背風險
- **Joint Champion Pair：** Steering committee 報告一律「企金主管 + 風控主管」雙人並列
- **Definition of Done 內含風控簽核：** 風控不是 post-hoc reviewer，是 sprint 內 contributor
- **Co-location：** Squad 實體同空間（GDS 原則）

### 4.3 工作模式
- **Agile（兩週 sprint）**：每 sprint 出可 demo 的增量
- **Open**：程式碼 + 設計模式公開於內部 GitLab
- **Flat**：PM 是 facilitator 不是 dictator
- **Co-located**：實體同空間，降低早期溝通成本

### 4.4 24 個月路線圖（**回應「Roadmap」**）

| 月份 | 階段 | 目標 / 交付 |
|---|---|---|
| M0 – M1 | Setup | 成立 Squad、訪談 20 位核貸專員、確認 MVP 3 為 first project、簽訂 Shared KPI |
| M1 – M4 | **MVP 3 Alpha**（13 週 GDS 模式） | Streamlit VaR alpha 上線，模擬數據驗證 |
| M4 – M6 | MVP 3 Shadow | 在 1 個電子業供應鏈影子運行（不撥款，平行計算 VaR） |
| M6 – M10 | **MVP 1 開發** | 應收帳款動態定價 alpha |
| M10 – M14 | Pilot Phase 1 | MVP 1 + MVP 3 在 1 個核心大廠 + Tier-1 供應商試點（實際撥款） |
| M14 – M20 | **MVP 2 開發** | 替代數據信評引擎（此時 Squad 已 14 個月 credibility） |
| M20 – M24 | Pilot Phase 2 | 三模組整合，加入第二個核心大廠 |

### 4.5 Rollout Plan（**回應「rollout plan」**）

**核心硬規則（GDS）：** 每加一個獨立部門 / 產業 = **+6 個月**。

| 階段 | 範圍 | 預估時間 |
|---|---|---|
| 試點 | 1 個核心買方 + Tier-1 供應商，電子業 | M10 – M14 |
| 同產業擴張 | 2 – 3 個電子業核心買方 | M14 – M20（+6m per cohort） |
| 跨產業 | 零售 / 電商 / 農業 | Year 3+ |

**過渡 gate：**
1. **Shadow Mode** — 演算法計算但不決策
2. **Parallel Run** — 演算法決策 + 人工同步審核
3. **Phased Cutover** — 按產業 / 額度分批轉自動

---

## V. 成本治理 (Cost Governance)

### 5.1 成本結構
- **CapEx：** 風險模型開發、Streamlit 架構、初始雲端基礎設施
- **OpEx：** 市場數據 API 訂閱、雲端代管、模型校準與維護人力
- **隱性人力成本：** Embedded Risk Officer 跨部門 cost transfer（風控部 vs Squad 預算分攤）

### 5.2 數據化衡量指標
見 §VII.1 — 已分層為 Leading / Lagging Indicators。

---

## VI. 系統擴展風險與利害關係人治理 (System Scaling Risks & Stakeholder Governance)

### 6.1 數位生態系對映
**多方協作：** 核心買方（驗證發票）、SME 供應商（終端用戶）、第三方市場數據商（VaR 報價）、銀行核心 IT 部門（合規 + 撥款）。

### 6.2 利害關係人風險管理（**人系統 vs 工程系統**）

> Week 9 強調：純技術 / 結構視角是 *engineering system* 思維，會忽略人系統的 inclusion / control / openness 三大需求。光改 org chart 不會解 Holy War。

#### 風控部門的 Schutz 分析
| 維度 | 現況風險 | 緩解設計 |
|---|---|---|
| **Inclusion** | 風控部覺得自己被「Agile 數位部」取代，存在價值受威脅 | Embedded Risk Officer — 風控人員是 Squad 核心成員，不是局外人 |
| **Control** | 失去把關權，新模型繞過傳統 review | Definition of Done 含風控簽核；Risk Officer 持否決權 |
| **Openness** | 雙方互不信任 | Joint Champion Pair；公開 retrospective；Shared KPI |

### 6.3 系統擴展風險（技術面）
- **Shadow Mode → Parallel Run → Phased Cutover** 三階段過渡
- **Circuit breaker：** 任何模型決策若連續 N 次與人工結果偏離超過閾值，自動降級至人工 fallback

---

## VII. 效益評估與未來擴展性藍圖 (Evaluation of Benefits & Future Scalability)

### 7.1 成功指標（**分層 Leading / Lagging — Whiteboard Challenge 2 框架**）

> 老師明示：好的數位 PM 看 Leading Indicator，不只看 Lagging。Lines of Code 是反例。

#### Leading Indicators（每週追蹤）
- API 成功匯入的數據點數量
- 即時定價運算延遲（Latency）
- Sprint velocity / burndown
- 風控否決率變化趨勢
- 用戶（核貸專員）每週回饋分數
- 跨部門協作健康指標（4 Diagnostic Questions：bottlenecks / dependencies / complexity / feedback）

#### Lagging Indicators（每季追蹤）
- 初步徵信時間縮短率（目標 80%）
- 單位風險評估成本
- 試點 SME 撥款金額
- EL 預測 vs 實際違約變異程度

### 7.2 「完成 (Done)」的定義
每個 MVP 能成功匯入模擬數據，並即時產出符合數學與金融邏輯的指標；同時通過 Shadow Mode 驗證階段。

### 7.3 擴展性藍圖
試點驗證後，MVP 2（替代信評）的底層邏輯可橫向擴展至零售、電商、農業——平台是整體轉型藍圖，協助銀行從「重實體擔保的傳統徵信」徹底進化為「數據驅動的量化風險管理組織」。

---

## 附錄 — 三個 MVP 的詳細 PRD

- [MVP 3 — 動態存貨價值監控（VaR）](docs/MVP3-inventory-var.md)
- [MVP 1 — 應收帳款動態定價系統](docs/MVP1-reverse-factoring.md)
- [MVP 2 — 替代數據智能信用評分引擎](docs/MVP2-alt-credit-scoring.md)
