# Windows 11 Update 暫停工具 (Windows 11 Update Pause Tool)

[![OS](https://img.shields.io/badge/OS-Windows%2011%20%7C%2010-blue)](https://www.microsoft.com/windows)
[![Language](https://img.shields.io/badge/Language-Batchfile-green)](https://en.wikipedia.org/wiki/Batch_file)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

[**英文說明**](README.md)

這是一個輕量且穩定的批次檔腳本，可用於長時間暫停 Windows Update（數月或數年），突破微軟預設的 5 週暫停限制。

## ✨ 功能特色

*   **突破天數限制**：可選擇暫停 **3 個月**、**半年**、**1 年**，或是**指定任意日期**（例如暫停到 2099-12-31）。
*   **自動語言偵測**：自動偵測 Windows 系統語言，並以 **繁體中文**、**簡體中文** 或 **英文** 顯示操作介面。
*   **時區感知 (Timezone Aware)**：正確處理時區轉換。輸入的日期即為「本地時間」的暫停截止日（23:59:59），不會因為時差轉換導致日期提前或延後一天。
*   **執行穩定**：使用暫存檔處理 PowerShell 運算與登錄檔寫入，徹底解決傳統 Batch 腳本因括號語法導致的閃退問題。

## 🚀 使用方式

1.  從 [Releases](../../releases) 頁面下載 `win11-update-pause-tool.bat` 或複製本專案代碼。
2.  對檔案按滑鼠右鍵，選擇 **「以系統管理員身分執行」**（修改登錄檔需要此權限）。
3.  從選單中選擇一個選項：
    *   `[1]` 暫停 3 個月
    *   `[2]` 暫停 6 個月 (半年)
    *   `[3]` 暫停 1 年
    *   `[4]` 指定日期 (輸入格式: YYYY-MM-DD)
4.  等待畫面顯示「執行完成」。
5.  前往 **「設定 > Windows Update」** 查看結果（若未更新顯示，請關閉設定視窗重開）。

## 🛠️ 運作原理

本腳本透過修改以下登錄檔 (Registry) 機碼來運作，路徑位於 `HKLM\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings`：

*   `PauseUpdatesExpiryTime`
*   `PauseFeatureUpdatesEndTime`
*   `PauseQualityUpdatesEndTime`

腳本內建 **PowerShell** 指令，根據您的「本地時間」計算出正確的 ISO 8601 UTC 時間戳記，產生暫存的 `.reg` 檔並匯入，最後自動重啟 `wuauserv` 服務，讓設定立即生效而無需重新開機。

## ⚠️ 免責聲明

*   **安全風險**：無限期暫停更新可能會讓您的電腦暴露在安全風險中。請明智使用此工具，並定期開啟更新以獲取微軟發布的安全性修補程式。
*   **風險自負**：作者不對因修改登錄檔造成的任何系統不穩定或問題負責。

## 📄 授權 (License)

本專案採用 MIT License 授權 - 詳情請參閱 [LICENSE](LICENES) 文件。
