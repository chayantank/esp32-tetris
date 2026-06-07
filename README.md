# ESP32 Portable Tetris 🎮

A highly polished, feature-complete Tetris clone built specifically for the ESP32 and an SSD1306 OLED display. This project has been optimized for **true portrait mode** gameplay, featuring smooth animations, non-blocking audio, EEPROM high-score saving, and ultra-compact HUD designs.

## ✨ Features
* **Portrait Mode:** The 128x64 OLED is rotated 90 degrees to provide a classic 64x128 vertical playing field perfectly suited for Tetris.
* **Non-Blocking Audio Engine:** Hardware PWM (`ledc`) is used to play the classic Korobeiniki theme song seamlessly in the background. Gameplay sound effects (rotate, hard drop, clear line) smoothly interrupt the music without ever freezing the game logic.
* **Polished Animations:**
  * Sine-wave bouncing "TETRIS" title screen with falling background particles.
  * Glitching "TV Static" Game Over screen with shaking text.
  * Smooth 5-frame disintegration animation when clearing lines.
* **Advanced Mechanics:** 
  * "Ghost Piece" system to preview where pieces will land.
  * "Next Piece" preview box using a custom ultra-miniature `TomThumb` font.
  * DAS (Delayed Auto Shift) and precise joystick debouncing for perfectly snappy controls.
* **Persistent High Scores:** Saves your best score directly to the ESP32's non-volatile memory using the `Preferences` library.

## 🛠️ Hardware Requirements
* **ESP32 Development Board** (NodeMCU-32S, ESP32-WROOM, etc.)
* **SSD1306 I2C OLED Display** (128x64 resolution)
* **Analog Joystick Module** (with X/Y axes and push button)
* **Passive Buzzer** (must be passive to play melodies!)
* Jumper wires & Breadboard

## 🔌 Pin Connections

| Component | Pin Name | ESP32 Pin | Notes |
| :--- | :--- | :--- | :--- |
| **OLED (I2C)** | SDA | **GPIO 21** | Default ESP32 I2C Data |
| | SCL | **GPIO 22** | Default ESP32 I2C Clock |
| | VCC | **3.3V** | Do not use 5V unless module has a regulator |
| | GND | **GND** | |
| **Joystick** | VRx | **GPIO 34** | Analog input for horizontal movement |
| | VRy | **GPIO 35** | Analog input for vertical movement (Hard Drop/Soft Drop) |
| | SW | **GPIO 27** | Digital input for rotation (uses internal pull-up) |
| | VCC | **3.3V** | |
| | GND | **GND** | |
| **Buzzer** | Signal (+) | **GPIO 14** | PWM output for melodies |
| | GND (-) | **GND** | |

## 🚀 Installation & Build
This project is built using **PlatformIO**.

1. Clone or download this repository.
2. Open the folder in VSCode with the PlatformIO extension installed.
3. The `platformio.ini` file is already configured for the `esp32dev` board. 
4. The following libraries will be automatically downloaded by PlatformIO:
   * `adafruit/Adafruit GFX Library`
   * `adafruit/Adafruit SSD1306`
5. Connect your ESP32 via USB and click **Upload** in PlatformIO.

## 🎮 How to Play
* **Push Left/Right:** Move the piece. Hold for smooth continuous movement (DAS).
* **Push Down:** Soft drop the piece faster.
* **Push Up:** Hard drop (instantly slams and locks the piece to the floor).
* **Click Joystick (SW):** Rotate the piece clockwise.

---
*Built with C++ and Adafruit GFX.*
