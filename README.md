# Conversational-AI-Audio-Guide-Handset

ESP32-C5-based handset for a museum AI audio guide. Captures and cleans visitor speech, streams it over Wi-Fi to an edge AI server, and plays back the synthesized reply.

## Highlights

- ESP32-C5 compute/connectivity platform dual-band (2.4/5 GHz) Wi-Fi 6.
- Codec-integrated DSP for echo cancellation, noise suppression, and VAD.
- Dual-input charging: Auto-arbitrated dual-input charging (Pogo pins + USB-C).
- 12-hour runtime on a single 1S Li-Po cell.
- BOM cost-modeled for 50, 100, and 500 units.
- The handset does no AI inference all compute-heavy work (speech recognition, the conversation model, speech synthesis) is offloaded to the edge server.

## Schematic & PCB

### Schematics
&nbsp;
![Schematic - Top Level](./Documents/Conversational%20AI%20audio%20guide%20handset_page-1.jpg)
*Page 1: Top-Level Hierarchy.*
&nbsp;
![Schematic - Power & Battery](./Documents/Conversational%20AI%20audio%20guide%20handset_page-2.jpg)
*Page 2: Power & Battery Management.*
&nbsp;
![Schematic - MCU & Interface](./Documents/Conversational%20AI%20audio%20guide%20handset_page-3.jpg)
*Page 3: Microcontroller & User Interface.*
&nbsp;
![Schematic - Audio System](./Documents/Conversational%20AI%20audio%20guide%20handset_page-4.jpg)
*Page 4: Audio System.*

---

### PCB Layout

![PCB Layout](./Documents/pcb-layout.png)
*Full PCB layout*

## Power Subsystem

| Component | Role |
|---|---|
| AO3401A | Reverse-polarity protection (Pogo pins) |
| TPS2116DRL |2:1 auto power mux (Pogo / USB-C) |
| BQ25601 | NVDC Li-Po charger (I2C, NTC, Ship-mode) |
| TPS63001 | 3.3V buck-boost regulator |

**Battery sizing:** Sized to a 1.8–2.0 Ah cell to meet a 12-hour runtime target. Average Load: ~110–135 mA (calculated across idle, listening, playback, and Wi-Fi roaming states).  Design Margins: 92% usable DoD and 80% EOL ageing margin.  Recharge: ~0.6–0.8 A for a 3–3.5 hour full charge.

## Audio Subsystem

- **Codec:** Stereo audio codec with an embedded miniDSP. Handles AEC, noise suppression, and VAD on-chip to offload the ESP32-C5 (whose RISC-V core lacks DSP extensions).
- **Microphones:** Dual digital MEMS array (main PCB + satellite PCB). Feeds a shared digital mic bus to provide a spatial reference for far-field noise cancellation. 
- **Amplifier:** Class-D amplifier driving a single 8Ω earpiece at 88–94 dB SPL. Meets hearing-aid-compatibility (HAC) targets.
  
## UI: Status LEDs & Buttons

- **LED Driver:** 8 indicator LEDs (4 battery, 4 status) are driven via a single shift register using only 3 MCU GPIOs (Data/Clock/Latch).
- **Battery UI:** Rear LEDs indicate charge level and active charging status. They remain off to save power and are woken on-demand via a dedicated push-button.
- **Volume Controls:** Multiplexed onto a single ADC input using a resistor-divider network, resolving up/down presses via distinct voltage bands.

## Manufacturing

A preliminary Bill of Materials cost analysis was prepared at 50, 100, and 500-unit production volumes.
## Key Components

| Subsystem | Part |
|---|---|
| MCU / connectivity | ESP32-C5 |
| Audio codec / DSP | TLV320AIC3254 |
| Audio amplifier | TPA2012D2 |
| Battery charger | BQ25601 |
| Power mux | TPS2116DRL |
| Buck-boost regulator | TPS63001 |
| Reverse-polarity protection | AO3401A |
| LED driver | 74HC595 shift register |
