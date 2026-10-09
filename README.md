# Go 3DP Agent 更新通道

Windows x64 Portable 開發預覽版，負責 CPD Booking 與區域網路內 3D 印表機的整合。

此版對齊 Python 0.3.13–0.3.15：停止／故障後保留重印授權、修正換檔遙測及離線回報順序、補齊更新安全判斷。部署前請先更新 CPD 的 stopped 契約與 Job detail 授權欄位。

目前版本：[0.2.2-dev](https://github.com/hikoyoshi/3dp-agent-go-updates/releases/tag/v0.2.2-dev)。此倉庫用於發佈套件與已簽章更新資訊。

## 安裝與啟動

1. 從 Release 下載 `3DP-Agent-Go-0.2.2-dev-windows-x64.zip` 並解壓到可寫入的資料夾。
2. 執行 `Start-Agent.cmd`，依畫面顯示的本機網址開啟管理介面。
3. 設定管理密碼，先用 Fake 機台驗證流程，再依套件內 `docs/hardware-validation.md` 進行現場驗收。

目的電腦不需安裝 Python、Go 或 Node.js。機台與 CPD 設定分開管理，真機連線與 CPD 接單預設關閉。

## 線上更新

系統設定 → 程式更新 → 檢查並下載。內建獨立的 Go 預覽通道；下載與簽章驗證不依賴 MQTT。

- 一般更新以新鮮 idle／completed 判斷閒置，已送達或停止的舊紀錄不永久阻擋。延後後於本次 Launcher 執行期間每 30 秒重查。
- 狀態未知或 Agent 卡住且沒有已知進行中作業時，可使用強制更新；列印、暫停、下載／傳檔／啟動仍須延後。網頁無法開啟時執行 `Force-Update.cmd`。
- 強制更新只處理 Agent 程序，不會向機台發送停止列印或斷電命令，也不會略過簽章驗證。
- 新版健康檢查失敗時回復程式與資料；原版與資料快照保留一組，最多 7 天。
- **0.2.2 需使用完整 Portable ZIP 更新 Launcher 一次。** 等列印與傳輸結束後執行 Stop-Agent，確認 Agent 結束，再替換三個 EXE 與 CMD，保留 data／updates／logs。Core 線上更新不會修正舊 Launcher；頁面會顯示版本差異。

發佈檔提供 SHA-256 清單；更新 manifest 使用 Ed25519 簽章。倉庫不包含現場設定、存取碼或簽章私鑰。

## 驗證狀態

本機驗證涵蓋 Fake 多檔重印、暫停／故障／斷線、回報順序、MQTT／FTPS／WebSocket 契約，以及真正 Core／Launcher 程序的簽章更新、卡死復原與回復。詳細測試結果見套件內 docs/validation.md。

P2S／X1C 實機及正式 CPD 部署驗收尚未執行；此版本仍為預覽版。首輪現場範圍為第一盤切片、外掛線材（AMS 關閉）、64 MiB 以內；遠端刪檔預設關閉。請依套件內驗收文件逐項確認後再交接現有 Agent。
