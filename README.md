# 💓 IoT based Heart Rate Monitoring System with Custom Android App

This project is a smart heart rate and SpO2 monitoring system built using the ESP32 microcontroller, MAX30102 sensor, and MP1584 buck converter. It connects to the internet via Wi-Fi, logs health data to Firebase Realtime Database, and is paired with a custom Android application for real-time viewing, session management, and multi-user support.

---

## 📦 Components Used

- ESP32 Dev Module
- MAX30102 Heart Rate and SpO2 Sensor
- MP1584 Buck Converter
- Onboard Blue LED (for status indication)

---

## ⚙️ Firmware Functionality (ESP32)

- On power-up, ESP32 checks for a saved Wi-Fi connection.
- It uses the **WiFiManager** library to simplify Wi-Fi setup:
  - If Wi-Fi is successfully connected, the onboard **blue LED** lights up.
  - If not, it starts an **Access Point (AP)** named `MyHeartAP`.
  - The user can connect to this AP and enter new Wi-Fi credentials.
- By **holding the BOOT button for 5 seconds**, stored credentials can be reset and the AP is re-enabled for new configuration.
- Once Wi-Fi is connected, the device connects to **Firebase Realtime Database**.
- It reads the heart rate and SpO2 data using the MAX30102 sensor and pushes the values to Firebase.
- After every reading, it updates a Firebase path called `online_status` with the value `1` to indicate the module is online.

---

## 📲 Android App Overview

### 🔐 Login & Authentication

- Users can log in using email and password.
- Google Sign-In is supported.
- A **Sign-Up page** is available for new users (name, email, password).
- Forgot password feature is also included.

### 🏠 Home Screen

- Displays a welcome message with the user’s name.
- Top-right **menu button** provides:
  - **Saved Readings**: Shows average HR and SpO2 for the session and total reading count.
  - **About**: Provides app description and developer info.
- An **Online Status box** checks the module's availability:
  - It turns **green** if `/online_status = 1` is received.
  - The app sets it back to `0` so the device can set it again after the next loop.
  - If value stays `0`, it means the module is offline and box turns **red**.
  - This check happens **every 60 seconds**.
- **Live HR and SpO2 values** are shown in real time.
- **Start** and **Stop** buttons begin or end a monitoring session.
  - A popup appears to confirm session start/stop.
  
### 👥 Multi-User Support

- A single device can be used by multiple users.
- Each user has their own **locally stored data** on the app.
- No cloud storage of personal session data ensures privacy and isolation.

---

## 🔁 Firebase Data Flow

1. ESP32 reads heart rate and SpO2 from MAX30102.
2. It uploads the values to Firebase RTDB under the current user's node.
3. Updates `online_status = 1` at the end of each loop.
4. The app reads this value every 60 seconds:
   - If `1`, it sets back to `0` and shows green indicator.
   - If not updated, the indicator remains red, showing offline state.

---

## 🧱 Local Data Storage (Android)

- All readings are stored **locally per user** on the phone.
- Ensures **privacy** and allows offline viewing of previous sessions.

---

## 📚 Libraries Used

### ESP32
- WiFiManager
- Firebase ESP Client
- SparkFun MAX3010x Sensor Library
- Arduino Core for ESP32

### Android
- Firebase Authentication
- Firebase Realtime Database
- Google Sign-In API
- Room (for local storage)
- Material Components

---

## 📌 Notes

- Firebase rules must be set up to allow authenticated read/write access.
- The system assumes only one ESP32 module is pushing data for a given time.

---

## 👨‍💻 Developer

Developed by **RagingHELIOS**

---
