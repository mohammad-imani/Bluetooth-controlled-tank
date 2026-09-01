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
  
  
};
```
