# ⚡ Power Consumption Monitoring & Prediction System

A smart IoT-powered solution to **monitor, log, and forecast** power usage in real time — combining the strengths of embedded systems, cloud connectivity, and machine learning. This system uses an **ESP32 microcontroller**, real-time sensors, cloud integrations (Blynk, ThingSpeak, Google Sheets, Telegram), and an interactive **Flask web app** with **LSTM-based forecasting**.

---

## 🔧 Hardware Overview

### 🧰 Required Components

| Component          | Purpose                       |
| ------------------ | ----------------------------- |
| ESP32 Dev Board    | Wi-Fi-enabled microcontroller |
| ACS712 Sensor      | Measures current              |
| ZMPT101B Sensor    | Measures voltage              |
| 16x2 LCD + I2C     | Real-time data display        |
| Breadboard + Wires | Circuit assembly              |
| 5V Power Supply    | Powers the system             |

### 📌 Pin Mapping

| Component      | ESP32 Pin  | Notes                          |
| -------------- | ---------- | ------------------------------ |
| ACS712 (OUT)   | GPIO 34    | Analog input for current       |
| ZMPT101B (OUT) | GPIO 35    | Analog input for voltage       |
| LCD (SDA/SCL)  | GPIO 21/22 | I2C interface                  |
| GND/VCC        | GND/5V     | Common ground and power source |

---

## 🚀 Firmware Setup

### 1. Flash the ESP32

* Open **Arduino IDE**

* Install board: *ESP32 by Espressif*

* Load `sketch_mar7a_copy_20250325014610.ino`

* Add libraries:

  * `WiFi.h`, `HTTPClient.h`, `LiquidCrystal_I2C`, `EmonLib`

* Configure the following in code:

  ```cpp
  const char* ssid = "YOUR_WIFI";
  const char* password = "YOUR_PASS";
  char auth[] = "BLYNK_TOKEN";
  String apiKey = "THINGSPEAK_API";
  String botToken = "TELEGRAM_BOT_TOKEN";
  const char* scriptUrl = "GOOGLE_SCRIPT_URL";
  ```

* Upload and you're done!

---

## 📲 Real-Time Integrations

### 🔗 Blynk IoT App

* Create project → Add:

  * LCD Display
  * Voltage, Current, Power Gauges
  * Notifications
* Paste Auth Token into sketch
* Monitor in-app

---

### 🌐 ThingSpeak Dashboard

* Create a channel → Add Fields:

  * Voltage, Current, Power
* Paste API Key into code
* Auto-plots incoming values

---

### 🤖 Telegram Bot Alerts

* Create bot with **@BotFather**
* Paste token into sketch
* Use **@userinfobot** to get `chat_id`
* Receives alerts for power spikes or anomalies

---

### 📈 Google Sheets Logging

* Create Sheet → Apps Script → Paste `logSensorData.gs`
* Deploy as Web App
* Copy URL to sketch
* System logs data every few seconds!

---

## 🧠 ML-Based Forecasting System

A powerful **LSTM model** predicts the next 48 hours of power and energy usage.

### 🗂 Project Structure

```
Software-Setup/
├── app.py                  ← Flask backend
├── model_training.py       ← LSTM training script
├── monitor_data.csv        ← Historical training data
├── current data.csv        ← Real-time input
├── power_units_lstm.keras  ← Trained model
├── scaler.pkl              ← Normalization scaler
├── static/
│   ├── prediction_plot.png ← Forecast chart
│   └── training_history.png← Loss curve
├── templates/
│   ├── index.html          ← Upload interface
│   └── results.html        ← Results + AI Assistant
└── uploads/                ← User-uploaded CSVs
```

---

## 🔁 End-to-End Data Flow

```
ESP32 → Google Sheets → model_training.py → app.py (web interface) → AI Forecast Output
```

* Logs voltage/current → calculates power and units
* Pushes to Google Sheets via Apps Script
* Data exported as CSV and used to train LSTM
* Trained model forecasts future consumption

---

## 🧪 Training the Model

```bash
python model_training.py
```

* Combines `DATE` + `TIME`
* Fills missing values, scales input
* LSTM layers with Dropout + Adam Optimizer
* Outputs:

  * `power_units_lstm.keras`
  * `scaler.pkl`
  * `training_history.png`

---

## 🌐 Web App Usage

### Run Locally:

```bash
pip install flask pandas numpy tensorflow scikit-learn joblib matplotlib google-generativeai
python app.py
```

Visit: [http://localhost:5000](http://localhost:5000)

---

### Upload File Format

| Column  | Type   | Example    |
| ------- | ------ | ---------- |
| DATE    | String | 20/05/2025 |
| TIME    | String | 14:00:00   |
| VOLTAGE | Float  | 230.5      |
| CURRENT | Float  | 3.2        |
| POWER   | Float  | 737.6      |
| UNITS   | Float  | 0.74       |

⚠️ **Minimum 24 rows required (for last 24 hours)**

---

## 📊 Output Insights

* 📈 Graph of predicted POWER and UNITS (48 hours)
* 🧾 Downloadable forecast data (CSV)
* 💬 **Gemini AI Assistant** for queries like:

  * "What’s tomorrow’s expected usage?"
  * "Any peak hours to watch?"
  * "How can I cut down electricity bills?"

---

## 🏁 Final Thoughts

This project is a complete fusion of **IoT**, **cloud computing**, and **machine learning**, bringing **real-time visibility** and **intelligent forecasting** to your fingertips. From DIY hardware setup to advanced AI predictions, it's designed to be practical, insightful, and future-ready.

---

## ✨ Future Enhancements

* Mobile-optimized dashboard
* Smart device control (relays)
* Daily/Weekly PDF reports
* Auto-email alerts

---

## 🤝 Credits

Built with ❤️ by **Avinash Kumar**
Inspiration drawn from the need for **energy efficiency** and **intelligent power management.**

---

