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


## Working with L298N module
this module is based on L298N IC. <br>
It has got three pins for power , six pins for control and four pins for two motor.

| Pin Name  |  Work |
|-----------|-------|
|   `ENA`   | PWM signal for motor A|
|   `ENB`   | PWM signal for motor B|
|`in1 , in2`| motor A control pins  |
|`in3 , in4`| motor B control pins  |
|   `vcc`   | input power 5V-28V    |
|   `GND`   | ground pin            |
|   `5V`    | 5V logic power supply |
| `out 1-2` | output for motor A    |
| `out 3-4` | output for motor B    |

In this module, there are three pins for input. One is ground, one is 5-28 volts,
and the other is connected to a 5-volt regulator that is activated with a jumper 
on the module and is used to power logic circuits like Arduino.<br>

There are also 8 pins for controlling the motors, 6 of which are in a row and the other two are above the side pins, usually connected to ENA and ENB with a jumper. These two pins are 5 volts and allow the motors to operate when connected. However, to send a square wave, the jumpers need to be removed and a PWM signal should be sent to ena and enb. The pins in1-2-3-4 are used for clockwise or counterclockwise rotation, braking, or letting the motors free. 1 and 2 are for the first motor, and the others are for the next motor. If the first pin is high and the second low, the motor will rotate clockwise. Reversing this will change the direction of rotation. If both are high, the motor is in brake mode, and if both are low, the motor is in free mode.<br>

OUT1/2/3/4 : this pins are for motor connection, Numbers 1 and 2 are for the first motor, and 3 and 4 are for the second motor. It’s normal that swapping these pins won’t affect performance and will only change the direction of rotation.






