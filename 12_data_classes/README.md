## Data classes

Creating classes often involves a lot of boilerplate code. In addition
to a constructor (`__init__`), a typical class may need to implement
special methods to define its string representation (`__str__`,
`__repr__`) and how it interacts with various operators (`__eq__`,
`__add__`, etc.). While this effort is worthwhile for classes with
complex behavior, sometimes we just need to define a simple object type
to hold data. In Python, this is what **data classes** are for.

A data class is a class decorated with `dataclasses.dataclass`. This
decorator automatically adds method definitions to the class to support
instance initialization, string representation and comparison methods.
It also allows us to declare and bind attributes using variable
annotations.

Here's an example of a data class that represent inventory items:

```python
from dataclasses import dataclass

@dataclass
class InventoryItem:
    name: str
    price: float
    quantity: int = 0

    def total_cost(self) -> float:
        return self.price * self.quantity


item = InventoryItem("apple", 1.25)
```

In a data class, variables defined in the class body, such as `name`,
`price` and `quantity`, automatically become instance attributes. Even
though we didn't write it ourselves, `InventoryItem` implements the
following constructor:

```python
def __init__(self, name: str, price: float, quantity: int = 0):
    self.name = name
    self.price = price
    self.quantity = quantity
```

Data classes can also have class attributes. To declare one, we use the
`typing.ClassVar` type annotation. Let's add a `next_id` class variable
to `InventoryItem` so that we can generate a unique ID for each
instance:

```python
from dataclasses import dataclass, field
from typing import ClassVar

@dataclass
class InventoryItem:
    name: str
    price: float
    quantity: int = 0
    next_id: ClassVar[int] = 0   # new class attribute
    id: int = field(init=False)  # new instance attribute

    # new special method
    def __post_init__(self):
        self.id = next_id
        InventoryItem.next_id += 1
```

The `init` parameter of the the `dataclasses.field` function allows us
to exclude `id` from the constructor, while the `__post_init__` special
method is used to execute additional initialization logic after
`__init__` is called.

`field` can also be used to set the initial value of mutable attributes
(e.g., lists, dictionaries, and so on). For example, let's add another
class variable named `all` to store all the inventory items:

```python
from dataclasses import dataclass
from typing import ClassVar

@dataclass
class InventoryItem:
    name: str
    price: float
    quantity: int = 0
    next_id: ClassVar[int] = 0  
    id: int = field(init=False) 
    all: ClassVar[list[InventoryItem]] = field(default_factory=list) # new

    # new special method
    def __post_init__(self):
        self.id = next_id
        InventoryItem.next_id += 1
        InventoryItem.all.append(self) # new
```

The value of the `default_factory` parameter must be a function (in this
case, the `list` constructor) that returns the initial value.
