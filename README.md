# FlexiSpot Standing Desk Controller (Wemos D1 Mini / ESP8266)

This project integrates a **FlexiSpot E8 Standing Desk** (LoctekMotion controller) into **Home Assistant** using **ESPHome**.

It runs on a **Wemos D1 Mini (ESP8266)** and communicates with the desk via the **RJ45** port (UART).

## Hardware & Wiring

### Components
*   **Microcontroller:** Wemos D1 Mini (ESP8266)
*   **Desk:** FlexiSpot E8 (RJ45 Control Port)

### Wiring Diagram (Direct Connection)

| RJ45 Cable Wire | Function | Connect to D1 Mini Pin |
| :--- | :--- | :--- |
| **Pin 8** | +5V Power | **5V** |
| **Pin 7** | Ground | **G** (GND) |
| **Pin 6** | TX (Data from Desk) | **GPIO1 (TX)** |
| **Pin 5** | RX (Data to Desk) | **GPIO3 (RX)** |
| **Pin 4** | Wake/Screen | **D1** (GPIO5) |

**Wait, why are pins swapped?**
By default, the desk's TX pin (Pin 6) should go to the ESP's RX pin (GPIO3). However, in our software configuration (`vish-desk.yaml`), we swapped the UART pins to correct the communication direction.
*   **RJ45 Pin 6 (TX)** -> connected to **GPIO1** on D1 Mini (configured as RX in software)
*   **RJ45 Pin 5 (RX)** -> connected to **GPIO3** on D1 Mini (configured as TX in software)

**Note on Voltage:** Connecting the 5V TX pin from the desk directly to the D1 Mini is commonly done in this specific project without immediate failure, though technically 3.3V is the safe limit.

## Software Configuration

The project uses the `loctek_motion` custom component from [iMicknl/LoctekMotion_IoT](https://github.com/iMicknl/LoctekMotion_IoT).

### Key Settings (`vish-desk.yaml`)
*   **Platform:** `ESP8266` (d1_mini)
*   **UART:** Hardware UART0 with swapped pins. **Logging is disabled on UART** to allow this.
*   **Sensors:** Height sensor (cm), Cover entity for Up/Down/Stop control.
*   **Wake Pin:** GPIO5 is configured as a switch to wake the desk controller.

## Installation / Updates

### First Time (USB)
1.  **Connect** the Wemos D1 Mini to your computer via USB.
2.  **Run:** `esphome run vish-desk.yaml`
3.  **Unplug USB** and install into the desk.

### Future Updates (Wireless / OTA)
1.  Leave the device plugged into the desk.
2.  **Run:** `esphome run vish-desk.yaml`
3.  It will automatically find `vish-desk.local` on your network and update it wirelessly.
