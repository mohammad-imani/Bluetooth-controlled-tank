### Tank Control Logic
In the previous logic section—which was designed to control just a single motor—we were limited to
clockwise and counter-clockwise rotation,stopping, and speed control. Here, we need to go a step further.
Here, we use either two or four motors—though the underlying logic remains the same—and the primary challenge lies
in synchronizing the left motor(s) with the right. For instance, to move forward, the left and right tracks must rotate
in the same direction and at the same speed; to turn, the speed of one track must differ from that of the other; and to
spin in place, the tracks must rotate in opposite directions.<br>

### making objects
First, we instantiate the necessary objects from their respective classes. Here, we utilize the library and class we created previously,
so I will not rewrite them again.<br>
At the beginning of this section of the code, we need to define three additional pins for the second motor—which I won't be writing out here.


```cpp

SoftwareSerial BLEserial(BLE_RX, BLE_TX);

Motor Motor_R(Motor_R1,Motor_R2,Motor_R_PWM);
Motor Motor_L(Motor_L1,Motor_L2,Motor_L_PWM);

void setup(){
  //will write in the final code
}
```
### the loop
As before, in this section too, we first need to receive commands via serial Bluetooth and then proceed with the subsequent steps.
```cpp
char command;

void loop(){
  if (BLEserial.available() > 0){
    command = BLEserial.read();

    // we will write the control logic here
  }
}
```
we use the `switch(command)` again.<br>
here we got more commands. Since the serial Bluetooth connection is not real-time, we are forced to use additional buttons for control instead
of a joystick. This latency issue the lack of real-time performance is even worse in BLE.

| char |     command      |
|------|------------------|
| `F`  |     Forward      |
| `B`  |     Backward     |
| `D`  |     Cockwise     |
| `d`  | Counter-clockwise|
| `S`  |       Stop       |
| `+`  |  speed increase  |
| `-`  |  speed decrease  |
| `Q`  |        LED       |
| `R`  |   Forward+Right  |
| `r`  |   Backward+Right |
| `L`  |   Forward+Left   |
| `l`  |   Backward+Left  |

We now enter `switch` and execute the commands in order from top to bottom.For each case, we add commands to send
a notification to the monitor—if connected—confirming the execution of each action, for the purpose of initial testing.<br><br>
`F` : To execute this command, we need to call the `Borward` methods for both motor objects.

```cpp
switch(command) {
  case 'F' :
    Motor_R.Forward();
    Motor_L.Forward();
    Serial.println("Tank is moving Forward");
  break;
```

`B` : To execute this command, we need to call the `Backward` methods for both motor objects.
```cpp
  case 'B' :
    Motor_R.Backward();
    Motor_L.Backward();
    Serial.println("Tank is moving Backward");
  break;
```

`D` : To execute this command, we need to call the `Forward` method for the Left Motor and call the `Backward` method for the Right Motor.
```cpp
  case 'D' :
    Motor_R.Backward();
    Motor_L.Forward();
    Serial.println("The Tank is rotating clockwise.");
  break;
```
`d` : To execute this command, we need to call the `Backward` method for the Left Motor and call the `Forward` method for the Right Motor.
```cpp
  case 'd' :
    Motor_R.Forward();
    Motor_L.Backward();
    Serial.println("The Tank is rotating counter-clockwise.");
  break;
```

`S` : To execute this command, we need to call the `Stop` methods for both motor objects.
```cpp
  case 'S' :
    Motor_R.Stop();
    Motor_L.Stop();
    Serial.println("The tank stopped.");
  break;
```

`+` : To increase the speed, we need to increase the value of `PWM_signal`; therefore, we use the `get_pwm_signal` method to retrieve the previous value, and then apply the new value to both motors using the `set_pwm_signal` method.
```cpp
  case '+' :
    Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() + 20);
    Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() + 20);
    Serial.println("The speed increased.");
  break;
```
`-` : To increase the speed, we need to decrease the value of `PWM_signal`; therefore, we use the `get_pwm_signal` method to retrieve the previous value, and then apply the new value to both motors using the `set_pwm_signal` method.
```cpp
  case '-' :
    Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() - 20);
    Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() - 20);
    Serial.println("The speed decreased.");
  break;
```
It is entirely up to you how much the speed changes with each + or - input, but I think 20 is a reasonable value.

`Q` : If you want to install an LED on your tank, you can power it directly from the Arduino pins using a resistor—provided if the LED has a low current draw; however, if it requires more power, you must use a 5V relay. Then, configure one of the pins as an output to control the relay or the LED.
```cpp
case 'Q' :
  if (digitalRead(RELY_PIN)){
    digitalWrite(RELY_PIN,LOW);
    Serial.println("LED Turned OFF");
    }
  else {
    digitalWrite(RELY_PIN,HIGH);
    Serial.println("LED Turned ON");
  }
  break;
```

For the next four commands—which relate to cornering—we must implement logic that introduces a speed differential between the left and right tracks upon receiving each command.For example, when we want to turn left, not only must both tracks rotate forward, but the speed of the right track must also be greater than that of the left track for the turn to occur.

`R` : this is the case for forward + right
```cpp
case 'R' :
  Motor_R.Forward();
  Motor_L.Forward();
  Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() - 20);
  Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() + 20);
  Serial.println("Turning right (forward)");
break;
```

`r` :
```cpp
case 'r' :
  Motor_R.Backward();
  Motor_L.Backward();
  Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() - 20);
  Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() + 20);
  Serial.println("Turning right (backward)");
break;
```
`L` :
```cpp
case 'L' :
  Motor_R.Forward();
  Motor_L.Forward();
  Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() + 20);
  Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() - 20);
  Serial.println("Turning left (forward)");
break;
```

`l` :
```cpp
case 'l' :
  Motor_R.Backward();
  Motor_L.Backward();
  Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() + 20);
  Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() - 20);
  Serial.println("Turning left (backward)");
break;
```
Now, the more we issue the turn command, the turning radius will become smaller.<br>
And this is the complete code:

```cpp
#include <SoftwareSerial.h>

const int BLE_RX = 11;
const int BLE_TX = 12;

const int Motor_R1 = 8;
const int Motor_R2 = 9;
const int Motor_R_PWM = 10;

const int Motor_L1 = 6;
const int Motor_L2 = 7;
const int Motor_L_PWM = 5;

const int LED_pin;

class Motor{
  private:
    int Pin_A;
    int Pin_B;
    int Pwm_Pin;
    int Pwm_signal = 150 ;
  public:
    Motor(int a,int b,int c){
      Pin_A = a;
      Pin_B = b;
      Pwm_Pin = c;
}
    void Forward(){
      digitalWrite( Pin_A , HIGH);
      digitalWrite( Pin_B , LOW );
      analogWrite(Pwm_Pin , Pwm_signal );
    }
    void Backward(){
      digitalWrite( Pin_B , HIGH);
      digitalWrite( Pin_A , LOW );
      analogWrite(Pwm_Pin , Pwm_signal );
    }
    void Stop(){
      digitalWrite( Pin_A , LOW);
      digitalWrite( Pin_B , LOW );
      analogWrite(Pwm_Pin , 0 );
    }
    void set_pwm_signal(int n){
      Pwm_signal = n;
      if (Pwm_signal > 255){
        Pwm_signal = 255;
      }
      if (Pwm_signal < 55){
        Pwm_signal = 55;
      }
      analogWrite(Pwm_Pin , Pwm_signal );
    }
    int get_pwm_signal(){
      return Pwm_signal;
    }
};


SoftwareSerial BLEserial(BLE_RX, BLE_TX);

Motor Motor_R(Motor_R1,Motor_R2,Motor_R_PWM);
Motor Motor_L(Motor_L1,Motor_L2,Motor_L_PWM);

void setup(){
  Serial.begin(9600);
  BLEserial.begin(9600);

  pinMode(Motor_R1, OUTPUT);
  pinMode(Motor_R2, OUTPUT);
  pinMode(Motor_R_PWM, OUTPUT);

  pinMode(Motor_L1, OUTPUT);
  pinMode(Motor_L1, OUTPUT);
  pinMode(Motor_L_PWM, OUTPUT);
}

char command;

void loop(){
  if (BLEserial.available() > 0){
    command = BLEserial.read();

    switch(command) {

  case 'F' :
    Motor_R.Forward();
    Motor_L.Forward();
    Serial.println("Tank is moving Forward");
  break;
  
  case 'B' :
    Motor_R.Backward();
    Motor_L.Backward();
    Serial.println("Tank is moving Backward");
  break;

  case 'D' :
    Motor_R.Backward();
    Motor_L.Forward();
    Serial.println("The Tank is rotating clockwise.");
  break;

  case 'd' :
    Motor_R.Forward();
    Motor_L.Backward();
    Serial.println("The Tank is rotating counter-clockwise.");
  break; 

  case 'S' :
    Motor_R.Stop();
    Motor_L.Stop();
    Serial.println("The tank stopped.");
  break;

  case '+' :
    Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() + 20);
    Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() + 20);
    Serial.println("The speed increased.");
  break;

  case '-' :
    Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() - 20);
    Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() - 20);
    Serial.println("The speed decreased.");
  break;

  case 'Q' :
    if (digitalRead(RELY_PIN)){
      digitalWrite(RELY_PIN,LOW);
      Serial.println("LED Turned OFF");
      }
    else {
      digitalWrite(RELY_PIN,HIGH);
      Serial.println("LED Turned ON");
    }
    break;

    case 'R' :
      Motor_R.Forward();
      Motor_L.Forward();
      Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() - 20);
      Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() + 20);
      Serial.println("Turning right (forward)");
    break;

    case 'r' :
      Motor_R.Backward();
      Motor_L.Backward();
      Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() - 20);
      Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() + 20);
      Serial.println("Turning right (backward)");
    break;

    case 'L' :
      Motor_R.Forward();
      Motor_L.Forward();
      Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() + 20);
      Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() - 20);
      Serial.println("Turning left (forward)");
    break;

    case 'l' :
      Motor_R.Backward();
      Motor_L.Backward();
      Motor_R.set_pwm_signal(Motor_R.get_pwm_signal() + 20);
      Motor_L.set_pwm_signal(Motor_L.get_pwm_signal() - 20);
      Serial.println("Turning left (backward)");
    break;
  }
}
```
### A Big Problem
If you test this code in real, you will notice a problem and that is when we send the `F` command,we need to see the tank moving forward and not in turn.
if we send one of those four commands for moving in turn, the pwm signal will become diffrent between left track and right one, and when we send the `F` , the motors will work with previous pwm signal wich they were not same, to solve this problem we will set a condition wich it chose the bigger value between pwm signals and set it for both motors when we send `F` or `B`

```cpp
case 'F' :
    Motor_R.Forward();
    Motor_L.Forward();
    if (Motor_R.get_pwm_signal() > Motor_L.get_pwm_signal()){
      Motor_R.set_pwm_signal(Motor_R.get_pwm_signal());
      Motor_L.set_pwm_signal(Motor_R.get_pwm_signal());
    }
    else {
      Motor_R.set_pwm_signal(Motor_L.get_pwm_signal());
      Motor_L.set_pwm_signal(Motor_L.get_pwm_signal());
    }
    Serial.println("Tank is moving Forward");
  break;
  
  case 'B' :
    Motor_R.Backward();
    Motor_L.Backward();
    if (Motor_R.get_pwm_signal() > Motor_L.get_pwm_signal()){
      Motor_R.set_pwm_signal(Motor_R.get_pwm_signal());
      Motor_L.set_pwm_signal(Motor_R.get_pwm_signal());
    }
    else {
      Motor_R.set_pwm_signal(Motor_L.get_pwm_signal());
      Motor_L.set_pwm_signal(Motor_L.get_pwm_signal());
    }
    Serial.println("Tank is moving Backward");
  break;
 ```
if set this conditions to our code , it will became ready to use. in the next step we will make our code cleaner.










