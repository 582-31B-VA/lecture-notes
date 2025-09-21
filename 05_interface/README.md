# Interface

The object system is designed to promote a **separation of concerns**
among the different aspects of a program. Each object encapsulates and
manages some part of the program's state, and each class defines methods
that implement some part of the program's overall logic. Importantly,
these different types of objects can interact _without having to know
how the others works_. That is why, for example, we can use the `date`
class while not having read a single line of its implementation.

To make this works, a great deal of thought must go into how other
programmers will use our classes — whether it's colleagues, or even
ourselves, a few months from now. When designing a class, we must
distinguish its **private implementation** (that is, the code that
manages its state and behavior) from its **public interface**, which
dictates how "client code" will use it. In a well-designed class, we
should be able to modify the implementation (e.g., to make it more
efficient or fix a bug) without affecting the interface. Otherwise, even
a small change in one part of our program would ripple to the other
parts.

Many approaches can be used to enforce this separation between
implementation and interface. For instance, we can use docstrings to
indicate which methods and attributes are safe for clients to use, while
marking others as reserved for internal use within the class. Most
modern languages also provide keywords to declare methods and attributes
as "private" or "protected", making them inaccessible from outside the
class.

Unfortunately, private variables that cannot be accessed except from
inside an object don't exist in Python. The language encourages another
point of view: treat all variables as public _unless_ you have a good
reason not to, in which case prefix its name with an underscore (e.g.,
`_spam`). Technically, an attribute or method with a leading underscore
can still be used outside the class, but most program will consider it
as private.

What's a good reason to make an attribute private? Often, it's because
there are restrictions on its value (e.g., it cannot contain whitespace,
it cannot be greater than 100, etc.). If client code were to assign a
value to the attribute directly, it might violate these restrictions and
inadvertently break the implementation. Whereas calling a method instead
would allow us to validate the new value before assigning it to the
attribute.

Another good reason to make an attribute private is to hide its internal
representation. For example, we might store a bank account's balance in
cents and convert it to the appropriate currency only when a method is
called. This would shield client code from implementation details and
allow us to change how the balance is stored without impacting the rest
of the program.
