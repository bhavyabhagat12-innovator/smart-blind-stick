# smart-blind-stick
Smart Blind Stick with AI-Based Obstacle Detection and Voice Alerts: This project presents a smart assistive device designed to help visually impaired individuals. The system uses an Arduino UNO along with multiple sensors to detect, classify and announce obstacles in real time.
# 🦯 Adaptive AI-Assisted Smart Blind Stick

> 🏆 **1st Place** — College-Level Technical Project Competition, NMIET Talegaon (2025)
---

## 🔍 Problem Statement
Over **40 million visually impaired individuals in India** rely on basic white canes that can only detect obstacles through physical contact. Existing electronic sticks offer a single beep for all obstacles — with no classification, no context, and no intelligence.

## 💡 Our Solution
An Arduino-based smart navigation stick that uses **multi-sensor fusion** and **rule-based AI logic** to:
- **Classify** obstacles as Wall / Human / Pit / Vehicle
- **Deliver** context-aware voice alerts based on distance and direction
- **Adapt** warning intensity based on environment (indoor/outdoor, day/night)

## ✨ What Makes This Unique

| Feature | This Project |
|--------|-------------|-------------|
| Obstacle detection | ✅ Yes
| Obstacle classification | ✅ Wall / Human / Pit / Vehicle |
| Voice alerts | ✅ Context-aware voice |
| Night/day detection | ✅ LDR-based |
| Fall/tilt detection | ✅ MPU6050 gyroscope |
| Multi-sensor fusion | ✅ 4 sensors combined |

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

🎯 Features
Obstacle detection using ultrasonic sensor
Pit/step detection using IR sensor
Motion-based detection using MPU6050
Rule-based AI for obstacle classification
Voice alerts using DFPlayer Mini
Low-cost and portable design

🧠 How It Works

The system continuously collects data from sensors:
Ultrasonic sensor detects distance to obstacles
IR sensor detects ground-level hazards
MPU6050 detects motion
All data is processed by the Arduino UNO using rule-based logic. Based on the conditions, the system classifies the obstacle and plays the appropriate voice message through a speaker.

🛠️ Components Used
Arduino UNO
HC-SR04 Ultrasonic Sensor
IR Sensor
MPU6050 (Accelerometer + Gyroscope)
DFPlayer Mini MP3 Module
Speaker
Battery

🔌 Circuit Connections
Component	Arduino Pin
Ultrasonic TRIG	D9
Ultrasonic ECHO	D10
IR Sensor	D6
DFPlayer RX	D2
DFPlayer TX	D3
MPU6050	SDA → A4, SCL → A5

The Arduino code reads sensor values continuously, processes them using rule-based AI logic, and triggers voice alerts depending on obstacle type.

▶️ How to Run
Connect all components as per the circuit diagram
Upload the Arduino code using Arduino IDE
Add voice files (0001.mp3, 0002.mp3, etc.) to SD card
Power the system
Test by placing obstacles in front

🔮 Future Scope
GPS-based navigation
Mobile app integration
Machine learning for object recognition
Emergency SOS feature

## 👤 Author
**Bhavya G Bhagat**
B.Tech AI & Data Science, NMIET Talegaon, Pune
IIT Madras BS — Data Science & Applications
[LinkedIn](https://www.linkedin.com/in/bhavyab12)
