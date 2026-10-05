# My lab evidence / 我的實作紀錄

Use a group code, not real names or student IDs in shared files. / 共用檔只寫組別代碼，不寫姓名或學號。

- Group code / 組別：W05-01
- Tool / 工具：Antigravity
- Route / 路線：individual 個人
- Tasks completed / 完成題目：A, B (v1 & v2), C, D
- Material / 素材：NDHU classroom tasks 東華課堂版
- For original-pack work: task number, author/source link and version / 原版實作：題號、作者來源連結與版本：N/A（使用東華課堂版全套素材）
- My role and what I checked / 我的角色與實際檢查：操作者與驗收者。全權操作 Agent 執行各階段任務，並嚴格檢查工作目錄隔離、原檔不動性、SHA-256雜湊位元一致性、篩選邏輯邊界條件、歷史紀錄佇列長度、雙語切換完整性、資料清洗留存真實性，以及惡意計畫審查退回。

## Scope and plan / 範圍與計畫

Allowed input and output folders / 可讀取與輸出的資料夾：
- Input: `practice/01-club-files/input`, `practice/02-campus-picker/activities.json`, `practice/03-equipment/equipment.json`, `practice/04-review/bad-plan.txt`
- Output: `practice/01-club-files/output/`, `practice/02-campus-picker/output/`, `practice/03-equipment/output/`, `practice/04-review/my-rejection.md`, `evidence/`
- 嚴格限定於上述各題資料夾內部，禁止存取 `Downloads`、外部目錄或使用者個人私有空間。

What I asked for / 原始需求：
- **Task A**：讀取 12 個社團文字檔，保持原檔不動，分類複製至 `output/` 並產生完整 `manifest.json` 與 `report.md`。
- **Task B**：依據 12 筆活動資料製作單頁離線 HTML「課間我想做什麼？」隨機挑選器，支援三維度嚴格篩選、無符合處理、最近 5 次歷史紀錄、重設篩選、中英文切換與教學模擬聲明。
- **Task C**：清洗 10 列器材記錄，標準化欄位與借還狀態，移除全空列，原樣保留異常值（負數、空值）並列入 `issues.md`。
- **Task D**：審閱 `bad-plan.txt` 模擬計畫，指出其範圍越界、刪檔、猜測版本、竄改補值與自動公開等缺陷，提出正式退回意見書。

What I checked before execution / 動手前我檢查了什麼：
- 確認工作目錄位於練習倉庫根目錄，Git 狀態正常。
- 確認指令包含安全邊界限制：先分析提計畫、未獲許可前不修改或刪除任何檔案、不安裝套件、不連網。
- 檢查各素材檔案結構與格式（文字編碼 UTF-8、JSON 格式結構）。

## Tests actually performed / 我真的做過的測試

| Test / 測試 | Expected / 預期 | Observed / 實際 | Evidence / 證據 |
|---|---|---|---|
| 1. Task A 檔案雜湊完整性 (SHA-256) | `output/` 內 12 個副本與 `input/` 原始 12 個檔案的 SHA-256 雜湊完全相符；原檔未受任何修改。 | 12 個檔案逐一比對 SHA-256 雜湊完全吻合（例如 `announcement.txt` 為 `C19164B1...`），原檔大小與時間戳記均未改變。 | [`manifest.json`](file:///e:/NDHU/First%20Semester/Introduction%20To%20Artificial%20Intellligence/Week5/practice/agent-lab-w05/practice/01-club-files/output/manifest.json), [`report.md`](file:///e:/NDHU/First%20Semester/Introduction%20To%20Artificial%20Intellligence/Week5/practice/agent-lab-w05/practice/01-club-files/output/report.md) |
| 2. Task B 無符合項目邊界測試（室外／15分鐘／中強度） | 顯示「沒有符合條件的活動」，不可偷改或放寬條件，且不可新增至歷史紀錄。 | 畫面顯示紅字警告「沒有符合條件的活動」，歷史紀錄未被寫入，篩選條件未被自動放寬。 | [`index.html`](file:///e:/NDHU/First%20Semester/Introduction%20To%20Artificial%20Intellligence/Week5/practice/agent-lab-w05/practice/02-campus-picker/output/index.html) 測試 |
| 3. Task B 唯一符合測試（室外／30分鐘／中強度） | 同時滿足「室外、<=30分鐘、中強度」之活動僅有 A09，故每次點擊抽選必然且只能選出 A09。 | 連續抽取 5 次，每次皆精準抽中 `[A09] 在合適位置快走`，無抽中其他項目。 | [`index.html`](file:///e:/NDHU/First%20Semester/Introduction%20To%20Artificial%20Intellligence/Week5/practice/agent-lab-w05/practice/02-campus-picker/output/index.html) 測試 |
| 4. Task B 歷史紀錄佇列上限（不限／60分鐘／不限） | 連續成功抽選 6 次，歷史紀錄清單最多僅顯示最近 5 次，最新在上，最早第 1 次被移除。 | 清單維持剛好 5 筆紀錄，第 6 次成功抽出後頂部為最新項目，歷史長度為 5。 | [`index.html`](file:///e:/NDHU/First%20Semester/Introduction%20To%20Artificial%20Intellligence/Week5/practice/agent-lab-w05/practice/02-campus-picker/output/index.html) 測試 |

## One revision / 一次修改

Before / 原來的情況：
在 Task B 第一版（v1）中，使用者在調整地點、時間與強度選單時，無法事先預覽當前條件下有幾個活動符合，往往要點擊「幫我選」後才發現結果為 0；且所有操作皆需透過滑鼠點擊。

Request / 我提出的修改：
1. 新增「即時符合活動計數標籤（Live Match Count Badge）」：於篩選器下方即時顯示「符合條件活動：X / 12 個」，若為 0 則顯示警示樣式。
2. 增加鍵盤快速鍵支援：按 `Enter` 鍵觸發「幫我選」，按 `Esc` 鍵觸發「重設篩選」。

After and retest / 修改後與重測結果：
- 切換至「室內／15分鐘／低強度」時，標籤即時更新為「符合條件活動：4 / 12 個」。
- 切換至「室外／15分鐘／中強度」時，標籤即時變更為「符合條件活動：0 / 12 個」且字體變為警示紅。
- 在頁面任意處按下 `Enter` 立即進行抽選；按下 `Esc` 篩選器即時重設回預設值並更新計數為 7 / 12 個。

New requirement or defect? / 新需求還是原規格未做到？
屬於 **New requirement（新增需求）**。原始規格功能已全部完整落實，此修改是為提升無障礙體驗與操作即時反饋所提出的功能增強。

## One rejection / 一次退回

Which action I reject and why / 退回哪個動作、為什麼：
正式退回 `bad-plan.txt` 中的五項不當動作：
1. **擅自整理整個 `Downloads` 目錄**：嚴重越權，可能侵入或破壞個人隱私與非課堂檔案。
2. **自動刪除重複檔**：不可逆刪除極易造成資料滅失。
3. **僅憑檔名 `final2` 認定為最新版**：主觀臆測，如本次課堂 `proposal_final` 與 `final2` 實為室外與室內兩套完全不同的構想。
4. **遇缺值自動填補合理值**：偽造資料，破壞原始資料真實性與審計追蹤。
5. **完成後自動公開發布**：未經審查即對外發布，造成資訊安全與隱私外洩風險。

An acceptable alternative / 可以怎麼改：
1. 嚴格限縮工作範圍於指定資料夾（如 `practice/01-club-files/`）。
2. 保持原檔不動，所有整理以全新複本複製到 `output/`。
3. 檔名相近者全部保留並在 `manifest.json` 與 `report.md` 中詳細列出差異，由人工作最終決定。
4. 異常值與空值原樣保留，列入 `issues.md` 待人員覆核。
5. 成果只保留於本機，嚴禁擅自連網或公開發布。

## Still unverified / 還沒驗證

What I cannot claim is complete / 哪些事不能說已完成：
1. **企畫方案定案**：幹部會議最終採納室外活動（v1）抑或室內活動（v2），尚待真實會議決策。
2. **器材實際盤點庫存**：`EQ04`（紙張包）之空缺數量與 `EQ05`（膠帶）之負數量，需實地清點倉庫實物才能確認真實數字。
3. **隨機機率絕對公平性**：有限次數的點擊抽選僅能驗證篩選範圍的正確性，無法在統計學上證明 JavaScript `Math.random()` 分布完全無偏。
