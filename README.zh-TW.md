[English](README.md) · **繁體中文**

# 嘿，我是 Yu-I 👋

電機工程大一 @ [Hanze 應用科學大學](https://www.hanze.nl/)，Groningen 🇳🇱
**VOL-VCA**（荷蘭主管級安全證照）· APCS · IELTS 7.0 / C1

台灣 maker。做嵌入式韌體、KiCad 繪板到 bringup，以及橫跨兩國、
跑在 Raspberry Pi 上的小型自託管服務群。這裡多數的專案，起點都是
某樣東西壞了，而我想弄清楚到底為什麼。

## 🔍 診斷日誌

一個症狀、一項推翻表面解釋的量測，接著是根本原因。
若要了解我，這些儲存庫是我會最先指給你看的。

### [搞定接觸不良的 USB SSD](https://github.com/Hydr0neFN/rpi4-usb-ssd-resilience)

只要桌子被撞到一下，Pi 4 就會當機。問題有四個，而且只有第一個是顯而易見的：
root 位於抽取式磁碟上；UAS 綁定到未設定 quirks 的 RTL9210B-CG 橋接晶片；
USB3 鏈結電源管理在每次開機時都失敗（`enable of device-initiated U1
failed`）；而且日誌裡完全沒有任何線索可供分析。

這第四點正是這個儲存庫的核心所在。**沒有證據，而且這是結構性問題** ——
持久性 journal 是從 *SSD 上的* 目錄 bind-mount 出來的，而
`/var/log` 則是 50 MB 的 tmpfs。當磁碟斷線時，記錄斷線的日誌也隨之陪葬。
`journalctl --list-boots` 只顯示單次開機。幾個月以來的當機，鑑識資料為零。

現在可在 22 秒內自動復原。而為了驗證機制是否奏效，又抓出了三個 bug，
其中兩個原本足以讓機器徹底癱瘓且無法連線。

### [羅勒種植箱](https://github.com/Hydr0neFN/basil-growbox)

一台 ESP8266 每小時重啟數次 —— 與此同時，**在每一次故障期間 ping 卻始終能在 1–2 ms 內回應。**
ICMP 是由 lwIP 回應，而非 sketch 的主迴圈，因此能回 ping 的裝置
依然可能完全無法建立連線。「它有回應」從來就不是充分的證據，
而我過去卻一直把它當成是。

三種原本信心滿滿的推論在證據面前破滅：slot contention、connection churn 與
power sag —— 最後一項藉由量測到 3V3 供電軌穩定維持在 3.29 V 而排除。
觸發條件現在已經確立：23 小時內零次當機，武裝第二個 API 客戶端後三分鐘內三次，
停掉它之後就沒有了。

而我寫錯的那部分，才是值得讀的部分。我當時斷定記憶體已被排除，依據是記錄裡
某一行顯示當機時還有 11.6 kB 連續區塊。但那一行不是快照——記錄器把一個新鮮的
讀值和一個過期的讀值配在一起——所以**我真正需要的那個數字，從來沒有被量到過。**
觸發條件站得住，機制仍然開放，而它現在就是照這樣寫的。
起因是在已有連線的情況下，第二個 API client 又發起了交握 ——
`reset_reason` 回傳 `Exception`，是韌體層級的錯誤，
不是 watchdog bite，也不是 brownout。

為裝置加入觀測機制消耗了約 655 B，相對於僅剩 3–4 kB 的可用 heap ——
佔了裕度將近五分之一。量測極限的同時，也推移了極限。

### [死掉的四分之一](https://github.com/Hydr0neFN/ili9341-dead-quarter)

一片廉價 2.8 吋 ILI9341 面板有四分之一永遠不會更新，但螢幕上的自我測試
卻全都回報 **PASS** —— 因為測試程式畫進哪個區域，就從哪個區域讀回。

此 demo 以**同一塊板子上的同一份原始碼檔案**產出兩套 PlatformIO 建置，
一好一壞，將差異精確限縮至建置組態，而非面板、
接線或函式庫版本。

### [固定 IP 真的能降低 ping 嗎？](https://github.com/Hydr0neFN/hinet-dual-path-probe)

在台灣同一條實體線路上使用兩種 ISP 帳號類型，由該線路上的 Pi 同步量測，
走的是 **Source 2 遊戲實際使用的 UDP 路徑**，而非隨便挑一個附近方便的
DNS 伺服器。

答案是不會，但也是會。兩個帳號的遊戲中位數 ping **完全相同**。
所有的差異全在長尾：浮動 IP 帳號每 6 個取樣點就有一個飆破 60 ms，
相較之下固定 IP 帳號是 868 個才有一個。探針每小時更新發布的資料。

## ⚡ 嵌入式與機電整合

<table>
<tr>
<td width="50%">

### 🐾 [ThermaPaw 智慧寵物門](https://github.com/Hydr0neFN/smart-pet-door)
**大一 Capstone · 組長**

ESP32-C6 + TMC2130 步進馬達 + LD2410 雷達 + ToF。以具備保溫密封的馬達驅動門
取代被動門片，減少空調流失。現場實機展示，由真正的狗實測。

</td>
<td width="50%">

### 🔌 [USB-C PD LED 控制器 PCB V2](https://github.com/Hydr0neFN/LightController)
**完整 KiCad 設計到 bringup**

CH224K PD sink + AP63205 buck + ESP32-C3。從電路圖、Layout、洗板打樣到 bringup，
透過智慧家庭系統驅動 LED 燈條。

</td>
</tr>
<tr>
<td width="50%">

### 🌬️ [DucoBox Silent 逆向工程](https://github.com/Hydr0neFN/Duco)
**RF 逆向工程 · 已封存**

ESP8266 + CC1101 at 868 MHz。側錄到通風主機的專有流量後，
在加入程序的交握上撞到輪替金鑰，始終沒能取得控制權，這個版本也就收了。
留著是因為這個否定的結果才是有用的部分。

</td>
<td width="50%">

### 🌿 [羅勒種植箱](https://github.com/Hydr0neFN/basil-growbox)
**ESPHome · Home Assistant**

土壤濕度、超音波水箱液位、排水故障閂鎖與防溢水連鎖。鋁箔防蟲隔離層採用
打孔而非全密閉設計；而面對「這會把土壤悶死」的質疑，是用實際量測來回答，
而不是靠一廂情願。

</td>
</tr>
</table>

另外還有：[ReactionTimeDuel 瞬時對決](https://github.com/Hydr0neFN/ReactionTimeDuel) —— 四人座配備無線搖桿的
ESP-NOW 反應速度對決遊戲，入選 Hanze Open Day 展示。

## 🏠 智慧家庭 & IoT

跨越兩大洲的多站點 Home Assistant + UniFi 部署，以 Docker 自託管於
單板電腦上。

| 專案 | 技術堆疊 |
|---|---|
| [PCDeskCYD](https://github.com/Hydr0neFN/PCDeskCYD) | ESP32 CYD 觸控螢幕 —— 電腦狀態、燈光、媒體控制 |
| [CO2](https://github.com/Hydr0neFN/CO2) | NeoPixel 檯燈 + SCD41 CO₂ + BME280，原生 HomeKit |
| [Kitchen](https://github.com/Hydr0neFN/Kitchen) | ESP8266 × 2：LD2410B 人體存在雷達 → HomeKit + 繼電器 |
| [tourplan](https://github.com/Hydr0neFN/tourplan) | 自託管團體出遊挑日工具，專為最不諳 3C 的親戚量身打造 |
| [yt-subtitle-translator](https://github.com/Hydr0neFN/yt-subtitle-translator) | 即時 YouTube 字幕翻譯器（DeepL + Google） |

## 🤖 AI 工具鏈

我將 LLM 當成結構化思考夥伴，而非僅僅是程式碼產生器 —— 串接為
辯論者、執行者與審查者，並在彼此之間設置檢驗機制。真正有趣的並非提示詞
（prompting），而是圍繞在外的 harness。

| 專案 | 功能說明 |
|---|---|
| [claude-bridges](https://github.com/Hydr0neFN/claude-bridges) | 讓單一 agent 能向其他 agent 諮詢，以進行結構化、依角色分工審查的 MCP 橋接工具 |
| [solver-verified-bench](https://github.com/Hydr0neFN/solver-verified-bench) | 一套 LLM 基準測試：標準答案即為資料、逾時一律算作失敗，且各項極限皆有明載 |
| [claude-memory-web](https://github.com/Hydr0neFN/claude-memory-web) | 自託管記憶庫的瀏覽器 UI —— 無需建置步驟、ETag 衝突 diff、支援 git 歷史紀錄 |
| [trader](https://github.com/Hydr0neFN/trader) · [DOWTrade](https://github.com/Hydr0neFN/DOWTrade) | 兩款模擬交易機器人：多模型管線受控於 Python 硬編碼安全護欄，並與確定性對照組進行量測比較 |

## 🛠 技術

**嵌入式** · `ESP32` `ESP8266` `Arduino` `PlatformIO` `ESPHome` `KiCad` `RF 868MHz` `MQTT`
**軟體** · `C/C++` `Python` `Flask` `FastAPI` `Docker` `HomeKit`
**設計 & 基礎設施** · `Fusion 360` `3D Printing` `Home Assistant` `UniFi` `Cloudflare`

## 📍 背景

🇹🇼 台灣 → 🇩🇪 德國交換一年 '21–'22 → 🇳🇱 荷蘭

## 📊 附錄 —— 即時遙測

兩款交易機器人會在下方發布各自的運作數字。Pi 上的 cron job 會定期改寫此
區塊，因此呈現的內容永遠是最新一次執行的結果。

<!-- LIVE_STATS:START -->
> **即時數據** · 更新於 2026-09-18 17:15 ET · *由 RPi cron 自動產生*
>
> | | Trader (Alpaca 模擬) | DOWTrade (MYM 模擬) |
> |---|---|---|
> | 淨值 | $105,111.44 | $1,001,479.63 |
> | 報酬率 | +5.11% | +5.92% |
> | 淨損益 | — | $+1,479.63 |
> | 持倉 | 20/20 | 空倉 |
> | 當日損益 | $-844.90 | — |
> | 總交易次數 | — | 88 |
> | 漲幅前三 | AMD +10.4%, META +8.4%, TMO +7.2% | |
> | 跌幅前三 | NOW -3.3%, KO -1.8%, MSFT -1.8% | |
>
> DOWTrade 報酬率以 $25,000 風險資本計算，而非模擬帳戶的 $1M 券商預設值 — 每筆風險預算僅 $250，對一百萬報價沒有意義。
> 對照組：同一批 15m bar、同樣的 2×ATR 停損與部位大小，只把 LLM 決策換成 EMA(9/21) 交叉 → **$-4,250.47 / 225 筆**（勝率 24.0%，平均 -0.044R）。把 LLM 版本用**同一套成本模型**重算（每筆一跳逆向滑價 + 手續費）為 **$+1,082.63 / 88 筆**。上表的淨損益是歷史帳載值，未含成本模型 — 成本模型自 2026-09-09 起才對新倉位生效。
> 兩邊樣本都遠不足以達到統計顯著；這是方向，不是結論。

![權益曲線](equity_chart.svg)

<!-- LIVE_STATS:END -->

---

*幾乎全部自託管於一台單板電腦 — 有一台 $35 的機器能跑，幹嘛付雲端的錢。*
