# Hey, I'm Yu-I 👋

Y1 Electrical Engineering @ [Hanze University of Applied Sciences](https://www.hanze.nl/), Groningen 🇳🇱

Taiwanese maker who builds embedded systems, designs PCBs, and runs AI trading bots on a Raspberry Pi.

## ⚡ Featured Projects

<table>
<tr>
<td width="50%">

### 🐾 [ThermaPaw Smart Pet Door](https://github.com/Hydr0neFN/smart-pet-door)
**Y1 Capstone · Team Lead**

ESP32-C6 + TMC2130 stepper + LD2410 radar + ToF sensor. Automated pet door with live demo featuring a real dog.

</td>
<td width="50%">

### 🔌 USB-C PD LED Controller PCB V2
**Full KiCad design-to-bringup**

CH224K + AP63205 + ESP32-C3. Salvaged a reversed footprint via firmware GPIO remap instead of respin.

</td>
</tr>
<tr>
<td width="50%">

### 🤖 [Multi-LLM Trading Bot](https://github.com/Hydr0neFN/trader)
**Live on RPi 4B · Paper trading**

Gemini analyst → HuggingFace sentiment → Claude risk gate → Alpaca paper orders. Scans 30 large-caps every 30 min with trailing stops & AI-driven exits.

</td>
<td width="50%">

### 📈 [DOWTrade](https://github.com/Hydr0neFN/DOWTrade)
**MYM Futures · Sim trading**

3-LLM pipeline (Haiku/Gemini/DeepSeek) with hard-coded safety rails, SMA cross filter, sim-fills with pyramid scaling. FastAPI dashboard + LLM-generated daily journal.

</td>
</tr>
<tr>
<td width="50%">

### 🎮 [Reaction Time Duel](https://github.com/Hydr0neFN/ReactionTimeDuel)
**Selected for Hanze Open Day**

ESP32 + ESP8266 over ESP-NOW, NeoPixel, I2S audio, MPU-6050 motion sensing.

</td>
<td width="50%">

### 🌬️ [DucoBox Silent RE](https://github.com/Hydr0neFN/Duco)
**Reverse-engineered RF protocol**

ESP8266 + CC1101 868 MHz → sniffed proprietary RF → Home Assistant integration for ventilation control.

</td>
</tr>
</table>

## 🏠 Smart Home & IoT

Cross-continent Home Assistant + UniFi deployment (Taiwan ↔ Groningen), running on a single RPi 4B with Docker + Cloudflare Tunnel.

| Project | Stack |
|---|---|
| [PCDeskCYD](https://github.com/Hydr0neFN/PCDeskCYD) | ESP32 CYD touchscreen — PC stats, lighting, media control |
| [CO2](https://github.com/Hydr0neFN/CO2) | NeoPixel desk lamp + SCD41 CO₂ + BME280, HomeKit-native |
| [Kitchen](https://github.com/Hydr0neFN/Kitchen) | ESP8266 × 2: LD2410B presence → HomeKit, Sonoff Dual R3 relay |
| [tourplan](https://github.com/Hydr0neFN/tourplan) | Self-hosted group trip date picker for friends |
| [yt-subtitle-translator](https://github.com/Hydr0neFN/yt-subtitle-translator) | Real-time YouTube subtitle translator (DeepL + Google) |

## 🛠 Tech

`ESP32` `ESP8266` `KiCad` `PlatformIO` `C/C++` `Python` `Arduino` `MQTT` `HomeKit` `ESPHome`
`Fusion 360` `3D Printing` `Docker` `Home Assistant` `UniFi` `Cloudflare` `Flask` `FastAPI` `RF 868MHz`

## 📍 Background

🇹🇼 Taiwan → 🇩🇪 Exchange year Germany '21–'22 (Arduino mentorship) → 🇳🇱 Netherlands

APCS certified · VOL-VCA (Dutch supervisor-level safety, 10yr) · IELTS 7.0/C1

---

*Everything runs on a single Raspberry Pi 4B — because why pay for cloud when you have a $35 computer.*
