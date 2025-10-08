# Special methods

A **polymorphic function** is a function that can be applied to many
(_poly_) different forms (_morph_) of data. It allows programmers to
build and use abstract data representations where the same interface can
operate on values of various types.

Inheritance is one way to implement polymorphism. If both `Account` and
`CheckingAccount` implement a `withdraw` method, for instance, then
`withdraw` is considered polymorphic because it can be invoked with
objects of two different types.

The built-in `print` function is another example of a polymorphic
function. It can be invoked seamlessly with all kinds of values:
numbers, strings, dates, time deltas, etc.

```python
>>> print(1)
1
>>> print("abc")
abc
>>> import datetime
>>> print(datetime.date.today())
2025-06-04
>>> print(datetime.timedelta(days=14))
14 days, 0:00:00
```

Notably, `print` renders each type of value using the appropriate
representation. Numbers and strings appear as-is, but more complex types
like dates or time deltas are displayed in a readable format. To do so,
`print` doesn't rely on inheritance — it's not a method, after all.
Rather, its implementation uses the built-in `str` function, which
returns the string representation of a value.

If we call `str` directly, we obtain the same result without it being
printed to standard output.

```python
>>> str(1)
'1'
>>> str("abc")
'abc'
>>> str(datetime.date.today())
'2025-06-04'
>>> str(datetime.timedelta(days=14))
'14 days, 0:00:00'
```

How does the `str` function know how to convert a value into a string?
When `str` is invoked, it calls a **special method** named `__str__` on
its argument. Whatever string is returned by the `__str__` method
dictates the string representation of the value.

Again, we can call the `__str__` method directly to obtain the same
result.

```python
>>> (1).__str__()
'1'
>>> "abc".__str__()
'abc'
>>> datetime.date.today().__str__()
'2025-06-04'
>>> datetime.timedelta(days=14).__str__()
'14 days, 0:00:00'
```

The special method `__str__` is supposed to return a human-interpretable
representation of the value. If it is not defined, `str` falls back on
another special method, `__repr__`, that is supposed to return an
expression that can be used to recreate an equivalent value. If
`__repr__` is not defined either, the representation is a string
enclosed in angle brackets that contains the name of the type of the
object together with additional information, such as its address in
memory.

```python
>>> class Person:
...     def __init__(self, name: str, age: int):
...         self.name = name
...         self.age = age
...
>>> p = Person("Jin", 23)
>>> str(p)
'<__main__.Person object at 0x100d006e0>'
```

The `Person` class defined above, for example, does not declare a
`__str__` method nor a `__repr__` method. Hence, `str` returns the
default representation when invoked on one of its instances.

By implementing the `__str__` method, we can make it so `str` (and by
extension, `print`) produces the appropriate representation, whatever
that is.

```python
>>> class Person:
...     def __init__(self, name: str, age: int):
...         self.name = name
...         self.age = age
...     def __str__(self):
...         return self.name
...
>>> p = Person("Jin", 23)
>>> str(p)
'Jin'
```

In addition to `__str__` and `__repr__`, classes can implement special
methods to define how _operators_ apply to their instances. For example,
to evaluate expressions that contain the `+` operator, Python checks for
an `__add__` method on the value of the left operand, then checks for an
`__radd__` method on the value of the right operand. If either is found,
that method is invoked with the value of the other operand as its
argument.

Let's implement the `__add__` method on `Person` so that "adding" two
people produces their "newborn child":

```python
>>> from typing import Self
>>> class Person:
...     def __init__(self, name: str, age: int):
...         self.name = name
...         self.age = age
...     def __str__(self):
...         return self.name
...     def __add__(self, other: Self) -> Self:
...         return Person(f"{self.name}-{other.name}", 0)
...
>>> jin = Person("Jin", 23)
>>> wei = Person("Wei", 25)
>>> child = jin + wei
>>> print(child)
JinWei
```

The Python documentation describes the exhaustive set of
[method names for operators][doc].

[doc]: https://docs.python.org/3/reference/datamodel.html#special-method-names
