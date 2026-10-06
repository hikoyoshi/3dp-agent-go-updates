# Go 3DP Agent 更新通道

Windows x64 Portable 開發預覽版，負責 CPD Booking 與區域網路內 3D 印表機的整合。

此版同步 Python 0.3.12 的多檔授權修正。部署前請先更新 CPD Server，確認 Job detail 提供 `print_authorized` 與 `print_denial_reason`。支援多檔任意順序、有效時段重印及重新啟動後的檔名授權核對。

目前版本：[0.2.1-dev](https://github.com/hikoyoshi/3dp-agent-go-updates/releases/tag/v0.2.1-dev)。此倉庫用於發佈套件與已簽章更新資訊。

## 安裝與啟動

1. 從 Release 下載 `3DP-Agent-Go-0.2.1-dev-windows-x64.zip` 並解壓到可寫入的資料夾。
2. 執行 `Start-Agent.cmd`，依畫面顯示的本機網址開啟管理介面。
3. 設定管理密碼，先用 Fake 機台驗證流程，再依套件內 `docs/hardware-validation.md` 進行現場驗收。

目的電腦不需安裝 Python、Go 或 Node.js。機台與 CPD 設定分開管理，真機連線與 CPD 接單預設關閉。

## 線上更新

系統設定 → 程式更新 → 檢查並下載。內建獨立的 Go 預覽通道；下載與簽章驗證不依賴 MQTT。

- 一般更新會等待工作完成，並確認機台閒置。
- 狀態未知或 Agent 卡住時，可使用強制更新；網頁無法開啟時執行 `Force-Update.cmd`。
- 強制更新只處理 Agent 程序，不會向機台發送停止列印或斷電命令，也不會略過簽章驗證。
- 新版健康檢查失敗時回復程式與資料；原版與資料快照保留一組，最多 7 天。
- 目前線上更新 Core（包含 UI）。Launcher 與 Maintenance 更新需使用完整 Portable ZIP，保留原資料目錄。

發佈檔提供 SHA-256 清單；更新 manifest 使用 Ed25519 簽章。倉庫不包含現場設定、存取碼或簽章私鑰。

## 驗證狀態

0.2.1-dev 已通過本機 Go 測試、MQTT／FTPS／WebSocket 協定測試，以及從 GitHub 將 0.2.0 升級到 0.2.1 的實際流程（包含機台未知時下載、一般更新暫緩與強制更新）。

P2S／X1C 實機及正式 CPD 部署驗收尚未執行；此版本仍為預覽版。首輪現場範圍為第一盤切片、外掛線材（AMS 關閉）、64 MiB 以內；遠端刪檔預設關閉。請依套件內驗收文件逐項確認後再交接現有 Agent。
