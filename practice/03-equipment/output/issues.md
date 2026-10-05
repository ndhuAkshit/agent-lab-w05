# 社團器材記錄清理問題報告 / Equipment Normalization Issues Report

## 1. 資料筆數統計 / Row Count Statistics

- **原始列數 (Original rows)**: 10 筆（含空行）
- **有效列數 (Valid normalized rows)**: 9 筆（保留原始 `source_row`）
- **移除列數 (Removed rows)**: 1 筆（原始第 6 筆為完全空白之空物件 `{}`）

---

## 2. 欄位清理與狀態標準化規則 / Normalization Rules Applied

1. **文字前後空白修剪 (Trim whitespace)**:
   - `item_id`、`name`、`status` 前後空白均已去除（例如 `" EQ01 "` → `"EQ01"`、`" available "` → `"available"`）。
   - `qty` 欄位不套用文字去空白或轉型規則，完全保留原值。
2. **借還狀態統一 (Status Mapping)**:
   - `available`、`可借`、`可出借` 統一標準化為 `available`。
   - `borrowed`、`借出` 統一標準化為 `borrowed`。
   - 非上述狀態（如 `待盤點`）一律標記為 `unknown` 並列入異常清單。

---

## 3. 重複 ID 與衝突檢查 / Duplicate Item IDs & Conflicts

| item_id | 涉及來源列號 (source_row) | 比對狀況 | 詳細說明 |
|---|---|---|---|
| `EQ01` | 第 1 列、第 4 列 | 欄位完全相符 | 兩筆名稱皆為 `Marker / 白板筆`，`qty` 均為 4，狀態均為 `available`。全部保留，待管理人員確認是否為重複登錄。 |
| `EQ02` | 第 2 列、第 5 列 | 數量欄位衝突 (`qty` conflict) | 兩筆名稱皆為 `Extension cord / 延長線`、狀態為 `borrowed`，但第 2 列 `qty` 為 2，第 5 列 `qty` 為 3。依指示保留所有有效列，不擅自猜測或加總，列出衝突供人員覆核。 |

---

## 4. 異常值與待確認問題清單 / Anomalies and Unverified Items

1. **`source_row: 7` (`item_id: "EQ04"`, 紙張包)**:
   - 問題：`qty` 為空字串 `""`。
   - 處置：原樣保留為空字串，不補 0、不猜測數量，需庫房人員實地清點。
2. **`source_row: 8` (`item_id: "EQ05"`, 膠帶)**:
   - 問題：`qty` 為負數 `-1`（不符合零或正整數規格）。
   - 處置：原樣保留 `-1`，不取絕對值、不擅改為 0，需確認是否為預支、帳目錯誤或系統記號。
3. **`source_row: 9` (`item_id: "EQ06"`, 剪刀)**:
   - 問題：原始狀態為 `"待盤點"`，非標準借還狀態。
   - 處置：狀態轉換為 `"unknown"`，需待幹部盤點完成後更新為 `available` 或 `borrowed`。
4. **`source_row: 10` (`item_id: "EQ07"`, 資料夾)**:
   - 數量 `qty: 0`：符合「零或正整數」規定，表示目前庫存歸零，保留記錄。
