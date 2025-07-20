# 💧 Water Tank Level Controller using Arduino

An automatic water tank level monitoring and controlling system using Arduino, an ultrasonic sensor, and a relay module. The system detects the water level in the tank and controls a motor or pump accordingly to avoid overflow and dry running. Ideal for homes, agriculture, and small industries.

---

## 🎯 Features

- 🚰 Real-time water level detection using ultrasonic sensor (HC-SR04)
- ⚙️ Automatic motor ON/OFF control with 5V relay
- 💡 Optional LCD display for water level in percentage
- 🔔 Buzzer/LED indicators for full or low levels (optional)
- 🛡️ Simple, low-cost, and reliable design

---

## 🧰 Components Used

| Component              | Quantity |
|------------------------|----------|
| Arduino UNO            | 1        |
| HC-SR04 Ultrasonic Sensor | 1     |
| 5V Relay Module        | 1        |
| Submersible Pump       | 1        |
| 16x2 LCD with I2C (optional) | 1  |
| Buzzer / LED (optional)| 1        |
| Power Supply (5V)      | 1        |
| Jumper Wires, Breadboard | As needed |

---

## 🛠️ Working Principle

1. The HC-SR04 ultrasonic sensor is placed at the top of the tank to measure the distance from the water surface.
2. Arduino calculates the tank water level by subtracting the distance from the total tank height.
3. If the water level drops below a minimum threshold (e.g., 30%), the pump is turned ON.
4. If the water level reaches maximum level (e.g., 90%), the pump is turned OFF.
5. Water level can be shown on the LCD or serial monitor.

---

## 💻 Arduino Code

The code is available in the `Arduino_Code/` folder.

```cpp
#define trigPin 9
#define echoPin 10
#define pumpPin 7

long duration;
int distance;
int tankHeight = 30; // cm
int level;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(pumpPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  level = tankHeight - distance; // Water level in cm
  int percentage = (level * 100) / tankHeight;

  Serial.print("Water Level: ");
  Serial.print(percentage);
  Serial.println("%");

  if (percentage < 30) {
    digitalWrite(pumpPin, HIGH); // Turn ON pump
  } else if (percentage >= 90) {
    digitalWrite(pumpPin, LOW); // Turn OFF pump
  }

  delay(1000);
}
