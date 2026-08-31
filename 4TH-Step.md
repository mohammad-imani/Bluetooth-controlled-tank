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








