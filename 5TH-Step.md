## Making Cleaner
In our code the control part is not modular and clean, in this step we will write it into a function and call the function in the loop.<br>
The variables or parameters we used in the control section consisting of two motors of the same class and a command, this are the first elements we
consider when constructing this function.
```cpp
void Control(Motor & mR , Motor & mL , char c);
```
