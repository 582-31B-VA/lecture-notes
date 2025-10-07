# Exceptions

Programmers must be always mindful of possible errors that may arise in
their programs. Examples abound: a function may not receive arguments
that it is designed to accept, a necessary resource may be missing, or a
connection across a network may be lost. When designing a program, one
must anticipate the exceptional circumstances that may arise and take
appropriate measures to handle them.

There is no single correct approach to handling errors in a program.
Programs designed to provide some persistent service like a web server
should be robust to errors, logging them for later consideration but
continuing to service new requests as long as possible. On the other
hand, the Python interpreter handles errors by terminating immediately
and printing an error message, so that programmers can address issues as
soon as they arise. In any case, programmers must make conscious choices
about how their programs should react to exceptional conditions.

**Exceptions** provides a general mechanism for adding error-handling
logic to programs. **Raising** an exception is a technique for
interrupting the normal flow of execution, signaling that some
exceptional circumstance has arisen, and returning directly to an
enclosing part of the program that was designated to react to that
circumstance.

## Raising exceptions

We've seen previously how to raise `ValueError` exceptions, but programs
may also define their own types of exceptions by creating a new class
that inherits from the built-in `Exception` class. These custom
exceptions let us be more precise when an error occurs. For example, we
may want to distinguish a bank account having insufficient funds versus
the account being blocked for other reasons.

Exception classes can be defined which do anything any other class can
do, but are usually kept simple, often only offering a number of
attributes that allow information about the error to be extracted by
handlers for the exception. Most exceptions are defined with names that
end in "Error", similar to the naming of the standard exceptions.

For example:

```python
class InsufficientFundsError(Exception):
    pass
```

We can then raise this exception with the `raise` construct:

```python
class Account:
    ...

    def withdraw(self, amount: int) -> int:
        """Withdraw the given amount."""
        if amount <= self.balance:
            raise InsufficientFundsError(f"amount: {amount}, balance: {self.balance}")
        return self.balance
```

When an exception is raised, no further statements in the current block
of code are executed. Unless the exception is handled, the program will
crash. In addition, the interpreter will print a stack backtrace, which
is a structured block of text that describes the nested set of active
function calls in the branch of execution in which the exception was
raised.

## Handling exceptions

An exception can be handled by an enclosing `try` statement. A `try`
statement consists of multiple clauses; the first begins with `try` and
the rest begin with `except`:

```
try:
    <try suite>
except <exception class> as <name>:
    <except suite>
...
```

The `<try suite>` is always executed immediately when the `try`
statement is executed. Suites of the `except` clauses are only executed
when an exception is raised during the course of executing the
`<try suite>`. Each `except` clause specifies the particular class of
exception to handle. For instance, if the `<exception class>` is
`AssertionError`, then any instance of a class inheriting from
`AssertionError` that is raised during the course of executing the
`<try suite>` will be handled by the following `<except suite>`. Within
the `<except suite>`, the identifier `<name>` is bound to the exception
object that was raised, but this binding does not persist beyond the
`<except suite>`.

For example, we can handle the `InsufficientFundsError` exception using
a `try` statement that displays a message to the user:

```python
try:
    Account("bob").withdraw(10)
except InsufficientFundsError as e:
    print(f"Insufficient funds: {e}")
```

Note that _where_ to catch an exception depends on if the caller knows
or not how to handle the error. Consider a program in which `withdraw`
is called by an auxiliary function, itself called in turn by an HTTP
request handler. It makes sense to let the handler catch the exception
since it knows how to render an error to the client, whereas the
auxiliary function does not.
