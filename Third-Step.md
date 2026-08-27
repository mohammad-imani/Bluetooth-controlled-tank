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


