# 🎮 Tetris on ESP32

A highly polished, fully playable **Tetris clone** running on an **ESP32 DevKit V1** with a **128×64 SSD1306 OLED display** rotated into true portrait mode, using an **analog joystick + button** for input. Features perfectly smooth animations, non-blocking background music, EEPROM high-score saving, and a beautifully compact HUD.

**Author:** Chayan Tank

---

## 📸 What It Does

This project brings the classic Tetris experience to the ESP32 with an obsessive focus on performance and visual polish. The 128x64 monochrome OLED is rotated 90 degrees to provide a perfect vertical playing field.

### Features
- **True Portrait Mode:** 64x128 vertical resolution.
- **Classic 10x20 Grid:** 6x6 pixel blocks with a seamless 2-pixel wide solid border.
- **Advanced Raycasting? No!** Just pure, optimized 2D grid logic!
- **Non-Blocking Audio Engine:** Hardware PWM (`ledc`) plays the classic Korobeiniki theme seamlessly. Game sound effects smoothly interrupt the music.
- **Polished Animations:** Sine-wave bouncing intro title, glitching "TV Static" Game Over screen, and 5-frame line clearing disintegration.
- **Ghost Piece & Next Piece Preview:** Uses an ultra-compact `TomThumb` font for the HUD.
- **EEPROM Save Data:** Remembers your high score even when powered off.

---

## 🧰 Components Required

| Component | Quantity | Notes |
|---|---|---|
| ESP32 DevKit V1 (30-pin) | 1 | Any ESP32 with ADC pins on GPIO 34/35 works |
| SSD1306 OLED Display (128×64, I2C) | 1 | I2C address must be `0x3C` |
| Analog Joystick Module (KY-023) | 1 | Has VRx, VRy analog outputs + SW (click) button |
| Passive Buzzer | 1 | Must be *passive* to play melodies! |
| Breadboard | 1 | For prototyping connections |
| Jumper Wires (M-M, M-F) | ~12 | For wiring everything together |

---

## 🔌 Pin Mapping & Wiring

### OLED Display (I2C — SSD1306 128×64)

| OLED Pin | ESP32 Pin | Notes |
|---|---|---|
| **VCC** | **3.3V** | Power supply (do NOT use 5V unless module has a regulator) |
| **GND** | **GND** | Ground |
| **SDA** | **GPIO 21** | I2C Data (ESP32 default SDA) |
| **SCL** | **GPIO 22** | I2C Clock (ESP32 default SCL) |

### Joystick (Analog — KY-023 or similar)

| Joystick Pin | ESP32 Pin | GPIO | Notes |
|---|---|---|---|
| **VRx** (X-axis) | **GPIO 34** | ADC1_CH6 | Analog input for Left/Right |
| **VRy** (Y-axis) | **GPIO 35** | ADC1_CH7 | Analog input for Soft Drop |
| **SW** (Click) | **GPIO 27** | Digital | Hard Drop / Rotate button (uses internal pull-up) |
| **+5V / VCC** | **3.3V** | — | Power (3.3V on ESP32) |
| **GND** | **GND** | — | Ground |

> **Joystick thresholds:** The code reads analog values (0–4095 range on ESP32). Movement is triggered below `1200` or above `2800`.

### Passive Buzzer

| Buzzer Pin | ESP32 Pin | Notes |
|---|---|---|
| **Signal (+)** | **GPIO 14** | PWM output for playing music |
| **GND (-)** | **GND** | Ground |

### Wiring Diagram (Text)

```
ESP32 DevKit V1
┌──────────────────────┐
│                      │
│  3.3V ──────┬────── OLED VCC
│             ├────── Joystick VCC
│             │
│  GND ───────┼────── OLED GND
│             ├────── Joystick GND
│             └────── Buzzer GND
│                      │
│  GPIO 21 (SDA) ───── OLED SDA
│  GPIO 22 (SCL) ───── OLED SCL
│                      │
│  GPIO 34 ──────────── Joystick VRx (X-axis → Left/Right)
│  GPIO 35 ──────────── Joystick VRy (Y-axis → Soft Drop)
│  GPIO 27 ──────────── Joystick SW  (Click → Hard Drop / Rotate)
│                      │
│  GPIO 14 ──────────── Buzzer Signal (+)
│                      │
└──────────────────────┘
```

### Summary Table — All Active Pins

| GPIO | Function | Direction | Type |
|---|---|---|---|
| 21 | I2C SDA (OLED) | Bidirectional | I2C |
| 22 | I2C SCL (OLED) | Output | I2C |
| 34 | Joystick X-axis | Input | Analog (ADC) |
| 35 | Joystick Y-axis | Input | Analog (ADC) |
| 27 | Joystick SW | Input | Digital (Pull-up) |
| 14 | Buzzer Signal | Output | PWM (LEDC) |

---

## 🛠️ Software Setup (PlatformIO)

This project is built using **PlatformIO**, rather than the standard Arduino IDE, to better organize the source files.

1. Install [Visual Studio Code](https://code.visualstudio.com/).
2. Install the **PlatformIO IDE** extension in VSCode.
3. Clone or download this repository.
4. Open the `tetris` folder in VSCode. PlatformIO will automatically detect the `platformio.ini` file and download the required ESP32 toolchains and libraries (Adafruit GFX and Adafruit SSD1306).
5. Connect your ESP32 via USB.
6. Click the **Upload** button (the right-pointing arrow at the bottom of the VSCode window) to compile and flash to the board.

---

## 🎮 Controls

| Input | Action |
|---|---|
| Joystick Left (X-axis < 1200) | Move Piece Left |
| Joystick Right (X-axis > 2800) | Move Piece Right |
| Joystick Down (Y-axis > 2800) | Soft Drop (Accelerate fall) |
| Joystick Up (Y-axis < 1200) | Hard Drop (Slam and lock) |
| Fire Button (GPIO 27) | Rotate Clockwise / Start Game |

> **Note:** The controls feature Delayed Auto Shift (DAS), meaning you can hold left or right to seamlessly slide pieces to the edge of the board.

---

## 📂 Project Structure

```
tetris/
├── platformio.ini    # PlatformIO configuration and library dependencies
├── src/
│   ├── main.cpp      # Main game loop, scene management, rendering orchestrator
│   ├── config.h      # All pin definitions, game tuning constants, display settings
│   ├── display.h/cpp # OLED rendering engine, animations, layout drawing
│   ├── input.h/cpp   # Joystick analog reads & debouncing logic
│   ├── game.h/cpp    # Tetris mechanics, piece logic, line clearing, scoring
│   └── sound.h/cpp   # PWM audio engine, background music playback, SFX interrupts
└── README.md         # This file
```

---

## 📜 Credits & License

- **Tetris** is a registered trademark of The Tetris Company.
- ESP32 port, mechanics, and UI developed by **Chayan Tank**.
- Built using Adafruit GFX and Adafruit SSD1306 libraries.

This project is for **educational and personal use only**. Not affiliated with The Tetris Company.
