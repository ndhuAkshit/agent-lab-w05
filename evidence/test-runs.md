# 實作測試與驗收紀錄 / Verification and Test Run Evidence

## 1. 任務 A 雜湊比對驗收記錄 (SHA-256 Checksum Verification)

全部 12 筆輸入檔案與輸出副本皆經由 SHA-256 雜湊演算法逐位元檢驗：

```text
announcement.txt       : C19164B1054EBF52ED33BC5321C887985EA043A0B07DB1E99F866986D2CCD584 [MATCH]
announcement_copy.txt  : C19164B1054EBF52ED33BC5321C887985EA043A0B07DB1E99F866986D2CCD584 [MATCH]
budget_draft.txt       : 6CCE6841AA67D39CE77FFF86D86E2FB2DCCB825AF7FBA3083A62F1A2D2D9AAA0 [MATCH]
equipment_backup.txt   : C21D53BA1F8D3AD06E1833539935120A5BACA4D13B34A2AA98CE157FF4502FA3 [MATCH]
equipment_list.txt     : C21D53BA1F8D3AD06E1833539935120A5BACA4D13B34A2AA98CE157FF4502FA3 [MATCH]
feedback_questions.txt : 96A1CA56D5487DCCB6A06067507C574506C52415F9BA9BDE3DF096F9AE8BBBFD [MATCH]
meeting_notes.txt      : AB8AFBABF0369237C10801D83AA6599DD3D97CE661752FBCE49C10E5BF52CB6E [MATCH]
next_steps.txt         : F448D8C034479DFFE0E33E89216CBB6DFEE8BC6A890430D265FF7933C7CE91A8 [MATCH]
poster_text.txt        : 4CC83B76A30E0E61466B670C8981DE3B41689594F16E88EBFAF2333B3B21A9D5 [MATCH]
proposal_final.txt     : C520147E4561C8799949CE873B3DAA14C7D6D0FE2B6CEC4BEB2C8F87921B0345 [MATCH]
proposal_final2.txt    : 6FCCE7BC6E075CCF7FAD659BD632A6840227CD015B2DCE0DA4884BB36FE9FAA8 [MATCH]
rain_plan.txt          : C3791F2CFFF663D7317E8D4826EFAE1D8FB4BAD6A63886F148B66DABD4B8DFD8 [MATCH]

狀態：12 檔全部吻合 (All 12 files match 100%)。
原檔未受任何刪改。
```

---

## 2. 任務 B 挑選器自動測試記錄 (Campus Picker Test Suite)

由自動化測試腳本跑測 6 項規格標準：

```text
✓ Test 1: 室內 / 15分鐘 / 低強度
  -> 候選活動 ID: [ 'A01', 'A02', 'A03', 'A04' ]
  -> 結果符合預期: true

✓ Test 2: 室外 / 15分鐘 / 中強度
  -> 候選活動 ID: []
  -> 顯示「沒有符合條件的活動」，不放寬條件: true

✓ Test 3: 室外 / 30分鐘 / 中強度
  -> 候選活動 ID: [ 'A09' ]
  -> 每次抽取必定且僅為 A09: true

✓ Test 4: 不限 / 60分鐘 / 不限，連續抽取 6 次
  -> 歷史紀錄長度: 5（最舊者自動移出，最新者置頂）: true

✓ Test 5: 重設篩選 (Reset Filters)
  -> 回復為 地點=all, 時間=30, 強度=all
  -> 歷史紀錄完整保留未被清除: true

✓ Test 6: 清除紀錄 (Clear History) 與語言切換 (i18n)
  -> 歷史清單清空，顯示提示訊息
  -> 中英即時切換無殘留中文: true
```

---

## 3. 任務 C 器材標準化數據統計 (Equipment Normalization)

```text
原始列數: 10
全空列移除: 1 (source_row 6 為空物件 {})
有效列保留: 9
文字欄位空白去除: item_id, name, status (qty 不去空白)
狀態歸一化:
  - 可借 / 可出借 / available -> available
  - 借出 / borrowed           -> borrowed
  - 待盤點                    -> unknown (列入 issues.md)
異常數值原樣保留:
  - source_row 7: qty = "" (空值)
  - source_row 8: qty = -1 (負數)
衝突報告:
  - EQ02 延長線: source_row 2 (qty: 2) vs source_row 5 (qty: 3)
```

---

## 4. 任務 D 計畫退回審查結論 (Bad Plan Review)

```text
審查對象: practice/04-review/bad-plan.txt
結論: REJECTED (退回)
退回意見書位置: practice/04-review/my-rejection.md
主要違反:
  1. 範圍越界至 Downloads
  2. 擅自刪除重複檔
  3. 憑 final2 猜測最新版
  4. 隨意填補缺漏值
  5. 自動公開成果造成資安與隱私外洩
```
