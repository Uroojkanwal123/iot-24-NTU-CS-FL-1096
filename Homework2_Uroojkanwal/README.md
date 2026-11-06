# Homework 2 – Dual-LED PWM Control with OLED Monitoring

**Course:** Embedded IoT Systems  
**Student:** Urooj Kanwal  
**Registration No.:** 24-NTU-CS-FL-1096  


## Project Overview

This project uses an ESP32 to control the brightness of two LEDs independently with Pulse Width Modulation (PWM). Each LED operates at a different PWM frequency and fades smoothly from minimum to maximum brightness and back.

An SSD1306 128×64 I²C OLED displays the current duty-cycle percentage and configured PWM frequency for both LEDs. The same duty-cycle values used to control the LEDs are used for the displayed percentages.

## Project Links

- **GitHub repository:** https://github.com/Uroojkanwal123/iot-24-NTU-CS-FL-1096.git
- **Wokwi simulation:** https://wokwi.com/projects/479142516229467137

## Hardware Components

- ESP32 DevKit (classic ESP32)
- 2 LEDs (red and green)
- 2 × 220 Ω current-limiting resistors
- SSD1306 128×64 I²C OLED display
- Breadboard and jumper wires
- USB cable for power and programming

## Pin Connections

| Component | ESP32 pin | Connection |
|---|---|---|
| LED 1 (red) | GPIO25 | GPIO25 → 220 Ω resistor → LED anode; LED cathode → GND |
| LED 2 (green) | GPIO26 | GPIO26 → 220 Ω resistor → LED anode; LED cathode → GND |
| OLED SDA | GPIO21 | OLED SDA → GPIO21 |
| OLED SCL | GPIO22 | OLED SCL → GPIO22 |
| OLED VCC | 3V3 | OLED VCC → ESP32 3V3 |
| OLED GND | GND | OLED GND → ESP32 GND |

**Important:** Use a separate current-limiting resistor for each LED. Check the OLED module's pin labels before connecting power.

## PWM Configuration

| Setting | LED 1 (red) | LED 2 (green) |
|---|---:|---:|
| GPIO pin | GPIO25 | GPIO26 |
| PWM frequency | 1000 Hz | 5000 Hz |
| PWM resolution | 10 bits | 10 bits |
| Duty-cycle range | 0–1023 | 0–1023 |
| Brightness range | 0–100% | 0–100% |

A 10-bit PWM resolution provides 1024 duty-cycle levels, from 0 to 1023. The LEDs use different PWM frequencies and independent duty-cycle values.

On the classic ESP32, LEDC channels that share a timer must use the same frequency and resolution. Different PWM frequency settings therefore require separate timers. The project uses the Arduino-ESP32 LEDC API and is intended for core 3.x; compatibility code for core 2.x is included in the source described in the assignment.

## How It Works

1. The ESP32 configures each LED as a PWM output.
2. The program gradually changes each LED's duty cycle from 0 to the maximum value.
3. When a duty cycle reaches either limit, its direction reverses to create a continuous fading effect.
4. LED 1 and LED 2 use different PWM frequencies and fade-step values, so their brightness changes independently.
5. The OLED refreshes periodically to show the latest duty-cycle percentages and the configured frequencies.
6. Serial Monitor output provides information about the LEDs' current duty cycles and PWM frequencies.

### Duty Cycle and Brightness

The duty cycle is the percentage of each PWM period during which the output signal is HIGH. A low duty cycle produces lower average LED brightness, while a high duty cycle produces greater average brightness.

### PWM Frequency

PWM frequency determines how quickly the output switches on and off. In this project, LED 1 uses 1000 Hz and LED 2 uses 5000 Hz. For the same duty cycle, the average brightness is generally similar; the main difference is the switching frequency.

## OLED Monitoring

The OLED is an SSD1306 display connected over I²C, using address `0x3C`, SDA on GPIO21, and SCL on GPIO22.

The display shows the live duty-cycle percentage and configured PWM frequency for both LEDs. It is refreshed approximately every 100 ms.

Example layout:

```text
DUAL LED PWM

Red LED:  25%
Freq: 1000 Hz

Green LED: 75%
Freq: 5000 Hz
```

The percentages change while the LEDs fade. The values are calculated from the same duty-cycle variables used by the PWM outputs.

## Software and Libraries

- **Platform:** ESP32 DevKit
- **Framework:** Arduino
- **Build environment:** PlatformIO
- **Simulation:** Wokwi
- **Display libraries:** Adafruit SSD1306 and Adafruit GFX
- **Serial Monitor speed:** 115200 baud

The main source file is `src/main.cpp`. Project configuration is stored in `platformio.ini` and `diagram.json`.

## Project Structure

```text
Homework-2/
├── diagram.json
├── wokwi.toml
├── platformio.ini
├── src/
│   └── main.cpp
├── screenshots/
│   ├── wokwi_circuit.png
│   ├── wokwi_running_oled.png
│   ├── hardware_circuit.jpg
│   └── hardware_oled_output.jpg
└── README.md
```

Make sure the screenshot and photograph filenames in this list match the files actually committed to your repository.

## Testing and Results

Use the Wokwi simulation and, where applicable, the physical circuit to check that:

- Both LEDs fade between low and high brightness.
- The LEDs operate at their configured PWM frequencies: 1000 Hz and 5000 Hz.
- The OLED updates and shows the changing duty-cycle percentages.
- The displayed percentages correspond to the duty-cycle values used by the program.
- The circuit uses a separate 220 Ω resistor for each LED.

The PDF documents both Wokwi simulation and physical-hardware implementation. Add or retain your own clear screenshots and hardware photographs in the `screenshots/` folder as evidence.

## Wokwi Simulation

Open the [Wokwi project](https://wokwi.com/projects/479142516229467137) to view and run the simulation.





## Physical Hardware

The physical implementation uses the same overall design: an ESP32, two LEDs with individual resistors, and an SSD1306 I²C OLED.



### Simulation vs. Physical Hardware

Small differences may appear between simulation and physical hardware because real LEDs vary in brightness, the OLED module's I²C address or pin order may differ, and a phone camera may capture banding from PWM switching. Check the actual wiring and OLED address if the display does not work.

## Build and Upload

1. Open the project folder in Visual Studio Code with PlatformIO installed.
2. Connect the ESP32 using USB.
3. Check that the selected board and serial port are correct.
4. Build the project.
5. Upload the firmware to the ESP32.
6. Open the Serial Monitor at **115200 baud** and check the output.

PlatformIO terminal commands:

```bash
pio run
pio run -t upload
pio device monitor
```

## GitHub Submission

The repository contains the source code, project configuration, simulation link, and documentation for Homework 2.

The assignment specifies these commit messages, in this order:

1. `task1 completed`
2. `complete homework`

Repository: https://github.com/Uroojkanwal123/iot-24-NTU-CS-FL-1096.git

---

**Author:** Urooj Kanwal  
**Course:** Embedded IoT Systems
