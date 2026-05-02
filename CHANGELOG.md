# Changelog — 專案管理

Brief release notes. Newest first.

---

## 2026-05-02 — Demo 課堂版互動升級 (Plan B)
- 引入主角「陳老闆的精密金屬加工廠」貫穿四份 HTML，將抽象框架轉成具體案件
- `demos/index.html` 改寫:30 秒三幕故事輪播 (被拒 → 平台啟動 → 核准) + 互動式 SVG 案件流程圖 (點節點看細節) + 整合決策卡 (三訊號匯流 → 1,500 萬 @ 3.2%)
- 三個 MVP 統一加入:跨 MVP 導覽列、陳老闆案件背景卡 (可收合)、Before/After 切換按鈕、key metric 上的「為什麼？」tooltip、站尾整合決策卡 (帶下一站連結)
- MVP 數字以陳老闆案件為預設:MVP 3 銅 800 萬 → 714 萬可融、MVP 1 對台積電 1,200 萬 → 動態 2.56% (-43%)、MVP 2 PD 1.42% (傳統 3% 退件)
- 全部 Self-contained (零 CDN 依賴)，數字 Node 驗證一致

## 2026-05-02 — V2 Framework + 簡報講稿 + 互動式 demo
- 重寫期中 checkpoint 為 V2 Framework（PRD.md），補上 Roadmap / 部門設計 / Rollout Plan
- 注入 Conway's Law、GDS 模式、Schutz Framework 三大課程主軸
- MVP 排序由 1→2→3 改為 **3→1→2**（greenfield + 政治風險優先）
- 新增三份 MVP PRD 草案於 docs/
- 新增 V2 高層簡報講稿（docs/V2-簡報講稿.md）— 顧問口吻完整邏輯鏈 why/who/how
- 新增四份 self-contained HTML demo 於 demos/（總覽 + 三 MVP 互動式）
  - MVP 3 · 動態存貨 VaR：Historical + Parametric VaR、敏感度表、SVG 走勢圖 + 直方圖
  - MVP 1 · 應收帳款動態定價：利率組成拆解、動態 vs 靜態對比、6 筆批次試算
  - MVP 2 · 替代信評：Logistic Regression PD 計算、SHAP-like 特徵歸因、5 個範例 SME 預設
