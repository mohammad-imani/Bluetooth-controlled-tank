## Making Cleaner
In our code the control part is not modular and clean, in this step we will write it into a function and call the function in the loop.<br>
The variables or parameters we used in the control section consisting of two motors of the same class and a command, this are the first elements we
consider when constructing this function.
```cpp
void Control(Motor & mR , Motor & mL , char c);
```
Now, we place that same control section inside this function, with the difference that we use the names `mR` and `mL` instead of `Motor_R` and `Motor_L`, and we use `c` instead of the `command`.
```cpp
void Control(Motor & mR , Motor & mL , char c){

   switch(c) {

  case 'F' :
    mR.Forward();
    mL.Forward();
    if (mR.get_pwm_signal() > mL.get_pwm_signal()){
      mR.set_pwm_signal(mR.get_pwm_signal());
      mL.set_pwm_signal(mR.get_pwm_signal());
    }
    else {
      mR.set_pwm_signal(mL.get_pwm_signal());
      mL.set_pwm_signal(mL.get_pwm_signal());
    }
    Serial.println("Tank is moving Forward");
  break;
  
  case 'B' :
    mR.Backward();
    mL.Backward();
    if (mR.get_pwm_signal() > mL.get_pwm_signal()){
      mR.set_pwm_signal(mR.get_pwm_signal());
      mL.set_pwm_signal(mR.get_pwm_signal());
    }
    else {
      mR.set_pwm_signal(mL.get_pwm_signal());
      mL.set_pwm_signal(mL.get_pwm_signal());
    }
    Serial.println("Tank is moving Backward");
  break;

  case 'D' :
    mR.Backward();
    mL.Forward();
    Serial.println("The Tank is rotating clockwise.");
  break;

  case 'd' :
    mR.Forward();
    mL.Backward();
    Serial.println("The Tank is rotating counter-clockwise.");
  break; 

  case 'S' :
    mR.Stop();
    mL.Stop();
    Serial.println("The tank stopped.");
  break;

  case '+' :
    mR.set_pwm_signal(mR.get_pwm_signal() + 20);
    mL.set_pwm_signal(mL.get_pwm_signal() + 20);
    Serial.println("The speed increased.");
  break;

  case '-' :
    mR.set_pwm_signal(mR.get_pwm_signal() - 20);
    mL.set_pwm_signal(mL.get_pwm_signal() - 20);
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
      mR.Forward();
      mL.Forward();
      mR.set_pwm_signal(mR.get_pwm_signal() - 20);
      mL.set_pwm_signal(mL.get_pwm_signal() + 20);
      Serial.println("Turning right (forward)");
    break;

    case 'r' :
      mR.Backward();
      mL.Backward();
      mR.set_pwm_signal(mR.get_pwm_signal() - 20);
      mL.set_pwm_signal(mL.get_pwm_signal() + 20);
      Serial.println("Turning right (backward)");
    break;

    case 'L' :
      mR.Forward();
      mL.Forward();
      mR.set_pwm_signal(mR.get_pwm_signal() + 20);
      mL.set_pwm_signal(mL.get_pwm_signal() - 20);
      Serial.println("Turning left (forward)");
    break;

    case 'l' :
      mR.Backward();
      mL.Backward();
      mR.set_pwm_signal(mR.get_pwm_signal() + 20);
      mL.set_pwm_signal(mL.get_pwm_signal() - 20);
      Serial.println("Turning left (backward)");
    break;
    }
};
```
### complete code
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

void Control(Motor & mR , Motor & mL , char c){

   switch(c) {

  case 'F' :
    mR.Forward();
    mL.Forward();
    if (mR.get_pwm_signal() > mL.get_pwm_signal()){
      mR.set_pwm_signal(mR.get_pwm_signal());
      mL.set_pwm_signal(mR.get_pwm_signal());
    }
    else {
      mR.set_pwm_signal(mL.get_pwm_signal());
      mL.set_pwm_signal(mL.get_pwm_signal());
    }
    Serial.println("Tank is moving Forward");
  break;
  
  case 'B' :
    mR.Backward();
    mL.Backward();
    if (mR.get_pwm_signal() > mL.get_pwm_signal()){
      mR.set_pwm_signal(mR.get_pwm_signal());
      mL.set_pwm_signal(mR.get_pwm_signal());
    }
    else {
      mR.set_pwm_signal(mL.get_pwm_signal());
      mL.set_pwm_signal(mL.get_pwm_signal());
    }
    Serial.println("Tank is moving Backward");
  break;

  case 'D' :
    mR.Backward();
    mL.Forward();
    Serial.println("The Tank is rotating clockwise.");
  break;

  case 'd' :
    mR.Forward();
    mL.Backward();
    Serial.println("The Tank is rotating counter-clockwise.");
  break; 

  case 'S' :
    mR.Stop();
    mL.Stop();
    Serial.println("The tank stopped.");
  break;

  case '+' :
    mR.set_pwm_signal(mR.get_pwm_signal() + 20);
    mL.set_pwm_signal(mL.get_pwm_signal() + 20);
    Serial.println("The speed increased.");
  break;

  case '-' :
    mR.set_pwm_signal(mR.get_pwm_signal() - 20);
    mL.set_pwm_signal(mL.get_pwm_signal() - 20);
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
      mR.Forward();
      mL.Forward();
      mR.set_pwm_signal(mR.get_pwm_signal() - 20);
      mL.set_pwm_signal(mL.get_pwm_signal() + 20);
      Serial.println("Turning right (forward)");
    break;

    case 'r' :
      mR.Backward();
      mL.Backward();
      mR.set_pwm_signal(mR.get_pwm_signal() - 20);
      mL.set_pwm_signal(mL.get_pwm_signal() + 20);
      Serial.println("Turning right (backward)");
    break;

    case 'L' :
      mR.Forward();
      mL.Forward();
      mR.set_pwm_signal(mR.get_pwm_signal() + 20);
      mL.set_pwm_signal(mL.get_pwm_signal() - 20);
      Serial.println("Turning left (forward)");
    break;

    case 'l' :
      mR.Backward();
      mL.Backward();
      mR.set_pwm_signal(mR.get_pwm_signal() + 20);
      mL.set_pwm_signal(mL.get_pwm_signal() - 20);
      Serial.println("Turning left (backward)");
    break;
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

    Control(Moto_R , Motor_L , command );

  }
}
