# Enums

As discussed previously, one of the reasons for declaring an attribute
private is to enforce restrictions on its value. The balance of a bank
account, for instance, must always be positive. By making the balance a
private attribute and forcing client code to change it via method calls,
we can ensure its value is always greater than 0.

Because numbers and strings have infinite domains, validating them often
involves some kind of _rule_: balances must be greater than 0, usernames
cannot contain whitespace, passwords must have at least one digit, etc.
But some information is more restricted. A task, for example, is either
_done_ or _not done_. To represent this fact, we could use strings
(`"done"` and `"not done"`) or numbers (`0` and `1`), but the infinite
domains of these types betray the constrained nature of the information.
Since there are only two options, it is better to use a boolean — a type
with just two values: true and false.

The real world is full of such information, but not all is limited to
two options. A traffic light, for instance, can be either green, yellow
or red. The four cardinal directions are north, east, south, west. The
days of the week are Monday, Tuesday, Friday, etc. To represent these
predetermined selections of values, we use **enums** (short for
"enumeration").

In Python, an enum is a set of symbolic names bound to unique values.
Creating an enum is as simple as writing a class that inherits from
`enum.Enum` (we will cover inheritance shortly).

```python
from enum import Enum


class TrafficLight(Enum):
    GREEN = 1 
    YELLOW = 2 
    RED = 3


class Direction(Enum):
    NORTH = 1 
    EAST = 2 
    SOUTH = 3
    WEST = 4
```

As you can see, we use class attributes to define the options (or
**members**) of an enumerations. Since members are constants, their name
is in upper case. In many cases, we don't care about the actual value of
each member. You can use integers, as shown above, or descriptive
strings.

To refer to a member, we use the dot operator. The `next_light` function
below, for instance, can be applied on a member of `TrafficLight` to get
the member that comes after.

```python
def next_light(current: TrafficLight) -> TrafficLight:
    """Return the next traffic light."""
    if current is TrafficLight.GREEN:
        return TrafficLight.YELLOW
    if current is TrafficLight.YELLOW:
        return TrafficLight.RED
    if current is TrafficLight.RED:
        return TrafficLight.GREEN


assert next_light(TrafficLight.GREEN) is TrafficLight.YELLOW
assert next_light(TrafficLight.YELLOW) is TrafficLight.RED
```

Note that we use `is` to compare members, and that we never instantiate
the class. Enums are used solely to group related options.
