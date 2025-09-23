# Properties

What if you want to change the visibility of an attribute from public to
private? Or what if you want to allow client code to read an attribute
without setting its value? In Python, this what **properties** are for.

A property is a _synthetic_ attribute used to manage access to an
object's data, or to compute values dynamically. The syntax for reading
and writing the value of a property is the same as the one used for
regular attributes, but reads and writes are handled by decorated
methods.

Consider a class for handling temperatures:

```python
class Temperature:
    def __init__(self, celsius: float):
        self.celcius = celsius;


t = Temperature(100)
```

The temperature is stored in degrees Celsius, but we may want to read
and write the temperature in degrees Fahrenheit as well. To do so, we
create **getter** and **setter** methods to automatically convert from
one unit to the other.

```python
class Temperature:
    def __init__(self, celsius: float):
        self.celsius = celsius;

    @property
    def fahrenheit(self):
        return self.celsius * 1.8 + 32

    @fahrenheit.setter
    def fahrenheit(self, value):
        self.celsius = (value - 32) / 1.8


t = Temperature(100)
assert t.fahrenheit == 212.0
t.fahrenheit = 32
assert t.celsius == 0.0
```

In Python, a getter is a method decorated with `@property`. It receives
no argument apart from the instance on which it is called (`self`), and
the value it returns is the value of the property (in this case,
`fahrenheit`).

A setter is a method decorated with `@<property>.setter` where
`<property>` is the name of the property. In addition to `self`, it
receives the `value` to which the property was bound to.
