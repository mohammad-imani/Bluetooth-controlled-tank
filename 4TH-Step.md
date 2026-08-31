### Tank Control Logic
In the previous logic section—which was designed to control just a single motor—we were limited to
clockwise and counter-clockwise rotation,stopping, and speed control. Here, we need to go a step further.
Here, we use either two or four motors—though the underlying logic remains the same—and the primary challenge lies
in synchronizing the left motor(s) with the right. For instance, to move forward, the left and right tracks must rotate
in the same direction and at the same speed; to turn, the speed of one track must differ from that of the other; and to
spin in place, the tracks must rotate in opposite directions.<br>

### making objects
First, we instantiate the necessary objects from their respective classes. Here, we utilize the library and class we created previously,
so I will not rewrite them again.
```cpp
SoftwareSerial BLEserial(RX_Pin, TX_Pin);

Motor Motor_A()
