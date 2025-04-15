# Python Interview Questions

A comprehensive list of Python interview questions covering core concepts, OOP, data structures, design patterns, testing, performance, and more.

---


# what is Closures

A closure in Python is created when a nested function captures variables from its enclosing scope, and those variables are remembered even after the outer function is done.
It’s useful when we want to create functions with some internal state or configuration without using global variables


    def multiplier(n):
        def multiply(x):
            return x * n
        return multiply


# What is GIL

The GIL (Global Interpreter Lock) is a rule in Python that says:
Only one thread can run Python code at a time, even if you have multiple CPU cores.

<br>

The GIL is a lock in Python that allows only one thread to run Python code at a time. It’s good for safety, but it limits performance for CPU-heavy tasks. That’s why for true parallelism, we use multiprocessing instead of threading.


# what is Lazy Evaluation

Lazy evaluation, also known as call-by-need, is an evaluation strategy used in some programming languages where the evaluation of an expression is delayed until its value is actually needed. In simpler terms, the computation is postponed until the result is required for some other operation


# is V/S == 

**is** : Checks if both operands (variables) refer to the exact same object in memory. It's comparing their identity, not just their values. <br>
**==** : Compares if the values of both objects are the same. It doesn't care if they are the exact same object in memory.








 
