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
```
There are times when motors unexpectedly stop moving and emit a buzzing sound. In such cases, it may be useful to manually 
invoke the "shock" function; to this end, we add it to the control function.

```cpp
case 's':
  mR.Shock();
  mL.Shock();
break;
```
## Inrush current
Electronic boards are sensitive to their input voltage. If the voltage drops—even for a very brief period—it causes the module to reset or shut down. In this scenario, when the system is moving forward (`F`,`R`,`L`) and a command is immediately sent to reverse direction (`B`,`r`,`l`), an inrush current is drawn at the moment of execution; this leads to a voltage drop and causes the system to reset.<br>
To resolve this issue, we need to implement commands that handle general movement and check—upon receiving a new command—that it does not conflict with the current direction. If a conflict exists, we must briefly send a stop command to the motors before executing the new command.
To this end, we need to add a new method to the class to store the rotation direction of the motors. Additionally, within the control function, we must add a section to store the overall movement direction of the new command before executing it.

### Statue Method
In this method, PWM pins do not matter; only the direction-control pins are significant. We check the state of the pins: if the first pin is High and the second is Low, the rotation is forward; if the reverse is true, the rotation is backward; and if both pins are Low, the motor stops.
First of all we need to declare to variables to store the statue of motors.

```cpp
char R_statue;
char L_statue;
// in the class
void Statue(){
  if()

}






























