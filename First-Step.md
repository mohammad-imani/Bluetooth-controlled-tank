# Bluetooth Communication using SoftwareSerial

This example shows how to receive data from an Android phone (via HC-05 Bluetooth module) and display it on the Arduino Serial Monitor.

## Wiring Diagram
| Arduino Pin | HC-05 Module |
|-------------|--------------|
| 5V          | VCC          |
| GND         | GND          |
| D8 (RX)     | TX           |
| D9 (TX)     | RX           |

## Complete Code

```cpp:bluetooth_serial.ino
#include <SoftwareSerial.h>

// Define software serial pins (RX, TX)
SoftwareSerial BTSerial(8, 9); // RX = 8, TX = 9

void setup() {
  // Main serial monitor for debugging
  Serial.begin(9600);
  
  // Bluetooth module communication
  BTSerial.begin(9600);
  
  Serial.println("Ready to receive data from Bluetooth...");
}

void loop() {
  // If data is available from Bluetooth
  if (BTSerial.available()) {
    char data = BTSerial.read();  // Read one character
    Serial.print(data);           // Display on serial monitor
  }
  
  // (Optional) Send data from monitor to phone
  if (Serial.available()) {
    char data = Serial.read();
    BTSerial.print(data);
  }
}
```

## Line-by-Line Explanation
- `#include <SoftwareSerial.h>`  
  Library that simulates a serial port on regular digital pins.

- `SoftwareSerial BTSerial(8, 9);`  
  Creates a software serial object with pins 8 (RX) and 9 (TX).

- In `loop()`, `BTSerial.available()` checks if data is ready to be read.

## Important Note
On boards like **Arduino Uno**, **avoid using pins 0 and 1** (hardware serial) for Bluetooth, as they interfere with code uploading.

In many hc-05 boards , the level of RX and TX pins are 3.3V.
RX resives data and the TX sends the data, 
Arduino works with 5V and sends data with this level , but it can resieve data in 3.3V from RX , so there is no problem with connecting the RX in microcontroller and TX in Bluetooth module. but in other hand , Bluetooth RX pin can not resieve data in 5V,
so we must place a voltage divider between this pins.

### voltage divider 
The RX and TX pins are pulled up in any module. so we can use resistor to make a voltage divider easily.<br>
3.3 / 5 ≈ 2 / 3 <br>
we use this simple equation to make that.
the TX in Arduino is 5V , we connect three same resistors 
and connect it to the ground.<br>
TX -##--##--##-- Gnd<br>
TX = 5V , Gnd = 0V<br>
so the voltage is divided by resistors, as they are same , they will decrease the voltage in same value. number of the resistors is 3 , so each resistors decreases 5 / 3 V , it means :<br>
5V --##-- 3.3V -- ## -- 1.6V -- ## -- 0V
<br>
and it's done. it just needs to connect the RX in the Bluetooth module next than first resistor , in that place the voltage is 3.3


### Connecting problem
Sometimes the boards we buy are not orginal classic bluetooth serial. sometimes they are BLE (Bluetooth Low Energy), and it will make an issue.
the problem is: mostly applications we download for serial connection , are made for classic bluetooth, so we can't connect our board. the soloution it searching BLE instead bluetooth serial and find applications for BLE.

### resieving wrong data
The bluetooth module sends characters with their ASCCI code. for example when you send 'A' , the application sends number 65 and the module knows its a char and change it into the char and you will see the 'A' , but when you send 65 , the app sends '6' and '5' or 54 and 53 and you will see the 54 and 53 in diffrent lines. we can change this number to string again but it's very simple to use characters.
