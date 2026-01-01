# Windows 11 Update Pause Tool

[![OS](https://img.shields.io/badge/OS-Windows%2011%20%7C%2010-blue)](https://www.microsoft.com/windows)
[![Language](https://img.shields.io/badge/Language-Batchfile-green)](https://en.wikipedia.org/wiki/Batch_file)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

[**中文說明 (Traditional Chinese)**](README_TW.md)

A lightweight, stable batch script to pause Windows Updates for extended periods (months or years), bypassing the default 5-week limit set by Microsoft.

## ✨ Features

*   **Bypass Limits**: Pause updates for **3 months**, **6 months**, **1 year**, or until any **specific date** you choose (e.g., 2099-12-31).
*   **Auto Language Detection**: The script automatically detects your system language and displays the UI in **English**, **Traditional Chinese**, or **Simplified Chinese**.
*   **Timezone Aware**: Correctly handles local time conversion to UTC. If you input `2026-12-31`, updates will pause exactly until `2026-12-31 23:59:59` in your local time zone (no "next day" errors).
*   **Stable Execution**: Uses temporary files for registry injection and PowerShell calculations to avoid common Batch syntax crashes (parentheses issues).

## 🚀 Usage

1.  **Download** the `PauseUpdate.bat` file from the [Releases](../../releases) page or clone this repository.
2.  **Right-click** the file and select **"Run as Administrator"** (Required to modify registry).
3.  Choose an option from the menu:
    *   `[1]` Pause for 3 Months
    *   `[2]` Pause for 6 Months
    *   `[3]` Pause for 1 Year
    *   `[4]` Custom Date (Input format: YYYY-MM-DD)
4.  Wait for the "Completed" message.
5.  Verify the result in **Settings > Windows Update**.

## 🛠️ How it Works

This script modifies the Windows Registry keys under `HKLM\SOFTWARE\Microsoft\WindowsUpdate\UX\Settings`:

*   `PauseUpdatesExpiryTime`
*   `PauseFeatureUpdatesEndTime`
*   `PauseQualityUpdatesEndTime`

It uses embedded **PowerShell** commands to calculate the correct ISO 8601 UTC timestamp based on your local time input, generates a temporary `.reg` file to safely import the settings, and restarts the `wuauserv` service to apply changes immediately without a reboot.

## ⚠️ Disclaimer

*   **Security Risks**: Pausing updates indefinitely may leave your system vulnerable to security threats. Use this tool wisely and enable updates regularly to get security patches.
*   **Use at your own risk**: The author is not responsible for any system instability or issues caused by modifying the registry.

## 📄 License

This project is licensed under the MIT License - see the LICENES file for details.
