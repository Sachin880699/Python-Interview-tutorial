# Python Interview Questions

A comprehensive list of Python interview questions covering core concepts, OOP, data structures, design patterns, testing, performance, and more.

---

## 🐍 Python Fundamentals

- What are Python's key features?
- What is the difference between Python 2 and Python 3?
- How does Python handle memory management?
- What are Python’s built-in data structures?
- What are Python's built-in types?
- What is the use of `zip()` function?
- How does Python’s `map()` function work?
- What does the `filter()` function do?
- What are lambda functions?
- What is slicing in Python?
- What is the difference between `str()` and `repr()`?
- How does Python manage namespaces?
- What is the use of `enumerate()`?
- What is the use of `any()` and `all()`?
- What is the difference between `int()` and `float()` constructors?

---

## 🧠 Memory, Copying, and Optimization

- Explain the difference between deep copy and shallow copy.
- What is the difference between `deepcopy` and `copy` in the `copy` module?
- How is garbage collection implemented in Python?
- What is the `gc` module used for?
- What are weak references in Python?
- How do you avoid memory leaks in Python?
- What are some common memory optimization techniques in Python?

---

## 🔐 Data Types and Structures

- What is the difference between mutable and immutable types?
- What are some immutable data types in Python?
- What is the difference between `tuple` and `list`?
- What are `namedtuples`?
- What is the difference between `frozenset` and `set`?
- What are frozen sets in Python?
- How to remove duplicates from a list?

---

## 🎯 Functions & Functional Programming

- What are Python decorators and how are they used?
- What is function currying?
- What are closures in Python?
- How are function annotations used in Python?
- What is the purpose of the `nonlocal` keyword?
- What is the difference between `*args` and `**kwargs`?
- What are comprehensions and how do they improve performance?
- What is lazy evaluation in Python?
- What is the use of the `functools` module?

---

## 🔄 Iteration, Generators & Comprehensions

- Explain list comprehension with an example.
- What are generators in Python?
- Explain the use of `yield` in Python.
- What are Python iterators and iterables?
- What is a memory view object?
- What is the use of `getitem`, `setitem`, and `delitem`?
- How to make a class iterable?

---

## 🧱 Object-Oriented Programming (OOP)

- What are the four principles of Object-Oriented Programming?
- What is method overriding vs method overloading in Python?
- Can you instantiate an abstract class in Python?
- How does multiple inheritance work in Python?
- What is the Diamond Problem and how does Python resolve it?
- How does Python resolve attribute lookups (MRO - Method Resolution Order)?
- What is the difference between `@staticmethod` and `@classmethod`?
- What are instance methods vs class methods vs static methods?
- What is operator overloading and how is it achieved in Python?
- What are the dunder methods for comparison operations?
- What is the purpose of `contains` method?
- What is the difference between `eq` and `hash`?
- What is the role of `object` as a base class?
- What is the `class` attribute used for?
- How do you define equality (`==`) for your own class?
- What is a bound method vs an unbound method?
- What are mixins in Python?
- How do Python objects manage state and behavior?
- What is the role of `__del__` method?
- What are callable objects and how do they relate to OOP?

---

## 🧬 Advanced OOP & Design Patterns

- What are metaclasses in Python?
- How to use `super()` with multiple inheritance?
- What is encapsulation and how do you implement it?
- Can you explain the difference between composition and inheritance?
- What’s the role of composition in Python over inheritance?
- What are inner classes and when should you use them?
- What are virtual subclasses in Python?
- How do you implement a factory pattern in Python?
- What is a factory method vs abstract factory?
- How do you enforce interface-like behavior in Python?
- What is an interface segregation principle and how to use it in Python?
- What is a proxy pattern and how can you implement it?
- How do you implement a singleton pattern in Python?
- What are design patterns commonly used in Python OOP?
- What is the SOLID principle and how do you apply it in Python?
- What is a fluent interface pattern in Python?
- How is polymorphism achieved in Python?
- How is polymorphism achieved without inheritance?
- How does inheritance affect performance in Python?

---

## 🔐 Attributes, Properties & Descriptors

- What are Python descriptors?
- What is the use of `@property`?
- What is the difference between `@property` and regular methods?
- What are the uses of `__dict__` in Python classes?
- What are the use cases of `getattr`, `setattr`, and `delattr`?
- How can you restrict dynamic attribute creation in classes?
- How do you create a private attribute in Python?
- How do you create immutable classes in Python?

---

## ⚙️ Modules, Packages & Imports

- What is the difference between a module and a package?
- What is the purpose of `__init__.py` in a Python package?
- How do you handle circular imports in Python?
- What are Python wheels and eggs?
- What is pip and how is it used?
- What are some built-in modules you use frequently?
- How can you dynamically load a module?
- What is the use of `__name__ == "__main__"`?

---

## 🧪 Testing & Debugging

- How do you unit test in Python?
- How to write parameterized tests in Python?
- What is the difference between `assert` and exception?
- What are Python’s built-in exceptions?
- How do you catch multiple exceptions in one line?
- How to mock external APIs during testing?
- How to test private methods in Python?
- What are fixtures in `pytest`?
- What tools can you use for debugging Python code?

---

## ⚡ Performance & Concurrency

- What is the GIL (Global Interpreter Lock) in Python?
- How does Python implement multi-threading?
- What is the difference between a daemon and non-daemon thread?
- What are semaphores and locks in Python?
- How do you use multiprocessing in Python?
- How can you safely terminate threads in Python?
- How to create a thread-safe counter?
- What is profiling in Python?
- What is the `timeit` module used for?
- How do you benchmark code performance?
- What is the difference between blocking and non-blocking code?
- How to monitor memory usage of a Python app?

---

## 🔁 Async, Event-Driven & Coroutine Systems

- What is the `asyncio` library?
- How does `async`/`await` work in Python?
- What are coroutines and how are they different from generators?
- What are context switches in coroutines?
- How to implement event-driven programming in Python?

---

## 💾 Data Handling & Serialization

- How to read and write JSON in Python?
- What is the `pickle` module and when should it be avoided?
- How do you serialize and deserialize data in Python?
- How do you handle missing values in data processing?

---

## 🔐 Security & Best Practices

- What are some best practices for writing Pythonic code?
- What is the Zen of Python?
- How can you prevent SQL injection in Python applications?
- How do you manage secrets or credentials in Python apps?
- How do you validate function inputs in Python?

---

## 📊 CLI Tools & Web Frameworks

- How do you build CLI tools using Python?
- What is the difference between `argparse`, `click`, and `docopt`?
- What are context processors in Python web frameworks?

---

## 🧠 Miscellaneous

- What is monkey patching in Python?
- What is the purpose of the `with` statement?
- What is the purpose of `globals()` and `locals()`?
- What is the `inspect` module used for?
- What is the `warnings` module?
- What are “dunder” methods?
- What is the `eval()` function? Why is it dangerous?
- What is the difference between `eval()` and `exec()`?
- What is the difference between `is` and `==`?
- What is the purpose of `getattr`, `getattribute`, and `setattr`?
- How do you implement the observer pattern in Python?
- How to implement custom exceptions in Python?
- How do you make an object JSON serializable?
- What are docstrings and how are they different from comments?
- What are sentinel objects and how are they used?
- How do you enforce type checking in Python?
- What is the `typing` module used for?
- What is reflection and how is it used in Python?
- What is introspection in Python?
- What are metaprogramming techniques in Python?

---

## 🧠 Interview Strategy

If you're preparing for Python interviews, focus first on:
- Core syntax and behavior
- OOP design and class behavior
- Testing and debugging
- Performance and optimization
- Real-world applications (file I/O, serialization, networking)

---

