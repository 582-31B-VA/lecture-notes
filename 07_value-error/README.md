# Value error

Up until now, when a caller would pass an invalid argument to one of our
methods, the method would _fail silently_ — that is to say, it would do
nothing. The `withdraw` method below, for instance, withdraws the given
amount only if that amount is lesser or equal to the account's balance.
Otherwise, it does nothing but returning the balance. We don't warn the
caller about the amount being too high.

```python
class Account:
   def __init__(self, account_holder: str):
       self._balance = 0
       self.holder = account_holder

   def withdraw(self, amount: int) -> int:
       """Withdraw the given amount."""
       if amount <= self._balance:
           self._balance = self._balance - amount
       return self.balance
```

To fail silently like this is generally considered a bad approach for
handling errors. When something goes wrong while executing a program
(and something _always_ goes wrong), we want to know as soon as possible
so that we can react appropriately.

In Python, the easiest way to warn a caller about an invalid argument is
is to raise a `ValueError` **exception**, like so:

```python
def withdraw(self, amount: int) -> int:
    """Withdraw the given amount."""
    if amount > self._balance:
        raise ValueError("insufficient funds")    
    self._balance = self._balance - amount
    return self.balance
```

Here, we have flipped the conditional: if the given amount is greater
than the account's balance, raise an error; otherwise, withdraw the
funds. To raise the error, we use the `raise` keyword, which acts like a
supercharged `return`. Not only does it stop the execution of the
current function, but it crashes the the program and displays an error
message if not caught by the caller.

Eventually, we will learn how to _catch_ exceptions, but for now,
instead of doing nothing when client code uses an interface incorrectly,
we can use `raise` and `ValueError` to display an error message.
