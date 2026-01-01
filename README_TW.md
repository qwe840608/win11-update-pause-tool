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

1.  從 [Releases](../../releases) 頁面下載 `win11-update-pause-tool.bat` 或複製[本專案代碼](win11-update-pause-tool.bat)。

```batch
@echo off
setlocal EnableDelayedExpansion

:: ---------------------------------------------------------
:: 偵測系統語言
:: ---------------------------------------------------------
set "TempLang=%TEMP%\sys_lang.txt"
powershell -NoProfile -Command "(Get-Culture).Name" > "%TempLang%" 2>nul
set /p SysLang=<"%TempLang%"
del "%TempLang%" >nul 2>&1

:: 預設英文，根據語言碼設定變數
set "LANG=EN"
if "%SysLang:~0,2%"=="zh" (
    if "%SysLang%"=="zh-TW" set "LANG=ZH_TW"
    if "%SysLang%"=="zh-HK" set "LANG=ZH_TW"
    if "%SysLang%"=="zh-CN" set "LANG=ZH_CN"
)
if "%SysLang:~0,2%"=="ja" set "LANG=JA"

:: ---------------------------------------------------------
:: 設定各語言文字
:: ---------------------------------------------------------
if "%LANG%"=="ZH_TW" (
    set "TXT_TITLE=Windows 11 Update 暫停工具"
    set "TXT_MENU_1=請選擇暫停時間："
    set "TXT_OPT_1=暫停 3 個月"
    set "TXT_OPT_2=暫停 6 個月 (半年)"
    set "TXT_OPT_3=暫停 1 年"
    set "TXT_OPT_4=指定日期 (範例: 2100-12-31)"
    set "TXT_OPT_0=取消並退出"
    set "TXT_INPUT=請輸入選項 (0-4): "
    set "TXT_CANCEL=已取消。"
    set "TXT_CALC_3M=正在計算 3 個月後的日期..."
    set "TXT_CALC_6M=正在計算 6 個月後的日期..."
    set "TXT_CALC_1Y=正在計算 1 年後的日期..."
    set "TXT_INPUT_DATE=請輸入日期 (YYYY-MM-DD，範例: 2100-12-31): "
    set "TXT_ERR_PERM=請以系統管理員身分執行此檔案！"
    set "TXT_ERR_INVALID=無效的選項！"
    set "TXT_ERR_NODATE=未輸入日期！"
    set "TXT_ERR_FORMAT=日期格式不正確！請使用 YYYY-MM-DD 格式。"
    set "TXT_CALC_DONE=計算完成: "
    set "TXT_PREP=正在準備登錄檔參數..."
    set "TXT_START=開始時間: "
    set "TXT_END=結束時間: "
    set "TXT_DISPLAY=顯示日期: "
    set "TXT_LOCAL= (台灣時間)"
    set "TXT_BUILD_REG=正在建立登錄檔..."
    set "TXT_REG_CREATED=登錄檔已建立: "
    set "TXT_IMPORTING=正在匯入登錄檔..."
    set "TXT_SUCCESS=登錄檔已成功匯入！"
    set "TXT_FAIL=登錄檔匯入失敗 (錯誤碼: "
    set "TXT_RESTART=正在重啟 Windows Update 服務..."
    set "TXT_CLEAN=正在清理暫存檔..."
    set "TXT_CLEANED=暫存檔已刪除。"
    set "TXT_COMPLETE=執行完成！"
    set "TXT_SELECTED=已選擇: "
    set "TXT_PAUSED=Windows Update 已暫停至: "
    set "TXT_CHECK=請關閉並重新開啟「設定 ^> Windows Update」確認。"
) else if "%LANG%"=="ZH_CN" (
    set "TXT_TITLE=Windows 11 Update 暂停工具"
    set "TXT_MENU_1=请选择暂停时间："
    set "TXT_OPT_1=暂停 3 个月"
    set "TXT_OPT_2=暂停 6 个月 (半年)"
    set "TXT_OPT_3=暂停 1 年"
    set "TXT_OPT_4=指定日期 (示例: 2100-12-31)"
    set "TXT_OPT_0=取消并退出"
    set "TXT_INPUT=请输入选项 (0-4): "
    set "TXT_CANCEL=已取消。"
    set "TXT_CALC_3M=正在计算 3 个月后的日期..."
    set "TXT_CALC_6M=正在计算 6 个月后的日期..."
    set "TXT_CALC_1Y=正在计算 1 年后的日期..."
    set "TXT_INPUT_DATE=请输入日期 (YYYY-MM-DD，示例: 2100-12-31): "
    set "TXT_ERR_PERM=请以管理员身份运行此文件！"
    set "TXT_ERR_INVALID=无效的选项！"
    set "TXT_ERR_NODATE=未输入日期！"
    set "TXT_ERR_FORMAT=日期格式不正确！请使用 YYYY-MM-DD 格式。"
    set "TXT_CALC_DONE=计算完成: "
    set "TXT_PREP=正在准备注册表参数..."
    set "TXT_START=开始时间: "
    set "TXT_END=结束时间: "
    set "TXT_DISPLAY=显示日期: "
    set "TXT_LOCAL= (本地时间)"
    set "TXT_BUILD_REG=正在创建注册表..."
    set "TXT_REG_CREATED=注册表已创建: "
    set "TXT_IMPORTING=正在导入注册表..."
    set "TXT_SUCCESS=注册表已成功导入！"
    set "TXT_FAIL=注册表导入失败 (错误码: "
    set "TXT_RESTART=正在重启 Windows Update 服务..."
    set "TXT_CLEAN=正在清理临时文件..."
    set "TXT_CLEANED=临时文件已删除。"
    set "TXT_COMPLETE=执行完成！"
    set "TXT_SELECTED=已选择: "
    set "TXT_PAUSED=Windows Update 已暂停至: "
    set "TXT_CHECK=请关闭并重新打开「设置 ^> Windows Update」确认。"
) else (
    set "TXT_TITLE=Windows 11 Update Pause Tool"
    set "TXT_MENU_1=Select pause duration:"
    set "TXT_OPT_1=Pause for 3 months"
    set "TXT_OPT_2=Pause for 6 months"
    set "TXT_OPT_3=Pause for 1 year"
    set "TXT_OPT_4=Custom date (Example: 2100-12-31)"
    set "TXT_OPT_0=Cancel and exit"
    set "TXT_INPUT=Enter option (0-4): "
    set "TXT_CANCEL=Cancelled."
    set "TXT_CALC_3M=Calculating date 3 months ahead..."
    set "TXT_CALC_6M=Calculating date 6 months ahead..."
    set "TXT_CALC_1Y=Calculating date 1 year ahead..."
    set "TXT_INPUT_DATE=Enter date (YYYY-MM-DD, Example: 2100-12-31): "
    set "TXT_ERR_PERM=Please run this file as Administrator!"
    set "TXT_ERR_INVALID=Invalid option!"
    set "TXT_ERR_NODATE=No date entered!"
    set "TXT_ERR_FORMAT=Invalid date format! Use YYYY-MM-DD format."
    set "TXT_CALC_DONE=Calculated: "
    set "TXT_PREP=Preparing registry parameters..."
    set "TXT_START=Start time: "
    set "TXT_END=End time: "
    set "TXT_DISPLAY=Display date: "
    set "TXT_LOCAL= (Local time)"
    set "TXT_BUILD_REG=Creating registry file..."
    set "TXT_REG_CREATED=Registry file created: "
    set "TXT_IMPORTING=Importing registry..."
    set "TXT_SUCCESS=Registry imported successfully!"
    set "TXT_FAIL=Registry import failed (Error code: "
    set "TXT_RESTART=Restarting Windows Update service..."
    set "TXT_CLEAN=Cleaning temporary files..."
    set "TXT_CLEANED=Temporary files deleted."
    set "TXT_COMPLETE=Completed!"
    set "TXT_SELECTED=Selected: "
    set "TXT_PAUSED=Windows Update paused until: "
    set "TXT_CHECK=Please close and reopen Settings ^> Windows Update to confirm."
)

:: ---------------------------------------------------------
:: 檢查管理員權限
:: ---------------------------------------------------------
net session >nul 2>&1
if %errorLevel% neq 0 (
    echo [ERROR] %TXT_ERR_PERM%
    echo.
    pause
    exit
)

:: ---------------------------------------------------------
:: 顯示選單
:: ---------------------------------------------------------
:MENU
cls
echo ========================================
echo  %TXT_TITLE%
echo ========================================
echo.
echo  %TXT_MENU_1%
echo.
echo  [1] %TXT_OPT_1%
echo  [2] %TXT_OPT_2%
echo  [3] %TXT_OPT_3%
echo  [4] %TXT_OPT_4%
echo.
echo  [0] %TXT_OPT_0%
echo.
echo ========================================
set /p "Choice=%TXT_INPUT%"

:: ---------------------------------------------------------
:: 處理選擇
:: ---------------------------------------------------------
if "%Choice%"=="0" (
    echo %TXT_CANCEL%
    pause
    exit
)

echo.

if "%Choice%"=="1" (
    echo %TXT_CALC_3M%
    set "DisplayText=%TXT_OPT_1%"
    set "Months=3"
    goto CALC_MONTHS
)

if "%Choice%"=="2" (
    echo %TXT_CALC_6M%
    set "DisplayText=%TXT_OPT_2%"
    set "Months=6"
    goto CALC_MONTHS
)

if "%Choice%"=="3" (
    echo %TXT_CALC_1Y%
    set "DisplayText=%TXT_OPT_3%"
    set "Years=1"
    goto CALC_YEARS
)

if "%Choice%"=="4" (
    set /p "TargetDate=%TXT_INPUT_DATE%"
    set "DisplayText=%TXT_OPT_4%"
    if "!TargetDate!"=="" (
        echo [ERROR] %TXT_ERR_NODATE%
        timeout /t 2 /nobreak >nul
        goto MENU
    )
    goto BUILD_REG
)

echo [ERROR] %TXT_ERR_INVALID%
timeout /t 2 /nobreak >nul
goto MENU

:: ---------------------------------------------------------
:: 計算月份
:: ---------------------------------------------------------
:CALC_MONTHS
set "TempCalc=%TEMP%\calc_date.txt"
powershell -NoProfile -Command "(Get-Date).AddMonths(%Months%).ToString('yyyy-MM-dd')" > "%TempCalc%" 2>nul
set /p TargetDate=<"%TempCalc%"
del "%TempCalc%" >nul 2>&1
echo %TXT_CALC_DONE%%TargetDate%
goto BUILD_REG

:: ---------------------------------------------------------
:: 計算年份
:: ---------------------------------------------------------
:CALC_YEARS
set "TempCalc=%TEMP%\calc_date.txt"
powershell -NoProfile -Command "(Get-Date).AddYears(%Years%).ToString('yyyy-MM-dd')" > "%TempCalc%" 2>nul
set /p TargetDate=<"%TempCalc%"
del "%TempCalc%" >nul 2>&1
echo %TXT_CALC_DONE%%TargetDate%
goto BUILD_REG

:: ---------------------------------------------------------
:: 建立 .reg 檔案並執行
:: ---------------------------------------------------------
:BUILD_REG
echo.
echo %TXT_PREP%

:: 取得開始時間 (UTC)
set "TempStart=%TEMP%\start_time.txt"
powershell -NoProfile -Command "(Get-Date).ToUniversalTime().ToString('yyyy-MM-ddTHH:mm:ssZ')" > "%TempStart%" 2>nul
set /p StartDateTime=<"%TempStart%"
del "%TempStart%" >nul 2>&1

:: 取得結束時間 (本地時間 23:59:59 轉 UTC)
set "TempEnd=%TEMP%\end_time.txt"
powershell -NoProfile -Command "try { $d = [DateTime]::ParseExact('%TargetDate%', 'yyyy-MM-dd', $null).AddHours(23).AddMinutes(59).AddSeconds(59); $d.ToUniversalTime().ToString('yyyy-MM-ddTHH:mm:ssZ') } catch { 'ERROR' }" > "%TempEnd%" 2>nul
set /p EndDateTime=<"%TempEnd%"
del "%TempEnd%" >nul 2>&1

:: 檢查日期是否有效
if "%EndDateTime%"=="ERROR" (
    echo [ERROR] %TXT_ERR_FORMAT%
    echo.
    pause
    goto MENU
)

echo %TXT_START%%StartDateTime%
echo %TXT_END%%EndDateTime%
echo %TXT_DISPLAY%%TargetDate% 23:59:59%TXT_LOCAL%

:: 建立 .reg 檔
set "TempReg=%TEMP%\pause_update.reg"
echo.
echo %TXT_BUILD_REG%

(
echo Windows Registry Editor Version 5.00
echo.
echo [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings]
echo "PauseUpdatesStartTime"="%StartDateTime%"
echo "PauseUpdatesExpiryTime"="%EndDateTime%"
echo "PauseFeatureUpdatesStartTime"="%StartDateTime%"
echo "PauseFeatureUpdatesEndTime"="%EndDateTime%"
echo "PauseQualityUpdatesStartTime"="%StartDateTime%"
echo "PauseQualityUpdatesEndTime"="%EndDateTime%"
) > "%TempReg%"

echo %TXT_REG_CREATED%%TempReg%

:: 執行 .reg 檔
echo.
echo %TXT_IMPORTING%
reg import "%TempReg%" >nul 2>&1

if %errorLevel% equ 0 (
    echo [SUCCESS] %TXT_SUCCESS%
) else (
    echo [WARNING] %TXT_FAIL%%errorLevel%)
    echo Temp file: %TempReg%
    pause
    exit
)

:: 重啟 Windows Update 服務
echo.
echo %TXT_RESTART%
net stop wuauserv >nul 2>&1
timeout /t 2 /nobreak >nul
net start wuauserv >nul 2>&1

:: 刪除暫存 .reg 檔
echo.
echo %TXT_CLEAN%
del "%TempReg%" >nul 2>&1
echo %TXT_CLEANED%

:: 顯示完成訊息
echo.
echo ========================================
echo  %TXT_COMPLETE%
echo ========================================
echo.
echo  %TXT_SELECTED%%DisplayText%
echo  %TXT_PAUSED%%TargetDate% 23:59:59
echo.
echo  %TXT_CHECK%
echo.
pause
exit

```
3.  對檔案按滑鼠右鍵，選擇 **「以系統管理員身分執行」**（修改登錄檔需要此權限）。
4.  從選單中選擇一個選項：
    *   `[1]` 暫停 3 個月
    *   `[2]` 暫停 6 個月 (半年)
    *   `[3]` 暫停 1 年
    *   `[4]` 指定日期 (輸入格式: YYYY-MM-DD)
5.  等待畫面顯示「執行完成」。
6.  前往 **「設定 > Windows Update」** 查看結果（若未更新顯示，請關閉設定視窗重開）。

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
