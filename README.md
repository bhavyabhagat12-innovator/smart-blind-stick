# smart-blind-stick
Smart Blind Stick with AI-Based Obstacle Detection and Voice Alerts: This project presents a smart assistive device designed to help visually impaired individuals. The system uses an Arduino UNO along with multiple sensors to detect, classify and announce obstacles in real time.
# 🦯 Adaptive AI-Assisted Smart Blind Stick
### Predictive Obstacle Classification & Context-Aware Voice Navigation

> 🏆 **1st Place** — College-Level Technical Project Competition, NMIET Talegaon (2025)

---

## 🔍 Problem Statement
Over **40 million visually impaired individuals in India** rely on basic white canes that can only detect obstacles through physical contact. Existing electronic sticks offer a single beep for all obstacles — with no classification, no context, and no intelligence.

---

## 💡 Our Solution
An Arduino-based smart navigation stick that uses **multi-sensor fusion** and **rule-based AI logic** to:
- **Classify** obstacles as Wall / Human / Pit / Vehicle
- **Deliver** context-aware voice alerts based on distance and direction
- **Adapt** warning intensity based on environment (indoor/outdoor, day/night)

---

## ✨ What Makes This Unique

| Feature | Basic Sticks | This Project |
|--------|-------------|-------------|
| Obstacle detection | ✅ Yes | ✅ Yes |
| Obstacle classification | ❌ No | ✅ Wall / Human / Pit / Vehicle |
| Voice alerts | ❌ No | ✅ Context-aware voice |
| Night/day detection | ❌ No | ✅ LDR-based |
| Fall/tilt detection | ❌ No | ✅ MPU6050 gyroscope |
| Multi-sensor fusion | ❌ No | ✅ 4 sensors combined |

---

## 🧠 AI Logic (Rule-Based Classifier)
```cpp
IF distance < 50cm AND object height increases rapidly
  → Classify as WALL → Alert: "Wall ahead"

IF distance < 70cm AND MPU detects motion ahead
  → Classify as HUMAN → Alert: "Person ahead"

IF IR detects no reflection + sudden depth change
  → Classify as PIT → Alert: "Danger, pit detected"

IF distance < 150cm AND speed change detected
  → Classify as VEHICLE → Alert: "Vehicle approaching"

## 🔧 System Architecture
```
[Sensors] → [Arduino UNO] → [AI Classification Logic] → [DFPlayer Voice Output]
  HC-SR04 (distance)              Rule-based engine              Speaker
  IR x2 (pit/side)
  MPU6050 (tilt/fall)
  LDR (light level)
```

---

## 🛠️ Hardware Components

| Component | Purpose |
|-----------|---------|
| Arduino UNO | Central controller |
| HC-SR04 Ultrasonic | Obstacle distance measurement |
| IR Sensor x2 | Pit detection + side obstacles |
| MPU6050 | Movement, tilt and fall detection |
| LDR | Day/night environment detection |
| DFPlayer Mini MP3 | Voice output module |
| 8Ω Speaker | Audio alerts |
| Li-ion Battery + TP4056 | Rechargeable power supply |

---

## 💻 Core Arduino Code
```cpp
#include <Wire.h>
#include <MPU6050.h>
#include <SoftwareSerial.h>
#include <DFRobotDFPlayerMini.h>

MPU6050 mpu;
SoftwareSerial mp3(2, 3);
DFRobotDFPlayerMini player;

#define trigPin 9
#define echoPin 10
#define irPin 6

long duration;
int distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(irPin, INPUT);
  Serial.begin(9600);
  mp3.begin(9600);
  player.begin(mp3);
  player.volume(25);
  Wire.begin();
  mpu.initialize();
}

int getDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  return duration * 0.034 / 2;
}

void loop() {
  distance = getDistance();
  int irValue = digitalRead(irPin);

  if (distance < 50 && irValue == HIGH) {
    player.play(1); // Obstacle ahead
    delay(2000);
  } else if (irValue == LOW) {
    player.play(3); // Pit detected
    delay(2000);
  }
}
```

---

## 🔮 Future Scope
- 📱 Mobile app integration for caregiver alerts
- 🗺️ GPS-based navigation with Google Maps API
- 🤖 Machine learning-based obstacle recognition
- 🆘 Emergency SOS with location sharing

---

## 📋 Patent Claim Ideas
1. Smart navigation aid using multi-sensor fusion for adaptive voice alerts
2. Rule-based AI decision engine for obstacle type prediction
3. Context-aware alert modulation based on environmental conditions

---

## 👤 Author
**Bhavya G Bhagat**
B.Tech AI & Data Science, NMIET Talegaon, Pune
IIT Madras BS — Data Science & Applications
[LinkedIn](https://www.linkedin.com/in/bhavyab12)
