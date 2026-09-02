# Practical problems
In my opinion, this is the most important part of the work. Generally, writing code that performs the required task
might not be difficult; but the practical challenges we encounter—and the solutions we are compelled to devise for them—represent
the most valuable aspect of building toys like this.
### Shock Method
At low speeds—for instance, when sending a negative command—the motors might stall. To prevent this, we need to send a signal value
of 255 for a very brief moment—imperceptible to us—and then immediately revert to the previous value. This ensures much smoother operation.
To do this, we create a `Shock` method in the `public` section of the `class`.

```cpp
void Shock(){
  analogWrite(Pwm_Pin,255);
  delay(20);
  analogWrite(Pwm_Pin,Pwm_signal);
}
```
Now, within the control function, we need to call this method in all sections.
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
    mR.Shock();
    mL.Shock();
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
    mR.Shock();
    mL.Shock();
    Serial.println("Tank is moving Backward");
  break;

  case 'D' :
    mR.Backward();
    mL.Forward();
    mR.Shock();
    mL.Shock();
    Serial.println("The Tank is rotating clockwise.");
  break;

  case 'd' :
    mR.Forward();
    mL.Backward();
    mR.Shock();
    mL.Shock();
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
    mR.Shock();
    mL.Shock();
    Serial.println("The speed increased.");
  break;

  case '-' :
    mR.set_pwm_signal(mR.get_pwm_signal() - 20);
    mL.set_pwm_signal(mL.get_pwm_signal() - 20);
    mR.Shock();
    mL.Shock();
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
      mR.Shock();
      mL.Shock();
      Serial.println("Turning right (forward)");
    break;

    case 'r' :
      mR.Backward();
      mL.Backward();
      mR.set_pwm_signal(mR.get_pwm_signal() - 20);
      mL.set_pwm_signal(mL.get_pwm_signal() + 20);
      mR.Shock();
      mL.Shock();
      Serial.println("Turning right (backward)");
    break;

    case 'L' :
      mR.Forward();
      mL.Forward();
      mR.set_pwm_signal(mR.get_pwm_signal() + 20);
      mL.set_pwm_signal(mL.get_pwm_signal() - 20);
      mR.Shock();
      mL.Shock();
      Serial.println("Turning left (forward)");
    break;

    case 'l' :
      mR.Backward();
      mL.Backward();
      mR.set_pwm_signal(mR.get_pwm_signal() + 20);
      mL.set_pwm_signal(mL.get_pwm_signal() - 20);
      mR.Shock();
      mL.Shock();
      Serial.println("Turning left (backward)");
    break;
    }
};





