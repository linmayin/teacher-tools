# 📋 PROGRESS.md — teacher-tools 專案修改紀錄

> 每次請 Claude 協助修改後，請更新此檔案並 commit。
> 格式：日期 → 改了什麼 → 影響哪些檔案 → 下一步

---

## 🗂️ 專案總覽

**Repository：** `linmayin/teacher-tools`
**部署網址：** https://linmayin.github.io/teacher-tools/
**主要工具列表：**

| 檔案 | 功能 | 裝置 |
|------|------|------|
| `index.html` | 工具首頁（🌻 向日葵風格） | 全裝置 |
| `life_rules.html` | 生活常規登記表（違規記錄＋家長通知） | iPad |
| `merit_record_v2.html` | 功過追蹤系統 | iPad |
| `teaching_progress.html` | 教學進度表（多班、週次、調課、匯出） | iPad |
| `classroom_record.html` | 課堂記錄（四班） | 手機 |
| `classroom_record_5class.html` | 課堂記錄（五班） | 手機 |
| `grade_voice_system.html` | 語音成績登記系統 | 手機 |
| `exam_dashboard.html` | 段考看板（含整點提示音） | 學校電腦 |
| `icon_teach.png` | 教學進度表主畫面圖示 | — |

---

## 📅 修改紀錄

### 2025 年以前（初始建置）
- 建立 GitHub Pages 部署架構
- 完成 `life_rules.html` 核心功能：
  - 9 項違規類別 tap 格線
  - 每週扣點計算
  - 家長通知文字匯出
  - CSV 匯入格式：`座號,項目,星期`（`作業缺交` 支援第四欄填科目）
- 建立 `index.html` 首頁，加入裝置適用徽章與說明
- 建立 `merit_record_v2.html` 功過追蹤

---

### 2025-06-29 ～ 2025-06-30　teaching_progress.html 全新建置

**修改人：** Mia（搭配 Claude 協助）

#### 本次完成

**基礎架構**
- 全新建立 `teaching_progress.html`，單一 HTML 檔案部署於 GitHub Pages
- 淺色模式設計，針對 iPad 橫放操作優化
- 資料存於瀏覽器 localStorage（key: `teachingProgress_v5`），從主畫面圖示開啟與 Safari 開啟為獨立儲存空間

**課表設定**
- 支援多班級（可自行新增／刪除，不限數量）
- 每班可設定週一～週五各天的節次、是否為彈性課程、是否有第八節
- 特殊情況支援：
  - 僅有彈性、無一般節次（如 202 週一，`period:null, isFlex:true`）
  - 僅有第八節、無一般節次（如 301 週一，`period:null, hasEighth:true`）
- 課表可由 Claude 解析照片後產生 JSON 貼入匯入，匯入前有確認提示
- 也可在設定介面手動新增／調整

**行事曆與特殊日**
- 特殊日類型：段考、複習考、放假日、其他
- 支援連續日期範圍設定（如段考連續兩天）
- 行事曆可由 Claude 解析 PDF 後產生 JSON 貼入匯入
- 匯入時自動計算最大週數並更新總週數設定
- 放假日整列鎖定不可輸入，顯示橘色背景
- 段考／複習考顯示紅／黃色背景提示

**進度填寫**
- 縱軸：週一～週五（各天）
- 橫軸：各班級並排
- 每格固定三欄：考試、進度、作業
- 彈性課程顯示藍色背景，第八節補課顯示紫色背景
- 每週有「本週備註」欄位

**複製功能**
- **單格複製**：每格右上角「複製→」按鈕，可選目標班級、目標天（跨天）、節次類型
- **拖曳複製**：格子可拖曳到同週另一班（覆蓋）
- **批次複製**：選來源班、目標班（可多選）、週次範圍，三種類型（一般／彈性／第八節）各自獨立按課次順序配對

**調課功能**
- 設定來源（班級、週次、星期）與目標（週次、星期、節次），可跨週
- 節次類型自動依課表判斷（無需手動選）
- 來源格顯示「🔄 調課」遮罩並鎖定
- 目標格新增「🔄 調課補課」區塊（即使目標天原本無課也能顯示）
- 可從調課列表刪除

**匯出**
- 匯出全學期 Word（.doc），每週一頁，橫向 A4
- 檔名格式：`教學進度_YYYYMMDD.doc`

**主畫面圖示**
- 上傳自訂圖示 `icon_teach.png`（粉色「教」字可愛風格）
- HTML 加入 `apple-touch-icon` 與 `apple-mobile-web-app-capable` meta
- 從 Safari 加入主畫面後顯示自訂圖示，以 App 模式開啟（無網址列）

**其他**
- 加入 no-cache meta 標籤減少快取問題
- loadState 加入資料清理機制，避免壞資料導致整個 JS 當掉
- 修正多個因 `sed` 破壞 UTF-8 編碼造成的 JS 錯誤
- 修正 HTML 模板字串中 `</style>` 等標籤未跳脫導致 script 提前結束的問題

#### 修改的檔案
- `teaching_progress.html`（新建）
- `icon_teach.png`（新建）
- `index.html`（新增教學進度表卡片）
- `docs/PROGRESS.md`（本次更新）

#### 已知限制
- localStorage 在主畫面 App 模式與 Safari 各自獨立，換裝置無法同步
- 匯出 Word 為 HTML-based .doc，部分格式在 Word 中可能需要調整
- iOS Safari 快取較積極，更新後有時需從 Safari 重新加入主畫面才能看到新版

#### 下次要做
- 持續根據實際使用回饋調整介面與功能

---

## 📌 長期待辦 Backlog

- [ ] 家長通知改為逐筆 Gmail draft（目前已可用 Claude 批次產生）
- [ ] 段考看板加入更多科目彈性設定
- [ ] 考慮新增「座位表編輯器」工具
- [ ] 教學進度表：評估是否需要雲端同步（目前為本機儲存）

---

## 🧠 AI 協作規則（給 Claude 看的）

每次修改前，請先說明要改哪些檔案及原因。
每次修改後，請更新本檔案的「最新更新」區塊，並建議 commit message。

**CSV 匯入格式（生活常規）：** `座號,項目,星期`
例：`3,作業缺交,一`（作業缺交可加第四欄填科目名稱）

**課表 JSON 格式：**
```json
[{"name":"313","schedule":[
  {"period":"第2節","isFlex":false,"hasEighth":false},
  null,
  {"period":"第3節","isFlex":true,"hasEighth":false},
  {"period":"第1節","isFlex":false,"hasEighth":false},
  {"period":"第4節","isFlex":false,"hasEighth":true}
]}]
```
schedule 為5元素陣列（週一到週五），無課填 null。
特殊：`period:null, isFlex:true` = 僅彈性課；`period:null, hasEighth:true` = 僅第八節

**行事曆 JSON 格式：**
```json
[{"date":"2025-10-14","type":"exam","note":"第1次定期評量"}]
```
type：exam（段考）、review（複習考）、holiday（放假日）、other
