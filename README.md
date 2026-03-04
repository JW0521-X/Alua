# Alua
# 🚀 Alua (AOS Lua-Engine)
> **A High-Performance, Register-based Scripting Engine for UEFI Environments.**

Alua 是一個專為 **AOS (Advanced Operating System)** 啟動環境開發的輕量化腳本引擎。它保留了 Lua 簡潔的語法特性，但針對底層韌體環境進行了深度重構與優化。

---

## 💎 核心技術特性

* **Firmware-Native ABI**: 核心完全基於 **UEFI MS ABI** 構建，與 `gnu-efi` 深度整合，無需任何標準 C 庫 (LibC) 支持。
* **Pure Integer VM**: 為確保在 EFI 環境下的絕對穩定性，Alua 移除所有浮點運算，採用 **64-bit 純整數 (INT64)** 核心，徹底解決 FPU/SSE 觸發的 #GP 異常。
* **Register-based Architecture**: 採用類似 Lua 5.x 的暫存器式虛擬機架構，減少棧操作開銷，指令執行更流暢。
* **Global Symbol Bridge (_G)**: 內建全域符號表，支持將 UEFI Boot Services 函數註冊為腳本指令。

---

## 🏗 指令集架構 (ISA)

Alua 採用 32-bit 定長指令格式，確保在內存受限環境下的解碼效率：

| 分類 | 指令 (OpCodes) | 說明 |
| :--- | :--- | :--- |
| **Data** | `LOADK`, `MOVE`, `GETGLOBAL` | 常量加載、寄存器移動、全域變數獲取 |
| **Math** | `ADD`, `SUB`, `MUL`, `DIV` | 純整數算術運算 |
| **Flow** | `JMP`, `EQ`, `LT`, `TEST` | 跳轉與邏輯判斷 (支持 `if-else-fi`) |
| **System**| `PRINT`, `CALL` | 系統輸出與函數調用 |



---

## 📂 專案結構

* `alua.h`: 引擎核心定義與指令集。
* `alua.c`: 虛擬機核心循環 (VM Loop) 實作。
* `ALapi.c`: 寄存器操作與全域表管理 API。
* `aluac-host.c`: 適用於 Host OS (MacOS/Linux) 的原始碼編譯器。
* `Makefile`: 自動化構建工具。

---

## 🚀 快速開始

### 1. 編譯編譯器 (Host 端)
在您的開發主機 (MacOS/Linux) 上編譯 `aluac`：
```bash
gcc aluac-host.c -o aluac
