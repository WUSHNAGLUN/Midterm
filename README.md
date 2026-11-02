# Midterm
# STM32G474RE Ultrasonic Radar & Smart Warning System

這是一個基於 **STM32G474RE (NUCLEO-G474RE)** 開發板實作的智慧安全警報與雷達系統。結合了超音波測距自動偵測與按鈕手動切換雙模式，透過三種狀態（Safe / Warning / Danger）驅動對應的 LED 燈號與蜂鳴器警報。

---

## 🛠️ 硬體架構與腳位對照 (Pinout)

| 裝置 / 感測器 | STM32 腳位 | 說明 |
| :--- | :--- | :--- |
| **綠燈 (Safe LED)** | `PB4` | 安全狀態 (Active-High 點亮) |
| **黃燈 (Warning LED)** | `PC7` | 預警狀態 (慢閃) |
| **紅燈 (Danger LED)** | `PC6` | 危險狀態 (快閃) |
| **有源蜂鳴器** | `PB0` | 依據危險程度發出對應頻率警報 |
| **使用者按鈕** | `PC13` | NUCLEO 板載 B1 按鈕 (EXTI 外部中斷) |
| **超音波 Trig** | `PA5` | 10us 觸發訊號輸出 |
| **超音波 Echo** | `PA1` | 接收高電位持續時間 (TIM2 微秒計時) |
| **UART 序列埠** | `USART2` | 鮑率 115200 (系統除錯與距離回報) |

---

## ⚙️ 系統功能與運作邏輯

系統支援 **自動雷達模式** 與 **手動測試模式** 雙向切換：

1. **自動超音波模式 (預設)**：
   * **Safe (> 30 cm)**：綠燈恆亮，蜂鳴器靜音。
   * **Warning (10 cm - 30 cm)**：黃燈 300ms 慢閃，蜂鳴器慢速逼逼。
   * **Danger (< 10 cm)**：紅燈 200ms 急促快閃，蜂鳴器急促狂叫。

2. **手動按鈕切換模式**：
   * 點擊板載藍色按鈕（`PC13`）可切換為手動模式，直接循環切換 **Safe $\rightarrow$ Warning $\rightarrow$ Danger** 狀態，方便展示與測試。

---

## 📂 專案結構
- `Core/Src/main.c`：主程式、狀態機邏輯、超音波測距演算法、硬體初始化。
- `Core/Inc/main.h`：標頭檔與巨集定義。
- `Drivers/`：STM32G4xx HAL 驅動程式庫。

---

## 🚀 快速開始 (Getting Started)
1. 使用 **STM32CubeIDE** 開啟此專案。
2. 確認 `.ioc` 腳位設定正確（或直接編譯現有 `main.c`）。
3. 連接 ST-LINK 與硬體線路，點擊 **Run** 燒錄至 NUCLEO-G474RE 開發板。
4. 打開 Serial Monitor（Baud Rate: `115200`）即可檢視即時偵測數據。w
