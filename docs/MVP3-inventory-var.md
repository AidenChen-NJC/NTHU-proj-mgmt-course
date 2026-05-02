# MVP 3 — 動態存貨價值監控系統 (Inventory VaR Monitor)

**Codename：** `inventory-var`
**排序：** **第一棒（First Project）**
**開發窗：** M1 – M4（13 週 alpha，GDS 模式）
**Shadow window：** M4 – M6

---

## 1. North Star
**解凍 SME 凍結資金。** 透過 Value-at-Risk 安全估算高波動存貨的抵押價值，將傳統銀行不敢吃的庫存轉為可融資資產。

## 2. 用戶
- **First User：** 銀行存貨融資承辦人 / 風險審查員（內部）
- **End Beneficiary：** 持有大量原物料 / 製成品庫存的 SME

## 3. 痛點
- 銀行傳統不接受存貨抵押，或大幅折價（haircut 60% +）
- SME 庫存佔資產比重高但無法變現
- 缺乏量化工具評估價格波動風險

## 4. 計量邏輯
- **VaR 計算：** Historical Simulation + Parametric（Variance-Covariance）雙軌驗證
- **時間窗：** 1-day / 5-day / 20-day（對應實際處分期）
- **信賴水準：** 95% / 99%
- **抵押率推薦：** `LTV = 1 - VaR_loss / current_market_value - safety_margin`

## 5. 數據源
| 數據 | 來源 | 取得方式 |
|---|---|---|
| 歷史市場價格 | Bloomberg / 業界商品報價 API | OpEx 訂閱 |
| 即時行情 | 同上 | API |
| 庫存清單 | SME 端 ERP（試點期手動匯入 CSV） | 過渡用，未來 API |

## 6. 介面（Streamlit）
- 標的選擇（商品代碼）
- VaR 計算結果視覺化（distribution + tail）
- 敏感度分析（不同 confidence / horizon）
- 推薦抵押率 + 建議授信額度
- 歷史 backtest 報表

## 7. 成功指標
- **Done：** alpha 能匯入模擬數據並產出符合金融邏輯的折價率
- **Leading：** 每週新增 backtest 通過案例數 / 模型 latency
- **Lagging（Pilot 期）：** 試點期 SME 透過存貨抵押取得授信總額

## 8. 排序理由
- **Greenfield 純度最高：** 銀行內既有信評 / 逆向保理流程都不碰
- **政治風險最低：** 沒有現任「擔當」要被取代
- **視覺衝擊強：** Streamlit 可直接 demo 給高層看 VaR 動態圖
- **Squad credibility：** 用 13 週交付一個能 demo 的 alpha，建立後續推 MVP 1 / 2 的政治資本

## 9. 風險
- 市場數據 API 訂閱成本（OpEx 預算需先確認）
- 商品種類差異大（電子零件 vs 大宗物資 vs 成品），第一版需限定品類
- VaR 模型在尾端（fat tail）失效——需搭配 stress testing

## 10. 待釐清問題（給用戶確認）
- [ ] 第一版鎖定哪一類商品？（建議：電子業常見原物料）
- [ ] 市場數據 API 預算上限？
- [ ] 試點供應鏈是否已有口頭意向的核心大廠？
