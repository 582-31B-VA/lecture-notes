# Modules

As your program gets longer, you may want to split it into several
**modules** for easier maintenance. A module is a file that contains
Python definitions and statements. Each module has its own private
namespace, which is used as the global namespace by all functions
defined in the module.

Modules can import other modules. Suppose you have a Python file named
`fibo.py` in a project directory:

```python
# fibo.py

def fib(n):
    result = []
    a, b = 0, 1
    while a < n:
        result.append(a)
        a, b = b, a + b
    return result
```

You can import the `fibo` module into another module from the same
directory using the `import` keyword. It is customary to place all
import statements at the beginning of a module. The imported module
name, which corresponds to its filename without the `.py` extension, is
added to the current module's global namespace:

```python
# app.py

import fibo

fibo.fib()
```

It is possible to import names from a module directly into the importing
module's namespace using the `from` keyword. It can, however, make the
code more difficult to understand.

```python
# app.py

from fibo import fib

fib()
```

## Packages

**Packages** are a way to organize related modules into a directory
hierarchy. A package is essentially a directory that contains a special
`__init__.py` file and one or more modules.

The structure of a Python package typically looks like this:

```
project_name/
    package_name/
        __init__.py       # Marks the directory as a package (can be empty or include initialization code)
        module1.py        # First module in the package
        module2.py        # Second module in the package
    main.py               # Main program
```

In the simplest case, `__init__.py` can just be an empty file, but it
can also execute initialization code for the package. When the package
is imported, this `__init__.py` file is implicitly executed, and the
objects it defines are bound to names in the package's namespace.

To use a package or its modules, the syntax is:

```python
import package_name.module_name

# OR
from package_name import module_name

# OR (import specific functions or classes)
from package_name.module_name import function_name, class_name
```
