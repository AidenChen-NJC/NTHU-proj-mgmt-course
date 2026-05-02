# MVP 2 — 替代數據智能信用評分引擎 (Alt-data Credit Scoring Engine)

**Codename：** `alt-credit-scoring`
**排序：** **第三棒**
**開發窗：** M14 – M20
**Pilot：** M20 – M24（與 MVP 1、MVP 3 整合）

---

## 1. North Star
**對沒有標準財報的 SME 也能算出 PD / EL。** 用供應鏈營運指標（訂單完成率、瑕疵率、付款週期）取代或補強傳統財報，繞過 SME 融資最大瓶頸。

## 2. 用戶
- **First User：** 風險審查員、風控長、CRO
- **End Beneficiary：** 缺乏完整財報的微型企業 / 新創 SME

## 3. 痛點
- 80% 以上 SME 沒有經過會計師簽證的標準財報
- 傳統信評在 SME 樣本上 AUC 表現不佳
- 現有信評流程依賴 RM 主觀補述「軟資訊」，不可規模化

## 4. 計量邏輯
- **特徵工程：** SCM 營運指標（訂單完成率、瑕疵率、退貨率、付款週期、應收帳款週轉）
- **模型：** Logistic Regression（baseline，可解釋性強）+ Gradient Boosting（performance benchmark）
- **輸出：** PD（違約機率）→ EL = PD × LGD × EAD
- **解釋性要求：** SHAP / 特徵重要度報告，符合監理對 explainable AI 的要求

## 5. 數據源
| 數據 | 來源 | 風險 |
|---|---|---|
| SCM 營運指標 | 核心買方 ERP（已透過 MVP 1 整合） | 中 — 數據授權需明確同意書 |
| 發票歷史 | 銀行內 + 核心買方 | 低 |
| SME 繳款行為 | 銀行內部 | 低 |
| 歷史違約標籤 | 銀行內部風控資料庫 | **高 — 樣本量是模型成敗關鍵** |

## 6. 介面
- Streamlit dashboard 給審查員看模型輸出 + 解釋
- API 給 MVP 1（定價系統）即時調用 PD
- 模型監控 dashboard（drift detection）

## 7. 成功指標
- **Done：** 模型 AUC 顯著優於現行信評 baseline（statistically meaningful）
- **Leading：** 每週新增訓練樣本 / 模型迭代次數 / 跨部門 review 通過率
- **Lagging：** 試點期模型決策的實際違約率 vs 預測 EL；通過監理報備數量

## 8. 排序理由（為何留到最後）
- **直接挑戰風控核心：** 取代或補強傳統信評，最高政治敏感度
- **需要前兩棒累積的 credibility：** 沒有 18 個月 squad 跨部門協作紀錄，這個提案會被風控直接擋下
- **監理風險最高：** 替代數據建模需符合金融監理（個資、解釋性、公平性）
- **數據依賴最深：** 違約標籤、SCM 數據授權、樣本量都需前 14 個月累積

## 9. 風險
- **歷史違約樣本不足：** SME 違約事件相對稀少，模型可能 unstable
- **解釋性 vs 表現的取捨：** 太黑箱會被監理擋；太簡單會被質疑「沒比舊的好」
- **數據授權：** SCM 數據從核心買方流向銀行，需法律框架（已在 MVP 1 期間建立）
- **公平性挑戰：** 是否對某些產業 / 規模 SME 系統性歧視？需事前 fairness audit

## 10. 待釐清問題（給用戶確認）
- [ ] 銀行內部歷史 SME 違約樣本量是多少？是否足夠訓練？
- [ ] 監理單位（金管會 / 央行）對替代數據建模的最新立場？
- [ ] 是否預留 fairness / explainability audit 預算？
