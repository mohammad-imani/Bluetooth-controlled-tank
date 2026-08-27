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
#include<softwareserial.h>


// Pin definition
const int PwmPin     = 10;  // PWM-capable pin (3,5,6,9,10,11 on Uno)
const int Motor_Pin1 = 9;
const int Motor_Pin2 = 8;
int pwm_signal = 255;

const int BLE_RX = 11;
const int BLE_TX = 12;

softwareserial BLEserial(BLE_RX,LE_TX);

void setup() {
  pinMode(  PwmPin   , OUTPUT);
  pinMode(Motor_Pin1 , OUTPUT);
  pinMode(Motor_Pin2 , OUTPUT);
  Serial.begin(9600);
  BLEserial.begin(9600);
}

void loop() {
  // Check for serial input
  if (Serial.available()) {
    char command = Serial.read();
    
    switch (command){
      case 'f' :
        digitalWrite(Motor_Pin1,HIGH);
        digitalWrite(Motor_Pin2,LOW);
        analogWrite(PwmPin,pwm_signal);
       break;
      case 'b' :
        digitalWrite(Motor_Pin1,LOW);
        digitalWrite(Motor_Pin2,HIGH);
        analogWrite(PwmPin,pwm_signal);
       break;
       case '+' :
         pwm_signal = pwm_signal + 20;
         if (pwm_signal>255){
            pwm_signal = 255;
         }
          analogWrite(PwmPin,pwm_signal);
        break;
        case '-' :
         pwm_signal = pwm_signal - 20;
         if (pwm_signal<55){
            pwm_signal = 55;
         }
          analogWrite(PwmPin,pwm_signal);
        break;
        case 's' :
        digitalWrite(Motor_Pin1,LOW);
        digitalWrite(Motor_Pin2,LOW);
       break;
};
}
```

## Line-by-Line Explanation
- `const int PwmPin = 9;`  
  Defines PWM pin. Must be one of: **3, 5, 6, 9, 10, 11** on Arduino Uno.

- `analogWrite(PwmPin, pwm_signal);`  
  Sends PWM signal. **pwm_signal** ranges from **0** (off) to **255** (full speed).

- `Serial.available()` checks if a command was sent from Serial Monitor.

## PWM Frequency Note
Arduino's default PWM frequency is **~490Hz** for pins 5 and 6, and **~980Hz** for pins 3, 9, 10, 11. This is sufficient for most DC motors.

## Serial Commands
| Command | Action |
|---------|--------|
| `f`     | Runs motor forward |
| `s`     | Stops the motor |
| `b`     | Runs motor backward |
| `+`     | increases the pwm signal +10 |
| `-`     | decreases the pwm signal -10 |



## Additional Notes


### Power Supply Warning
**Never power a motor directly from Arduino's 5V pin** – it can damage the board. Use an external battery or power supply (3-12V depending on motor).

### Flyback Diode Protection
Always place a diode (1N4007) across motor terminals with the **cathode** (striped end) connected to VCC to prevent voltage spikes from damaging your components.













