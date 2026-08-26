# DC Motor Control with PWM (Speed Control)

This example shows how to control a DC motor's speed using PWM (Pulse Width Modulation) on Arduino.

| Arduino Pin | Component   |
|-------------|-------------|
| D9 (PWM)    | ENA/ENB pin |
| GND         | GND         |
| Motor (-)   | OUT 1/3     |
| Motor (+)   | OUT 2/4     |

## Complete Code

```cpp:motor_control.ino
// Pin definition
const int motorPin = 9;  // PWM-capable pin (3,5,6,9,10,11 on Uno)

void setup() {
  pinMode(motorPin, OUTPUT);
  Serial.begin(9600);
  Serial.println("Motor Control Ready");
  Serial.println("Send numbers 0-255 to set speed");
  Serial.println("Or use commands: 'f' (forward), 's' (stop)");
}

void loop() {
  // Check for serial input
  if (Serial.available()) {
    char command = Serial.read();
    
    // Convert command to integer (for speed values 0-255)
    if (command >= '0' && command <= '9') {
      int speed = (command - '0') * 25;  // Map 0-9 to 0-225
      analogWrite(motorPin, speed);
      Serial.print("Speed set to: ");
      Serial.println(speed);
    }
    // Simple commands
    else if (command == 'f') {
      analogWrite(motorPin, 200);  // Forward at ~78% speed
      Serial.println("Motor running forward");
    }
    else if (command == 's') {
      analogWrite(motorPin, 0);    // Stop
      Serial.println("Motor stopped");
    }
    else if (command == 'h') {
      Serial.println("Commands: 0-9 (speed), f (forward), s (stop)");
    }
  }
}
```

## Line-by-Line Explanation
- `const int motorPin = 9;`  
  Defines PWM pin. Must be one of: **3, 5, 6, 9, 10, 11** on Arduino Uno.

- `analogWrite(motorPin, speed);`  
  Sends PWM signal. **speed** ranges from **0** (off) to **255** (full speed).

- `Serial.available()` checks if a command was sent from Serial Monitor.

## PWM Frequency Note
Arduino's default PWM frequency is **~490Hz** for pins 5 and 6, and **~980Hz** for pins 3, 9, 10, 11. This is sufficient for most DC motors.

## Serial Commands
| Command | Action |
|---------|--------|
| `0-9`   | Sets speed to 0-225 (step 25) |
| `f`     | Runs motor at 200 speed |
| `s`     | Stops the motor |
| `h`     | Shows help menu |

## Sample Output on Serial Monitor
```
Motor Control Ready
Send numbers 0-255 to set speed
Or use commands: 'f' (forward), 's' (stop)
> 5
Speed set to: 125
> f
Motor running forward
> s
Motor stopped
```

---

## Additional Notes

### Using a Motor Driver (L298N) instead of MOSFET

| L298N Pin | Arduino Pin |
|-----------|-------------|
| ENA       | D9 (PWM)    |
| IN1       | D8          |
| IN2       | D7          |
| VCC       | 5V          |
| GND       | GND         |

Code for L298N:
```cpp
const int enA = 9;
const int in1 = 8;
const int in2 = 7;

void setup() {
  pinMode(enA, OUTPUT);
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
}

void loop() {
  // Set direction
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  
  // Set speed (0-255)
  analogWrite(enA, 150);
}
```

### Power Supply Warning
⚠️ **Never power a motor directly from Arduino's 5V pin** – it can damage the board. Use an external battery or power supply (3-12V depending on motor).

### Flyback Diode Protection
Always place a diode (1N4007) across motor terminals with the **cathode** (striped end) connected to VCC to prevent voltage spikes from damaging your components.

---

## Next Steps
- Add **direction control** (forward/reverse) using an H-bridge (L298N).
- Add **encoder** for precise speed measurement.
- Add **button control** instead of Serial.

Let me know if you want any of these extensions! 😊
