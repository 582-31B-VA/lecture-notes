# Class attributes

Attributes that are specific to a particular object are called
**instance attributes**. But some attribute values are _shared_ across
all objects of a given class. Such attributes, called **class
attributes**, are associated with the class itself, rather than any
individual instance of the class.

For instance, let us say that a bank pays interest on the balance of
accounts at a fixed interest rate. That interest rate may change, but it
is a single value shared across all accounts.

Class attributes are created by assignment statements in the suite of a
class statement, outside of any method definition. The following `class`
statement creates a class attribute for `Account` with the name
`interest`.

```python
class Account:
    interest = 0.02  # class attribute

    def __init__(self, account_holder: str):
        self.balance = 0  #instance attribute
        self.holder = account_holder  #instance attribute

    def deposit(self, amount: int) -> int:
        """Deposit the given amount."""
        self.balance = self.balance + amount
        return self.balance

    def withdraw(self, amount: int) -> int:
        """Withdraw the given amount."""
        if amount <= self.balance:
            self.balance = self.balance - amount
        return self.balance
```

This attribute can be accessed from any instance of the class, or from
the class itself.

```python
assert spock_acc.interest == 0.02
assert uhura_acc.interest == 0.02
assert Account.interest == 0.02
```

However, a single assignment statement to a class attribute changes the
value of the attribute for all instances of the class.

```python
Account.interest = 0.04
assert spock_acc.interest == 0.04
assert uhura_acc.interest == 0.04
```

## Class methods

**Class methods** are similar to class attributes in that they belong to
the class rather than to instances of the class. While methods like
`withdraw` or `deposit` must be called on a specific account, some
actions may be more general, pertaining instead to the domain as a
whole.

The `datetime` module from the standard library provides an example of
such an action. The `date` class has a `today` method that returns the
current local date. Because retrieving the current date is not a
behavior of any particular date instance, `today` is defined as a class
method.

```python
>>> from datetime import date
>>> date.today()
datetime.date(2025, 5, 19)
```

Notice how `today` is called on the `date` class rather than on a
specific instance of `date`.

Class methods are defined with the `@classmethod` decorator. Their first
parameter, conventionally named `cls`, is automatically bound to the
class on which they are called.

Let's define a class method that instantiates an account with a given
name and balance.

```python
class Account:
    ...

    @classmethod
    def from_balance(cls, name: str, balance: int) -> Account:
        """Instantiate a new account with the given balance."""
        acc = cls(name)
        acc.balance = balance
        return acc
```

To instantiate the account, the `cls` parameter is used instead of
`Account` to ensure that the correct class is used when `from_balance`
is called on a subclass of `Account` (we will discuss subclasses later).

## Static methods

Static methods serve a similar purpose to class methods, but they do not
have access to the class itself. Whereas class methods are often used to
provide alternative constructors, static methods are utility functions
related to the class's domain.

Like class methods, static methods belong to the class, not its
instances. They can access class methods and attributes, but not
instance variables.

For instance, we could define a static method for validating a given
name before creating the account:

```python
class Account:
    ...

    @staticmethod
    def name_is_valid(name: str) -> bool:
        """Determine if the given name is valid."""
        has_digit = any(char.isdigit() for char in name)
        return not has_digit
```

Notice that `name_is_valid` could just be a regular function. It doesn't
_have_ to be a method of `Account`, but since it is closely related to
its domain, defining it as a method makes sense.

## How to choose

Here are a couple of questions to help you decide between **instance**
versus **class** versus **static**:

- Will different instances have different values for the attribute?
  - Yes → instance attribute
  - No → class attribute

- Does this method need to access data stored in the instance (e.g.,
  `self.balance` or `self.name`)?
  - Yes → regular method
  - No → class or static method

- Does this method need to instanciate the class?
  - Yes → class method
  - No → regular or static method

- Could this method be declared as is outside the class (i.e., it
  doesn't use `self` nor the class)?
  - Yes → static method
  - No → regular or class method
