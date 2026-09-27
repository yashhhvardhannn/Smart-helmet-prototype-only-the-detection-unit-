# 🪖 Smart Helmet – Detection Unit

A prototype **Smart Helmet safety system** designed to improve rider safety by monitoring helmet usage, alcohol presence, and drowsiness, then transmitting the detected status wirelessly to a receiver unit.

> **Project scope:** This repository contains the **detection and communication unit prototype** of the Smart Helmet system.

---

## 🎥 Demo

### Prototype demonstration

> **Video:** Add the demo video to the repository as `demo/smart-helmet-demo.mp4`.

[▶️ **Watch the Smart Helmet Demo**](./demo/smart-helmet-demo.mp4)

> GitHub may not render a repository `.mp4` as an inline player in every README context. The link above provides a reliable way to open the video file.

---

## 📌 Project Overview

The Smart Helmet prototype consists of two main units:

**1. Transmitter (TX)**  
Mounted on/near the helmet and responsible for reading the safety sensors and transmitting their status wirelessly.

**2. Receiver (RX)**  
Receives the transmitted data, displays the current status on an I2C LCD, controls the vehicle ignition relay, and activates alerts when unsafe conditions are detected.

### Data transmitted

The transmitter sends three values in the following format:

`Helmet Status, Alcohol Status, Drowsiness Status`

Example:

`1,0,0`

- `1` → Helmet detected
- `0` → No alcohol detected
- `0` → No drowsiness detected

---

## ⚙️ Key Features

- 🪖 **Helmet-wear detection**
- 🍺 **Alcohol detection**
- 😴 **Drowsiness detection**
- 📡 **Wireless RF communication**
- 🔊 **Buzzer alerts**
- 📟 **16×2 I2C LCD status display**
- 🛵 **Ignition control through relay**
- ⏱️ **Communication timeout detection**
- 🔄 **Automatic LCD status page switching**
- ⚡ Real-time sensor monitoring and transmission

---

## 🧠 How It Works

### Transmitter Unit

The transmitter continuously reads:

- Helmet-wear sensor
- Alcohol sensor
- Sleep/drowsiness sensor

For drowsiness detection, the system tracks repeated sleep signals within a **10-second window**. When the configured threshold of **5 detections** is reached, drowsiness is triggered and the buzzer is activated.

The transmitter sends the status packet approximately every **50 ms** using the RF module.

### Receiver Unit

The receiver listens for RF packets and extracts:

- Helmet status
- Alcohol status
- Drowsiness status

The ignition relay is enabled only when:

`Helmet = Detected AND Alcohol = Not Detected`

The receiver also:

- Displays sensor information on a 16×2 LCD
- Sounds the buzzer when alcohol or drowsiness is detected
- Turns the system offline and activates an alert when no RF data is received for **5 seconds**

---

## 🔌 System Architecture

```text
           SMART HELMET
                │
        ┌───────┼────────┐
        │       │        │
     Helmet   Alcohol   Sleep
     Sensor   Sensor   Sensor
        │       │        │
        └───────┼────────┘
                │
        ┌───────────────┐
        │  TX Arduino   │
        │ + RF Module   │
        └───────┬───────┘
                │
             RF Link
                │
        ┌───────▼───────┐
        │  RX Arduino   │
        │ + RF Module   │
        └───┬────────┬──┘
            │        │
       ┌────▼───┐  ┌─▼────────┐
       │  LCD   │  │  Buzzer  │
       └────────┘  └──────────┘
            │
       ┌────▼─────┐
       │ Ignition │
       │  Relay   │
       └──────────┘
```

---

## 🧩 Hardware Used

| Component | Purpose |
|---|---|
| Arduino-compatible microcontroller ×2 | TX and RX control |
| RF transmitter/receiver modules | Wireless communication |
| Alcohol sensor | Detect alcohol presence |
| Helmet-wear sensor | Detect whether the helmet is worn |
| Sleep/drowsiness sensor | Detect repeated sleep signals |
| Buzzer ×2 | Audible safety alerts |
| 16×2 I2C LCD | Display system status |
| Relay module | Vehicle ignition control |
| Supporting wiring/components | Circuit connections |

---

## 🔧 Pin Configuration

### Transmitter

| Function | Pin |
|---|---|
| Alcohol Sensor | A0 |
| Helmet/Wear Sensor | A1 |
| Sleep Sensor | A2 |
| Buzzer | D7 |
| RF module | RadioHead ASK |

### Receiver

| Function | Pin |
|---|---|
| Ignition Relay | D4 |
| Buzzer | D5 |
| RF Receive | D7 |
| RF Transmit | D12 |
| I2C LCD | I2C |

> Pin assignments can be modified directly in the Arduino source files.

---

## 📂 Repository Structure

```text
Smart-helmet-prototype-only-the-detection-unit-/
│
├── Code/
│   ├── Smart_Helmet_TX/
│   │   └── Smart_Helmet_TX.ino
│   │
│   └── Smart_Helmet_RX/
│       └── Smart_Helmet_RX.ino
│
├── img/
│   ├── Circuit Diagram of Reciever.png
│   ├── Circuit Diagram of Transmitter.png
│   └── ...
│
├── Smart Helmet tx Flow Chart.svg
├── Smart Helmet rx Flow Chart.svg
└── README.md
```

---

## 🖼️ Circuit & Flow Diagrams

### Transmitter Circuit

![Transmitter Circuit](./img/Circuit%20Diagram%20of%20Transmitter.png)

### Receiver Circuit

![Receiver Circuit](./img/Circuit%20Diagram%20of%20Reciever.png)

### Transmitter Flowchart

![TX Flowchart](./Smart%20Helmet%20tx%20Flow%20Chart.svg)

### Receiver Flowchart

![RX Flowchart](./Smart%20Helmet%20rx%20Flow%20Chart.svg)

---

## 💻 Software & Libraries

The project is developed using the **Arduino/C++ ecosystem**.

### Libraries

- [RadioHead](https://www.airspayce.com/mikem/arduino/RadioHead/)
- `SPI`
- `Wire`
- `LiquidCrystal_I2C`

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/yashhhvardhannn/Smart-helmet-prototype-only-the-detection-unit-.git
cd Smart-helmet-prototype-only-the-detection-unit-
```

### 2. Install the required libraries

Install:

- RadioHead
- LiquidCrystal_I2C

### 3. Upload the transmitter code

Open:

```text
Code/Smart_Helmet_TX/Smart_Helmet_TX.ino
```

Connect the transmitter-side hardware and upload the program.

### 4. Upload the receiver code

Open:

```text
Code/Smart_Helmet_RX/Smart_Helmet_RX.ino
```

Connect the receiver-side hardware and upload the program.

### 5. Test the system

Test:

1. Helmet detection
2. Alcohol detection
3. Drowsiness detection
4. RF communication
5. LCD output
6. Buzzer alerts
7. Ignition relay response

---

## 🔄 Safety Logic

The prototype uses the following ignition condition:

```text
Helmet Detected?
      │
      ├── No ──────► Engine OFF
      │
      └── Yes
           │
      Alcohol Detected?
           │
           ├── Yes ─► Engine OFF + Alert
           │
           └── No ──► Engine ON
```

Drowsiness detection independently triggers an audible warning.

---

## 📊 Current Prototype Status

| Feature | Status |
|---|---|
| Helmet detection | ✅ Implemented |
| Alcohol detection | ✅ Implemented |
| Drowsiness detection | ✅ Implemented |
| RF transmission | ✅ Implemented |
| RF reception | ✅ Implemented |
| LCD display | ✅ Implemented |
| Buzzer alerts | ✅ Implemented |
| Ignition relay control | ✅ Implemented |
| Communication timeout | ✅ Implemented |
| Full helmet integration | 🚧 Prototype stage |

---

## 🔮 Future Improvements

- Improve drowsiness detection accuracy using a dedicated sensor/algorithm
- Add GPS-based emergency location tracking
- Add GSM/4G communication for emergency alerts
- Add crash/impact detection using an IMU
- Add mobile application integration
- Improve RF communication reliability and range
- Add battery monitoring and low-power operation
- Integrate the detection unit into a complete helmet enclosure
- Add data logging for safety events

---

## 👨‍💻 Project

**Smart Helmet – Detection Unit Prototype**

Built using embedded systems, sensors, RF communication, and Arduino/C++.

### Repository

[GitHub Repository](https://github.com/yashhhvardhannn/Smart-helmet-prototype-only-the-detection-unit-)

---

## ⭐ Acknowledgement

This project was developed as an embedded-systems prototype focused on combining multiple rider-safety checks into a single wireless detection and control system.
