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
### Using our class 


