# Inheritance

When working in the object-oriented programming paradigm, we often find
that different types are related. In particular, we find that similar
classes differ in their amount of specialization. Two classes may have
similar attributes, but one represents a special case of the other.

Consider our `Account` class:

```python
class Account:
    interest = 0.02

    def __init__(self, account_holder: str):
        self.balance = 0
        self.holder = account_holder

    def deposit(self, amount: int) -> int:
        """Deposit the given amount."""
        self.balance = self.balance + amount
        return self.balance

    def withdraw(self, amount: int) -> int:
        """Withdraw the given amount."""
        if amount > self._balance:
            raise ValueError("insufficient funds")    
        self._balance = self._balance - amount
        return self.balance
```

We may want to implement a `CheckingAccount`, which is _partially_
different from a standard account. A checking account charges an extra
$1 for each withdrawal and has a lower interest rate, but is otherwise
the same as a standard account. In OOP terminology, we say that
`Account` is the _base class_ (or _parent class_, or _superclass_) of
`CheckingAccount`, while `CheckingAccount` is a _subclass_ (or _child
class_) of `Account`.

A subclass _inherits_ the attributes of its base class, but may override
certain attributes, including certain methods. With inheritance, we only
specify what is different between the subclass and the base class.
Anything that we leave unspecified in the subclass is automatically
assumed to behave just as it would for the base class.

Inheritance is meant to represent **is-a** relationships between
classes, which contrast with **has-a** relationships. A checking account
_is a_ specific type of account, so having a `CheckingAccount` inherit
from `Account` is an appropriate use of inheritance. On the other hand,
a bank _has a_ list of bank accounts that it manages, so neither should
inherit from the other. Instead, a list of account objects would be
naturally expressed as an instance attribute of a bank object.

In Python, we specify inheritance by placing an expression that
evaluates to the base class in parentheses after the class name:

```python
class CheckingAccount(Account):
    withdraw_charge = 1
    interest = 0.01

    def withdraw(self, amount) -> int:
        """Withdraw the given amount."""
        return Account.withdraw(self, amount + self.withdraw_charge)
```

Here, we introduce a class attribute `withdraw_charge` that is specific
to the `CheckingAccount` subclass, and assign a lower value to the
`interest` attribute. We also define a new `withdraw` method to override
the behavior defined in the `Account` class. With no further statements
in the class suite, all other behavior is inherited from the base class
`Account`.

Attributes that have been overridden are still accessible via the class
itself. For instance, we implemented the `withdraw` method of
`CheckingAccount` by calling the `withdraw` method of `Account` and
manually passing `self`.

Notice that we accessed the class variable `withdraw_charge` on `self`
rather than the equivalent `CheckingAccount.withdraw_charge`. The
benefit of the former over the latter is that a class that inherits from
`CheckingAccount` might override the withdrawal charge. If that is the
case, we would like our implementation of `withdraw` to find that new
value instead of the old one.

Here is how to use `CheckingAccount` once the class has been defined:

```python
bones_acc = CheckingAccount("Leonard McCoy")
assert bones_acc.interest == 0.01  # lower interest rate
bones_acc.deposit(20)  # deposits are the same
assert bones_acc.withdraw(10) == 9  # withdrawals decrease balance by an extra charge
```

A subclass can also override the constructor of its base class. For
instance, consider a `JointAccount` with two holders:

```python
class JointAccount(Account):
    def __init__(self, account_holder: str, account_holder_2: str):
        Account.__init__(self, account_holder)
        self.holder_2 = account_holder_2
```

Note that we manually call the constructor of `Account` to initialize
`holder` and `balance`, then define `holder_2`, which is only accessible
on instances of `JointAccount`.
