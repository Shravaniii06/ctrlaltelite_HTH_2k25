# ctrlaltelite_HTH_2k25
Fall detection IoT system using ESP32 and Blynk
# 🛡️ Smart Fall Detection System

## 🆔 Problem Statement ID: HTH-FALL-CARE-01  
## 📌 Title: Fall Detection for Elderly Using ESP32 + MPU6050 + IoT Alerts

---

## 🧠 Problem Statement  
Falls are one of the leading causes of injury among the elderly. Timely detection and alerting is critical for safety and survival. Existing solutions are either expensive or not real-time.

---

## 💡 Our Solution  
We are building a low-cost wearable IoT system that detects when a person falls using an MPU6050 sensor and immediately sends a mobile notification through the Blynk IoT platform. It also turns on an LED to indicate the fall.

---

## 🔩 Components Used  
- ESP32 Dev Board  
- MPU6050 Accelerometer/Gyroscope  
- LED + 220Ω resistor  
- Blynk IoT Mobile App  
- Arduino IDE  

---

## 🔧 Solution Architecture  
- MPU6050 monitors acceleration
- ESP32 processes data to detect fall (free-fall + impact)
- On fall:  
  - LED turns ON  
  - Notification is sent to caregiver using Blynk

---

## 📐 Overall Structure
[MPU6050 Sensor]
       ↓
[ESP32 Microcontroller] → [LED Alert]
       ↓
[Wi-Fi Network]
       ↓
[Blynk Cloud] → [Mobile Notification]