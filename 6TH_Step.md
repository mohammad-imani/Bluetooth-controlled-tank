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
  analogWrite(pwm_pin,255);
  delay(20);
  analogWrite(pwm_pin,pwm_signal);
}
```
Now, within the control function, we need to call this method in all sections where the pwm signal is modified.





