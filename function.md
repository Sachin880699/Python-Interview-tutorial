# Python Functions – Advanced Interview Questions (36–50)

---

## Question 36: What is a pure function?

A pure function is a function that always returns the same output for the same input and does not modify any external state. It does not depend on global variables, files, or databases. Pure functions are easy to test and debug because they have no side effects. They make code more predictable and reliable. Functional programming encourages the use of pure functions. Example: a function that only calculates and returns a value.

---

## Question 37: What are side effects in functions?

Side effects occur when a function modifies external state such as global variables, files, databases, or prints output. Functions with side effects are harder to test and debug. Examples include writing to a file, updating a database, or changing a global variable. Minimizing side effects improves code maintainability. In production systems, side effects should be controlled and well-documented.

---

## Question 38: Explain pass by value and pass by reference in Python.

Python follows pass by object reference. Immutable objects like integers and strings cannot be changed inside a function. Mutable objects like lists and dictionaries can be modified inside a function. If you reassign a variable, a new reference is created. This behavior can sometimes look like pass by reference. Understanding this helps avoid unexpected bugs in real projects.

---

## Question 39: Can functions be stored in variables?

Yes, functions are first-class objects in Python. They can be assigned to variables, passed as arguments, and returned from other functions. This allows flexible and dynamic programming. It is commonly used in callbacks and higher-order functions. Example: assigning a function to a variable and calling it later. This feature enables functional programming concepts.

---

## Question 40: What is a decorator in Python (basic idea)?

A decorator is a function that modifies another function without changing its source code. It is commonly used to add functionality like logging, authentication, or validation. Decorators wrap the original function and extend its behavior. They are applied using the `@` syntax. Decorators help keep code clean and reusable. Django heavily uses decorators.

---

## Question 41: Why are decorators useful in real projects?

Decorators help separate core logic from cross-cutting concerns like logging and security. They reduce code duplication and improve readability. Common use cases include authentication checks, performance monitoring, and caching. Decorators make code easier to maintain and scale. In Django, decorators are used for permissions and request handling. Interviewers expect basic decorator understanding.

---

## Question 42: What is a callback function?

A callback function is a function passed as an argument to another function. It is executed after a certain task is completed. Callbacks are used in event handling, async programming, and background processing. They improve flexibility by allowing dynamic behavior. Example: passing a function to `map()` or `sorted()`. Callbacks are common in real-world applications.

---

## Question 43: What is function overloading in Python?

Python does not support traditional function overloading like Java or C++. However, it can be achieved using default arguments or `*args` and `**kwargs`. This allows a function to behave differently based on input. It simplifies function design. Example: handling different input lengths inside one function. Interviewers often ask this to check language understanding.

---

## Question 44: What is the difference between return and yield?

`return` sends a value and exits the function immediately. `yield` returns a value and pauses the function state. Functions using `yield` become generators. Generators are memory efficient for large data processing. They generate values one at a time instead of storing all in memory. This is important for performance optimization.

---

## Question 45: When should you use generators instead of functions?

Generators should be used when working with large datasets or streams of data. They reduce memory usage by generating values on demand. Generators improve performance for iterative processing. They are useful in data pipelines and background jobs. Functions return all data at once, which can be inefficient. Generators help build scalable systems.

---

## Question 46: What is function shadowing?

Function shadowing happens when a local variable or function name overrides a built-in function name. Example: naming a variable `list` or `sum`. This can cause unexpected errors and bugs. It is considered bad practice. Avoid using names of built-in functions. Interviewers ask this to test attention to detail.

---

## Question 47: What is idempotent function behavior?

An idempotent function produces the same result even if executed multiple times. Calling it repeatedly does not change the outcome after the first call. This is important in APIs and backend systems. Idempotency prevents duplicate processing. Examples include GET APIs or safe update operations. Interviewers like this concept for backend roles.

---

## Question 48: How do functions help in testing?

Functions allow writing small, isolated units of logic. They make unit testing easier and faster. Functions with clear inputs and outputs are easy to mock and validate. Pure functions are especially test-friendly. Modular functions improve test coverage. Testing becomes simpler and more reliable.

---

## Question 49: What is the importance of docstrings in real projects?

Docstrings help explain what a function does, its parameters, and return values. They improve code readability and maintainability. Tools like Sphinx use docstrings to generate documentation. Good docstrings help new developers understand code faster. They are important in large teams. Interviewers expect basic knowledge of docstrings.

---

## Question 50: How do you decide when to create a function?

A function should be created when logic is reused or becomes complex. It helps break large problems into smaller units. Functions improve readability and reduce duplication. They make code easier to test and debug. Creating functions follows clean code principles. Good function design is a key skill for backend developers.

---


## Question 1: What is a function in Python?

A function in Python is a reusable block of code designed to perform a specific task. Functions allow you to write code once and reuse it multiple times. They make programs more organized, readable, and easier to maintain. You can think of a function as a small machine: you give it input, it processes it, and it gives you output.

---

## Question 2: Why do we use functions?

Functions help improve code readability, reduce repetition, and make programs modular. They allow us to break complex problems into smaller, manageable tasks. Instead of rewriting the same logic multiple times, we can encapsulate it inside a function and reuse it, which saves time and reduces the chance of errors.

---

## Question 3: What is the difference between function and method?

A function is an independent block of code, while a method is a function associated with an object. Functions can exist on their own, such as `len()` or `sum()`, while methods, like `my_list.append()`, operate on objects and can access or modify the object's data.

---

## Question 4: What are built-in functions in Python?

Built-in functions are functions provided by Python that you can use directly without defining them yourself. Examples include `print()`, `len()`, `sum()`, `type()`, and `range()`. They save time and are optimized for performance.

---

## Question 5: What are user-defined functions?

User-defined functions are functions that programmers create to perform specific tasks. They allow us to encapsulate our own logic and reuse it across the program, making code modular and easier to debug. For example, a function to calculate the area of a rectangle can be reused wherever needed.

---

## Question 6: What is function definition and function call?

A function definition is where you write the code of the function, specifying its name, parameters, and the block of code it executes. A function call is when you actually use the function to perform its task. For example:

```python
def greet(name):
    print(f"Hello, {name}!")

greet("Sachin")  # Function call
```

---

## Question 7: What is a function signature?

A function signature is the combination of a function's name and its parameter list. It defines how the function can be called and what arguments it expects, but not what it does internally. Example: `def add(a, b)` has the signature `add(a, b)`.

---

## Question 8: What are parameters and arguments?

Parameters are variables defined in a function to accept input, while arguments are the actual values passed when calling the function. Example:

```python
def add(a, b):  # a and b are parameters
    return a + b

result = add(5, 3)  # 5 and 3 are arguments
```

---

## Question 9: Difference between positional and keyword arguments

Positional arguments are passed in the order of parameters, whereas keyword arguments are passed using parameter names. Example:

```python
def greet(name, age):
    print(f"{name} is {age} years old")

greet("Sachin", 26)               # Positional
greet(age=26, name="Sachin")      # Keyword
```

---

## Question 10: What are default arguments?

Default arguments provide a default value if the caller doesn’t supply one. They make functions flexible. Example:

```python
def greet(name="Guest"):
    print(f"Hello, {name}!")

greet()            # Hello, Guest!
greet("Sachin")    # Hello, Sachin!
```

---

## Question 11: What are variable-length arguments?

Variable-length arguments allow a function to accept an arbitrary number of arguments. Python provides `*args` for non-keyword arguments and `**kwargs` for keyword arguments. This makes functions flexible to handle multiple inputs without specifying a fixed number.

---

## Question 12: Explain *args in Python

`*args` allows a function to accept any number of positional arguments as a tuple. Example:

```python
def add_numbers(*args):
    return sum(args)

print(add_numbers(1, 2, 3, 4))  # Output: 10
```

---

## Question 13: Explain **kwargs in Python

`**kwargs` allows a function to accept any number of keyword arguments as a dictionary. Example:

```python
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Sachin", age=26)
```

---

## Question 14: What is argument unpacking?

Argument unpacking allows you to pass a list, tuple, or dictionary to a function as individual arguments using `*` or `**`. Example:

```python
def greet(name, age):
    print(f"{name} is {age}")

info = ["Sachin", 26]
greet(*info)  # Unpacks list into arguments
```

---

## Question 15: What is return statement?

The `return` statement allows a function to send a result back to the caller. Without `return`, the function outputs `None` by default. Example:

```python
def add(a, b):
    return a + b

result = add(5, 3)
print(result)  # 8
```

---

## Question 16: Can a function return multiple values?

Yes, Python allows functions to return multiple values as a tuple. Example:

```python
def get_person():
    return "Sachin", 26

name, age = get_person()
print(name, age)  # Sachin 26
```

---

## Question 17: What happens if a function does not return anything?

If a function does not have a `return` statement, it returns `None` by default. Example:

```python
def greet(name):
    print(f"Hello, {name}!")

result = greet("Sachin")
print(result)  # None
```

---

## Question 18: What is recursion?

Recursion is a technique where a function calls itself to solve smaller instances of a problem until a base condition is met. It is commonly used in problems like factorials, Fibonacci series, and tree traversals.

---

## Question 19: What is base condition in recursion?

The base condition is a condition that stops the recursive calls, preventing infinite loops. Example:

```python
def factorial(n):
    if n == 0:
        return 1  # Base condition
    return n * factorial(n-1)
```

---

## Question 20: What is the difference between recursion and iteration?

Recursion solves problems by repeatedly calling itself, while iteration uses loops to repeat tasks. Recursion can be more elegant for problems with natural hierarchical structure, but iteration is often more memory-efficient.

---

## Question 21: What is a lambda function?

A lambda function is a small, anonymous function defined using the `lambda` keyword. It can have any number of arguments but only one expression. Example:

```python
square = lambda x: x ** 2
print(square(5))  # 25
```

---

## Question 22: Limitations of lambda functions

Lambda functions can only contain a single expression, cannot include statements, and do not have a name unless assigned to a variable. They are best suited for simple, short operations.

---

## Question 23: What is a higher-order function?

A higher-order function is a function that either takes another function as an argument, returns a function, or both. Examples include `map()`, `filter()`, and `reduce()`.

---

## Question 24: What is map() function?

`map()` applies a given function to all items in an iterable and returns an iterator with the results. Example:

```python
numbers = [1, 2, 3, 4]
squared = list(map(lambda x: x**2, numbers))
print(squared)  # [1, 4, 9, 16]
```

---

## Question 25: What is filter() function?

`filter()` applies a function to an iterable and returns an iterator containing only the items for which the function returns `True`. Example:

```python
numbers = [1, 2, 3, 4, 5]
even = list(filter(lambda x: x%2==0, numbers))
print(even)  # [2, 4]
```

---

## Question 26: What is reduce() function?

`reduce()` is used to apply a function cumulatively to items of an iterable, reducing it to a single value. In Python 3, it is available in `functools`. Example:

```python
from functools import reduce
numbers = [1, 2, 3, 4]
product = reduce(lambda x, y: x*y, numbers)
print(product)  # 24
```

---

## Question 27: What are nested functions?

Nested functions are functions defined inside other functions. They are useful to encapsulate helper logic that should not be accessible outside the outer function. Example:

```python
def outer():
    def inner():
        print("Hello from inner")
    inner()

outer()
```

---

## Question 28: What is a closure?

A closure occurs when a nested function remembers the values from its enclosing scope even after the outer function has finished executing. Example:

```python
def outer(x):
    def inner(y):
        return x + y
    return inner

add5 = outer(5)
print(add5(10))  # 15
```

---

## Question 29: What is scope in Python?

Scope determines the visibility of variables in different parts of a program. Variables defined in a function are not visible outside it (local), whereas variables defined outside are accessible globally.

---

## Question 30: Explain local, global, and nonlocal scope

Local scope refers to variables defined inside a function. Global scope refers to variables defined at the top level of a script or module. Nonlocal scope refers to variables in the nearest enclosing function scope. Example:

```python
x = 10  # global

def outer():
    x = 5  # local to outer
    def inner():
        nonlocal x
        x += 1
        print(x)
    inner()
    print(x)

outer()  # 6 6
print(x) # 10
```

---

## Question 31: What is global keyword?

The `global` keyword allows a function to modify a variable defined in the global scope. Example:

```python
x = 10
def modify():
    global x
    x += 5
modify()
print(x)  # 15
```

---

## Question 32: What is nonlocal keyword?

The `nonlocal` keyword allows a nested function to modify a variable from the nearest enclosing function scope, but not the global scope. Example:

```python
def outer():
    x = 5
    def inner():
        nonlocal x
        x += 1
    inner()
    print(x)
outer()  # 6
```

---

## Question 33: What is function annotation?

Function annotations are a way to attach metadata to a function’s parameters and return value. They do not affect runtime behavior but provide hints. Example:

```python
def add(a: int, b: int) -> int:
    return a + b
print(add.__annotations__)  # {'a': <class 'int'>, 'b': <class 'int'>, 'return': <class 'int'>}
```

---

## Question 34: What is docstring?

A docstring is a string literal that describes what a function, class, or module does. It is enclosed in triple quotes and is placed at the beginning of the function or module. Example:

```python
def greet(name):
    """Prints a greeting message for the given name"""
    print(f"Hello, {name}!")
```

---

## Question 35: How do you access a function’s docstring?

A function's docstring can be accessed using the `.__doc__` attribute. Example:

```python
def greet(name):
    """Prints a greeting message"""
    print(f"Hello, {name}!")

print(greet.__doc__)  # Prints: Prints a greeting message
```
