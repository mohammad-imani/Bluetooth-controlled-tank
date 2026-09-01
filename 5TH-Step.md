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
    Serial.println("Tank is moving Forward");
  break;
  
  case 'B' :
    mR.Backward();
    mL.Backward();
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

    case 'R' :
      mR.Backward();
      mL.Backward();
      mR.set_pwm_signal(mR.get_pwm_signal() - 20);
      mL.set_pwm_signal(mL.get_pwm_signal() + 20);
      Serial.println("Turning right (backward)");
    break;

    case 'R' :
      mR.Forward();
      mL.Forward();
      mR.set_pwm_signal(mR.get_pwm_signal() + 20);
      mL.set_pwm_signal(mL.get_pwm_signal() - 20);
      Serial.println("Turning left (forward)");
    break;

    case 'R' :
      mR.Backward();
      mL.Backward();
      mR.set_pwm_signal(mR.get_pwm_signal() + 20);
      mL.set_pwm_signal(mL.get_pwm_signal() - 20);
      Serial.println("Turning left (backward)");
    break;
    }
};
```
