# Arduino Serial Controlled Servo Motor 🎛️

Project to control the position/angle of a servo motor by sending numerical values `0 - 180` through the Serial Monitor.
Covers important concepts like **separate power supply** and **common ground connection** — critical for real-world electronics.

---

## 🛒 Components Needed
- Arduino Uno R3
- SG90 / MG996R Servo Motor
- External 5V Power Supply (4xAA Battery Pack or Adapter)
- Jumper Wires *(Recommended: 24AWG Solid Wire for tight connections)*
- Breadboard

---

## ⚡ Wiring Diagram

![Circuit Diagram](circuit-diagram.png)

### 📌 Connections:
| Servo Wire | Connection |
| :--- | :--- |
| **ORANGE / YELLOW (Signal)** | → Arduino Digital Pin **2** |
| **RED (VCC / +)** | → **External 5V (+)** ⚠️ **DO NOT USE ARDUINO 5V** |
| **BLACK / BROWN (GND / -)** | → **External GND (-) AND Arduino GND** ⚠️ **COMMON GROUND RULE** |

> 💡 *Important:* Servos draw high current. Powering it directly from Arduino will cause resets, jitter, or damage. **Always use separate power + common ground.**

---

## 💻 Full Code
```cpp
#include <Servo.h>

Servo servo;
int data = 0; // Variable to store received number

void setup() {
  servo.attach(2); // Attach servo to Pin 2
  Serial.begin(9600); // Start communication at 9600 baud
  Serial.println("✅ Servo Ready! Send number 0 - 180");
}

void loop() {
  // Check if data was sent from Serial Monitor
  if (Serial.available() > 0) {
    
    // Read the whole number sent
    data = Serial.parseInt();

    // Move servo exactly to the received angle
    servo.write(data);
    Serial.print("➡️ Moved to Angle: ");
    Serial.println(data);

    delay(1000); // Wait 1 second before listening again
  }
}
