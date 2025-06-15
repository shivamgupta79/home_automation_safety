# 🌍 Smart Environmental Monitoring and Control System

This project implements a smart environmental monitoring system using an Arduino. It monitors:

- **Temperature and Humidity** using a DHT11 sensor
- **Air Quality** using an MQ-type analog sensor
- **Vibration (Earthquake detection)** using a vibration sensor
- **Power Cut Automation** via a relay
- **LCD Display** for real-time data
- **Buttons** to set temperature threshold

When certain conditions are met, such as:
- **High temperature**, the **cooling system (relay)** is activated.
- **High humidity or poor air quality**, an **alert is shown** on the display.
- **Earthquake detected**, **power is shut off automatically**.

---

## ⚙️ Features

- 🌡️ Real-time temperature & humidity display
- 🌪️ Automatic cooling relay control
- 🌫️ Air quality alert system
- ⚡ Power shutdown on vibration (e.g., earthquake)
- 📟 LCD display for environmental status
- 🔘 Buttons for user-defined temperature threshold

---

## 🧰 Hardware Required

| Component             | Quantity |
|-----------------------|----------|
| Arduino Uno/Nano      | 1        |
| DHT11 Sensor          | 1        |
| MQ-135 / MQ-2 (Air Sensor) | 1   |
| Vibration Sensor      | 1        |
| Relay Module (5V)     | 1        |
| LCD 16x2 (I2C)        | 1        |
| Push Buttons          | 2        |
| Resistors (10kΩ)      | 2        |
| Jumper Wires + Breadboard | -  |

---

## 🖥️ Pin Configuration

| Component             | Arduino Pin |
|-----------------------|-------------|
| DHT11                 | D3          |
| Vibration Sensor      | D2          |
| Relay Module          | D4          |
| Button (Up)           | D5          |
| Button (Down)         | D6          |
| Air Quality Sensor    | A0          |
| LCD I2C               | SDA/SCL     |

---

## 📋 How It Works

1. **Boot and Start Monitoring**
2. Continuously reads temperature, humidity, vibration, and air quality.
3. If vibration is detected → **Relay OFF** (emergency power cut) and warning shown.
4. If temperature >= setTemp → **Relay ON** (start fan or AC).
5. If air quality > threshold or humidity > threshold → **Alert user to open windows**.
6. Use Up/Down buttons to adjust the `setTemp`.

---

## 🚀 Setup Instructions

1. Upload the Arduino code to your board.
2. Connect all components as per the pin configuration.
3. Power up your board and open Serial Monitor at 9600 baud.
4. Use the LCD and buttons to view and adjust settings.

---

## 🧪 Threshold Values

- `setTemp`: Default = 25°C (Adjustable via buttons)
- `humidityThreshold`: 60%
- `airQualityThreshold`: 400 (Analog read)

---

## 📦 Dependencies

Install these libraries in Arduino IDE:

- `LiquidCrystal_I2C`
- `DHT`

Use Library Manager:
Sketch > Include Library > Manage Libraries...

yaml
Copy
Edit

---

## 🛡️ License

MIT License – Free for personal and commercial use.
📊 Flowchart
Here's the process flowchart description (you can visualize it using tools like draw.io, Lucidchart, or Figma):

pgsql
Copy
Edit
       +------------------------+
       |     Arduino Setup      |
       +-----------+------------+
                   |
                   v
       +------------------------+
       | Initialize LCD & DHT   |
       | Set Pin Modes          |
       +-----------+------------+
                   |
                   v
       +----------------------------+
       |       Start Main Loop      |
       +----------------------------+
                   |
                   v
       +----------------------------+
       | Check Vibration Sensor     |
       +------------+---------------+
                    |
           Yes      |      No
            +       v
            |  Continue Loop
            |
    +-------------------------+
    | Cut Power (Relay OFF)   |
    | Show "Earthquake" Msg   |
    +-------------------------+
            |
         delay(2000)
            |
            v
         Restart loop
                   |
                   v
      +-----------------------------+
      | Read Temp & Humidity (DHT) |
      +-----------------------------+
                   |
                   v
      +-----------------------------+
      | Read Button Press (↑ ↓)     |
      | Adjust `setTemp`           |
      +-----------------------------+
                   |
                   v
      +-----------------------------+
      | Temp >= setTemp?            |
      +-------------+---------------+
                    |
            Yes     |      No
             |      |       |
    +--------v--+   |   +---v--------+
    | Relay ON  |   |   | Relay OFF  |
    +-----------+   |   +------------+
                    |
                   v
      +-----------------------------+
      | Read Air Quality & Humidity|
      +-------------+--------------+
                    |
        >Threshold? |      OK
             |      |       |
    +--------v--+   |   +---v--------+
    | Show Alert |   |   | Show Temp |
    | (LCD & Serial) |  | & Humidity |
    +------------+   |   +-----------+
             |
         delay(2000)
             |
            loop
