# Classes

A **class** serves as a template for all objects whose type is that
class. Every object is an **instance** of some particular class. The
objects we have used so far all have built-in classes, but new
user-defined classes can be created as well.

User-defined classes are created by `class` statements, which consist of
a single clause. A `class` statement defines the class name, then
includes a suite of statements to define **attributes**. An attribute is
a name-value pair accessible via dot notation.

Classes are typically organized around manipulating attributes. The
class specifies the attributes of its objects by defining a **method**
for **initializing** new objects. A method is an attribute whose value
is a function. The method that initializes objects has a special name in
Python, `__init__` (two underscores on each side of the word "init"),
and is called the **constructor** for the class.

Consider the following `Account` class, used to create bank account
objects, each with a holder and a balance:

```python
class Account:
    def __init__(self, account_holder: str):
        self.balance = 0
        self.holder = account_holder
```

The `__init__` method for `Account` has two parameters. The first one,
`self`, is automatically bound to the newly created object. The second
parameter, `account_holder`, is bound to the argument passed to the
class when the constructor is called. There is no need to annotate the
type of `self` nor the return value of `__init__` since they always
correspond to the current class — in this case: `Account`.

The `Account` class allows us to create multiple instances of bank
accounts. The act of creating a new object instance is known as
**instantiating** the class.

Having defined the `Account` class, we can instantiate it. The syntax in
Python for instantiating a class is identical to the syntax of calling a
function. In this case, we call `Account` with the argument `'Kirk'`,
the account holder's name:

```python
kirk_acc = Account('Kirk')
```

This "call" to the `Account` class creates a new object that is an
instance of `Account`, then calls the constructor function `__init__`
with two arguments: the newly created object and the string `'Kirk'`.

The constructor for the `Account` class binds the attribute name
`balance` to `0`. It also binds the attribute name `holder` to the value
of `account_holder`. Note that the parameter `account_holder` is a local
name in the `__init__` method, whereas `holder` is stored as an
attribute of `self` using dot notation.

Now, we can access the object's `balance` and `holder` using dot
notation.

```python
assert kirk_acc.holder == 'Kirk'
assert kirk_acc.balance == 0
```

## Identity

Each new `Account` instance has its own `holder` and `balance`
attribute, the value of which is independent of other objects of the
same class.

```python
spock_acc = Account('Spock')
assert spock_acc.holder == 'Spock'
```

To enforce this separation, every object that is an instance of a
user-defined class has a unique _identity_. Object identity is compared
using the `is` and `is not` operators.

```python
assert kirk_acc is kirk_acc
assert kirk_acc is not spock_acc
```

Despite being constructed from calling the same method, the objects
bound to `kirk_acc` and `spock_acc` are not the same.

As usual, binding an object to a new name using assignment does not
create a new object. New objects that have user-defined classes are only
created when a class (such as `Account`) is instantiated with call
expression syntax.

```python
alias = kirk_acc
assert alias is kirk_acc
```
