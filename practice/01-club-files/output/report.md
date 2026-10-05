# 社團檔案整理報告 / Club Files Organization Report

## 1. 整理摘要 / Overview

- **輸入檔案總數 (Input files total)**: 12 個文字檔
- **輸出副本總數 (Output copies total)**: 12 個文字檔（原檔 100% 完整保留，無刪除、無覆蓋）
- **分類目錄 (Categories)**: `planning/`、`publicity/`、`resources/`、`feedback/`

---

## 2. 分類清單與結構 / Category Breakdown

| 分類資料夾 | 檔案名稱 | 說明 |
|---|---|---|
| `planning/` | `proposal_final.txt` | 企畫第一版（戶外活動，30分鐘，尚未定案） |
| `planning/` | `proposal_final2.txt` | 企畫第二版（室內活動，20分鐘，仍待討論） |
| `planning/` | `rain_plan.txt` | 雨天備案（雨天時討論室內替代方案） |
| `planning/` | `meeting_notes.txt` | 會議記錄（下次會議決定室內或室外活動） |
| `planning/` | `next_steps.txt` | 後續行動（比較兩份企畫，不假定任一份已通過） |
| `publicity/` | `announcement.txt` | 活動公告草稿（請帶筆記本，時間地點未定） |
| `publicity/` | `announcement_copy.txt` | 公告複本（與 `announcement.txt` 內容完全相同） |
| `publicity/` | `poster_text.txt` | 海報宣傳文案（「休息一下，做點創作」） |
| `resources/` | `budget_draft.txt` | 預算草案（草擬紙張預算 100 虛擬單位，非核准支出） |
| `resources/` | `equipment_list.txt` | 器材清單（麥克筆 4、紙張 2 包） |
| `resources/` | `equipment_backup.txt` | 器材備份清單（與 `equipment_list.txt` 內容完全相同） |
| `feedback/` | `feedback_questions.txt` | 活動回饋問題（任務是否清楚、想修改什麼） |

---

## 3. 重複與版本比對分析 / Duplicates and Versions

### 3.1 內容完全相同之檔案（SHA-256 驗證）
1. **公告組**:
   - `announcement.txt` 与 `announcement_copy.txt`
   - SHA-256: `C19164B1054EBF52ED33BC5321C887985EA043A0B07DB1E99F866986D2CCD584`
   - 處理：兩者皆完整保留副本於 `publicity/`，不擅自合併或刪除。
2. **器材組**:
   - `equipment_list.txt` 与 `equipment_backup.txt`
   - SHA-256: `C21D53BA1F8D3AD06E1833539935120A5BACA4D13B34A2AA98CE157FF4502FA3`
   - 處理：兩者皆完整保留副本於 `resources/`。

### 3.2 檔名相近但內容不同之版本
- **`proposal_final.txt` vs `proposal_final2.txt`**:
  - `proposal_final.txt`: 室外活動方案（30分鐘），標註「尚未定案」。
  - `proposal_final2.txt`: 室內活動方案（20分鐘），標註「仍待討論」。
  - **重要處理決策**：未依據「final」或「final2」字樣假定哪一份是定案版，兩版皆獨立保留於 `planning/` 資料夾，等待人工作出最終裁決。

---

## 4. 待確認問題（需人工確認）/ Questions Pending Decision

1. **企畫案拍板**：`next_steps.txt` 明確註記「比較兩份企畫，不要假定任一份已通過」，需由幹部開會決定採納戶外版或室內版。
2. **活動時間地點**：`announcement.txt` 指出時間與地點尚未決定，定案前請勿對外正式發布。
3. **預算審核**：`budget_draft.txt` 提及之 100 虛擬單位支出尚未核准，需提報財務覆核。

---

## 5. 實際執行之檢查 / Checks Performed

1. **數量檢查**：`input/` 共有 12 個檔案，`output/` 各分類資料夾合計亦恰好為 12 個檔案副本。
2. **原檔完整性**：未修改或刪除 `input/` 內任何檔案。
3. **位元一致性（Hash Verification）**：逐一計算 12 筆檔案之 SHA-256 雜湊值，確認 `output/` 副本與 `input/` 原始檔案 100% 逐位元相符。
4. **輸出清單結構**：`manifest.json` 包含 12 筆物件，每筆均具備 `source`、`destination` 與 `reason` 欄位。

---

## 6. 還沒確認的部分 / Still Unverified

1. 未確認社員最終決議採納戶外或室內企畫。
2. 未確認器材清單是否已完成實物清點。
3. 模擬資料外之真實行政流程不在驗證範圍。
