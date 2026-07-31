# Autonomous & Smart RC Multi-Mode Robot (Mark 2)

![ESP32](https://img.shields.io/badge/Microcontroller-ESP32-blue?style=for-the-badge&logo=expressif)
![C++](https://img.shields.io/badge/Language-C%2B%2B-00599C?style=for-the-badge&logo=c%2B%2B)
![Status](https://img.shields.io/badge/Build-Working-brightgreen?style=for-the-badge)

The **Mark 2 Smart Robot** is an engineering upgrade of a differential-drive robotics platform designed to demonstrate advanced embedded systems integration, concurrent logic processing, and multi-sensor data fusion using the **ESP32** microcontroller.

Building upon the fundamental power isolation lessons of Mark 1, this iteration integrates multiple operational states into a single non-blocking architecture, capable of switching dynamically between manual human control and active autonomous navigation.

## 🌟 Features & Operational Modes

This platform operates as a 3-in-1 system managed by an efficient state machine:

1.  🕹️ **Manual RC Mode (Bluetooth Classic):**
    Allows real-time wireless driving using a custom mobile dashboard (Bluetooth Electronics app). The system handles low-latency UART commands (`F`, `B`, `L`, `R`, `S`) with dynamic PWM speed control.
2.  🤖 **Autonomous Obstacle Avoidance:**
    Utilizes an HC-SR04 ultrasonic distance sensor. The ESP32 calculates live distances and executes non-blocking evasive maneuvers (Halt, Reverse, Dodge) when a threshold (e.g., $25\text{ cm}$) is breached.
3.  🛤️ **Line Follower Mode:**
    Employs a dual IR sensor array to track black lines on a white surface. The control loop implements sharp differential corrections to maintain the robot’s path.

## 📐 Technical Architecture & Design Considerations

The core technical challenge solved in the Mark 2 iteration was upgrading system intelligence without sacrificing electrical stability.

### ⚡ Power Isolation Architecture
Maintaining clear decoupling between high-current inductive motor draws and sensitive $3.3\text{V}$ microcontroller logic was critical.
*   **High Voltage Rail ($11.1\text{V}$):** Powers the TB6612FNG driver’s logic-independent motor outputs.
*   **Regulated Logic Rail ($5.0\text{V}$):** Decoupled via an LM2596 Buck Converter, stabilizing the ESP32, OLED, and sensors.

### 📊 Real-Time Telemetry & physical UI
*   **Onboard OLED Dashboard:** A 0.96" SSD1306 display provides instantaneous feedback, rendering the active mode selection, calculated speed ($\text{Km/h}$), and real-time obstacle distance data.
*   **Hardware Mode Switching Logic:** A dual-button UI enables robust system state control. An external button cycles through mode previews, and the onboard **BOOT button (GPIO 0)** acts as a final confirm trigger to lock in the mode, preventing accidental transitions during operation.

### 🛡️ Signal Safety & Logic Level Tolerance
The HC-SR04 $5\text{V}$ `Echo` output is dangerous to the ESP32’s $3.3\text{V}$ GPIO tolerance. This schematic integrates a simple voltage divider ($1\text{k}\Omega / 2\text{k}\Omega$) on this critical line to reduce the pulse to safe $3.3\text{V}$ logic levels.

<img width="1280" height="960" alt="4f4d1c7f-ab72-4b8e-9a74-bc4262cafc54" src="https://github.com/user-attachments/assets/33a9a334-2741-4a74-b558-88d29f67b639" />
<img width="960" height="1280" alt="59611923-6a18-44fa-b5c5-94bb697bd508" src="https://github.com/user-attachments/assets/773f2707-f8ad-4c6d-a7b5-b843409cf1ac" />
<img width="875" height="1216" alt="28143122-fc1c-4678-95b4-e06f5aa9a56e" src="https://github.com/user-attachments/assets/7a06b3f1-c4d0-4a8a-9128-c773ac3b6e8c" />
