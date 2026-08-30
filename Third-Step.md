# Object oriented programming

Using classes in our programs makes our code more modular and readable, and it also makes debugging,
fixing, and changing things easier. And as we see in all the libraries of the language in question,
like softwareserial and others, they use classes, which confirms this point.<br>
for example in the softwareserial library when we want to create new rx and tx pins , we make that thing 
from the softwareserial class. <br>
here :  
```cpp
softwareserial BLEserial(RX_pin,TX_pin);
```
## creating class
to create class we use the "class" word and we declare it :
```cpp
class Motor{
  private:
    pass;
  public:
    pass;
};
```
what features does a motor have? <br>
1 - to "IN" pins <br>
2 - one "EN" pin for PWM signal <br>
3 - A value for the pwm signal <br>
those three pins must be constant but the value of pwm signal can change and it's better to set a defult value for pwm signal. <br>
it's recommended to declare our features or variable in private and the methods in public.<br>
for our motor, we have got 5 commands: two for duration of rotation , one for stop and two for changing the speed.<br>
we know this commands as methods, and we declare them in public:

```cpp
class Motor{
  private:
    int Pin_A;
    int Pin_B;
    int Pwm_Pin;
    int Pwm_signal = 150 ;
  public:
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
};
```
You might ask why the signal has been limited to 55, The answer is that, in reality, if the signal drops below a certain threshold, the motor will not rotate due to friction and will merely emit a buzzing sound. There is no specific rule governing the choice of the value 55; one must determine the appropriate setting for their specific device through trial and error.

### constructor
If you've noticed, when we defined a serial port object from the SoftwareSerial library, we wrote its members inside the parentheses right after it. But if we want to do the same thing for our motor class now, we'll run into an error. In C++, there's a neat way to do this. You define a function inside the class that has exactly the same name as the class, then you give this function as many parameters as there are members in the class and assign each parameter to the corresponding member.
```cpp
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
};
```
Now it's much easier to create an object. Without this feature, we would have to set each member one by one.
### Using our class 
Now that we have created the motor class, we can use it in our code to control it.<br>
At the beginning, we define the pins that we use for the motor we want. We could use the pin numbers in our code, but that's a mistake and makes our code really hard to read. Instead, we give each pin a name that is also used in real life and use that name. Keep in mind that these pins aren't something that are meant to change, so we should define them as constants.<br>
we must include the softwareserial library to creating a serial connection and resieving commands.

```cpp
#include <softwareserial>

const int BLE_RX = 11;
const int BLE_TX = 12;

const int Motor_pinA = 8;
const int Motor_pinB = 9;
const int Motor_Pwm = 10;

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
};

softwareserial BLEserial(BLE_RX , BLE_TX);

Motor Motor_A(Motor_pinA , Motor_pinB , Motor_Pwm);
```
Now we can use this two objects to make a control function. 

### Using objects
To create control logic, we must first define the pin types used by the microcontroller; this is done within the `setup` function.
UART-based communications—such as those involving the serial monitor or Bluetooth modules—operate at specific baud rates that can
be adjusted; therefore, it is important to specify these baud rates in the `setup` section as well.
```cpp
void setup(){

  Serial.begin(9600);
  BLEserial.begin(9600);
  
  pinMode(Pin_A , OUTPUT);
  pinMode(Pin_B , OUTPUT);
  pinMode(Pwm_Pin , OUTPUT);

}
```
In the next step, we enter the loop, receive commands via Bluetooth (if any), and perform the corresponding control operations.







