# Methods

Each object class has an **interface**, which is the collection of
operations that external code can perform on it. To define the interface
of a class, we use **methods**. Object methods are defined by a `def`
statement in the suite of a class statement. Below, `deposit` and
`withdraw` are both defined as methods on objects of the `Account`
class.

```python
class Account:
    def __init__(self, account_holder: str):
        self.balance = 0
        self.holder = account_holder

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

While method definitions do not differ from function definitions in how
they are declared, they have a different effect when executed. The
function value that is created by a `def` statement within a `class`
statement is bound locally within the class as an attribute. That value
is invoked as a method using dot notation from an instance of the class.

```python
uhura_acc = Account('Uhura')
uhura_acc.deposit(100)
assert uhura_acc.balance == 100
```

Each method definition again includes a special first parameter `self`,
which is automatically bound to the object on which the method is
invoked. This object is called the **receiver**. For example, let us say
that `deposit` is invoked on a particular `Account` object and passed a
single argument value: the amount deposited. The object itself is bound
to `self`, while the argument is bound to `amount`. All invoked methods
have access to the object via the `self` parameter, and so they can all
access and manipulate the object's state.

When a method is invoked via dot notation, the object itself (bound to
`uhura_acc`, in this case) plays a dual role. First, it determines what
the name `withdraw` means; `withdraw` is not a name in the global
environment, but instead a name that is local to the `Account` class.
Second, it is bound to the first parameter `self` when the `withdraw`
method is invoked.
