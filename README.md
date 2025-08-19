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

## main code
/***************************************************
   ESP32 + MPU6050 + Blynk IoT Cloud
   Fall Detection System (Improved)
   Author: ChatGPT
***************************************************/

// ----------- BLYNK TEMPLATE INFO -----------
#define BLYNK_TEMPLATE_ID "TMPL3G2wKN658"
#define BLYNK_TEMPLATE_NAME "HEALTHIQ"
#define BLYNK_AUTH_TOKEN "zMHpjbGiYaclnYw0aM23fkrdG_qC44ON"

// ----------- LIBRARIES -----------
#include <Wire.h>
#include <MPU6050.h>
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

// ----------- WIFI INFO -----------
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Madhav";      
char pass[] = "12345678";  

// ----------- MPU6050 OBJECT -----------
MPU6050 mpu;

// ----------- BLYNK TIMER -----------
BlynkTimer timer;

// Variables
float AccX, AccY, AccZ;
float totalAcc;
unsigned long fallStart = 0;
bool fallDetected = false;

void sendSensorData() {
  // Read raw values
  int16_t ax, ay, az;
  int16_t gx, gy, gz;
  mpu.getMotion6(&ax, &ay, &az, &gx, &gy, &gz);

  // Convert to g’s
  AccX = ax / 16384.0;
  AccY = ay / 16384.0;
  AccZ = az / 16384.0;

  // Magnitude of acceleration
  totalAcc = sqrt(AccX * AccX + AccY * AccY + AccZ * AccZ);

  // ----------- Fall Detection Algorithm -----------

  // Step 1: Free-fall (very low acceleration)
  if (totalAcc < 0.5 && !fallDetected) {
    fallStart = millis();
    Serial.println("⚠️ Possible fall: Free-fall detected...");
  }

  // Step 2: Impact (high acceleration after free-fall)
  if (fallStart > 0 && totalAcc > 2.5) {
    unsigned long impactTime = millis() - fallStart;

    // Step 3: Check immobility after impact
    if (impactTime > 200 && impactTime < 2000) {  // fall duration 0.2–2 sec
      fallDetected = true;

      Serial.println("🚨 DANGER! FALL DETECTED 🚨");
      Blynk.logEvent("fall_alert", "🚨 Fall detected! Please check immediately.");

      // Optional: send acceleration value to Blynk
      Blynk.virtualWrite(V0, totalAcc);
    }

    fallStart = 0;  // Reset
  }

  // Normal monitoring (send total Acc to app)
  Blynk.virtualWrite(V1, totalAcc);
}

void setup() {
  Serial.begin(115200);

  Wire.begin();
  mpu.initialize();

  if (mpu.testConnection()) {
    Serial.println("✅ MPU6050 connected successfully!");
  } else {
    Serial.println("❌ MPU6050 connection failed!");
  }

  // Connect WiFi + Blynk
  Blynk.begin(auth, ssid, pass);

  // Run sensor check every 200 ms
  timer.setInterval(200L, sendSensorData);
}

void loop() {
  Blynk.run();
  timer.run();
}

## circuit diagram
![circuit diagram](https://github.com/user-attachments/assets/5df8108a-1d79-448a-a1bf-33e2f608935f)
