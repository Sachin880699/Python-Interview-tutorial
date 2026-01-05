# Python Object-Oriented Programming (OOP) – Interview Q&A

## Question 1: What is Object-Oriented Programming?

Object-Oriented Programming (OOP) is a programming paradigm based on the concept of objects. Objects are instances of classes that encapsulate both data (attributes) and behavior (methods). OOP helps in organizing code in a modular way, making it easier to manage, reuse, and scale large programs.

---

## Question 2: What are the main principles of OOP?

The main principles of OOP are encapsulation, abstraction, inheritance, and polymorphism. Encapsulation hides the internal state and exposes only necessary functionality. Abstraction hides complexity and shows only essential details. Inheritance allows classes to reuse code from other classes. Polymorphism allows objects to be treated as instances of their parent class, enabling flexible code.

---

## Question 3: What is a class?

A class is a blueprint or template for creating objects. It defines the attributes (data) and methods (functions) that the objects will have. Think of a class as a plan, and objects as the real-world instances built from that plan.

---

## Question 4: What is an object?

An object is a specific instance of a class. It contains actual values for the attributes defined in the class and can use the methods defined in the class to perform actions.

---

## Question 5: Difference between class and object

A class is a blueprint that defines properties and behavior, while an object is a concrete instance of that class with actual values. For example, `Car` can be a class, and `my_car = Car()` is an object.

---

## Question 6: What is `__init__()` method?

The `__init__()` method is a special method in Python called a constructor. It initializes the attributes of an object when the object is created. Example:

```python
class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

my_car = Car("Audi", "A4")
```

---

## Question 7: What is self keyword?

`self` represents the instance of the class and allows access to its attributes and methods. It must be the first parameter in instance methods.

---

## Question 8: Can we create multiple objects of a class?

Yes, we can create multiple objects of a class. Each object will have its own copy of instance variables, but they share the class methods.

---

## Question 9: What are instance variables?

Instance variables are attributes that belong to a specific object. They are defined inside methods using `self` and can have different values for different objects.

---

## Question 10: What are class variables?

Class variables are attributes that are shared among all objects of a class. They are defined within the class but outside any instance method.

---

## Question 11: Difference between instance variable and class variable

Instance variables are unique to each object, while class variables are shared across all objects of the class. Changing a class variable affects all instances, but changing an instance variable affects only that object.

---

## Question 12: What is encapsulation?

Encapsulation is the concept of hiding an object’s internal state and providing methods to access or modify it. This prevents external code from directly modifying internal data and ensures controlled access.

---

## Question 13: How is encapsulation achieved in Python?

Encapsulation in Python is achieved by prefixing attribute names with a single underscore `_` for protected or double underscore `__` for private attributes. This signals that they should not be accessed directly outside the class.

---

## Question 14: What is abstraction?

Abstraction is the process of hiding complex implementation details and showing only essential features to the user. It simplifies interaction with objects and reduces complexity.

---

## Question 15: How do you implement abstraction in Python?

Abstraction can be implemented using abstract classes and methods from the `abc` module. Example:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
```

---

## Question 16: What is inheritance?

Inheritance allows a class to acquire attributes and methods from another class. The child class can reuse, extend, or override the behavior of the parent class.

---

## Question 17: Types of inheritance in Python

Python supports single inheritance, multiple inheritance, multilevel inheritance, hierarchical inheritance, and hybrid inheritance.

---

## Question 18: What is multiple inheritance?

Multiple inheritance occurs when a class inherits from more than one parent class. Python handles this with Method Resolution Order (MRO) to determine which parent method to call.

---

## Question 19: What is method overriding?

Method overriding is when a child class provides a new implementation of a method already defined in its parent class. This allows customizing or extending the behavior of inherited methods.

---

## Question 20: What is method overloading in Python?

Python does not support traditional method overloading by argument type or number. However, default arguments or `*args` and `**kwargs` can simulate overloading behavior.

---

## Question 21: What is polymorphism?

Polymorphism allows objects of different classes to be treated as objects of a common parent class. It enables a single interface to handle different underlying data types or classes.

---

## Question 22: What is dynamic binding?

Dynamic binding refers to the process where the method to be executed is determined at runtime, not at compile time. It allows Python to call the correct method in inheritance or polymorphism scenarios.

---

## Question 23: What is super() function?

The `super()` function allows a child class to call methods or access attributes of its parent class. It is commonly used in constructors or overridden methods.

---

## Question 24: What is Method Resolution Order (MRO)?

MRO determines the order in which base classes are searched when executing a method. You can check it using `ClassName.__mro__` or `ClassName.mro()`.

---

## Question 25: What is isinstance()?

`isinstance(obj, Class)` checks if `obj` is an instance of `Class` or its subclass. Example:

```python
print(isinstance(my_car, Car))  # True
```

---

## Question 26: What is issubclass()?

`issubclass(Child, Parent)` checks if the `Child` class is derived from the `Parent` class.

---

## Question 27: What are magic methods?

Magic methods, or dunder methods (like `__init__`, `__str__`), are special methods Python calls automatically to perform built-in operations such as object creation, printing, or arithmetic.

---

## Question 28: What is `__str__()` and `__repr__()`?

`__str__()` returns a user-friendly string representation of an object, while `__repr__()` returns an unambiguous representation meant for developers. Example:

```python
class Car:
    def __init__(self, brand):
        self.brand = brand
    def __str__(self):
        return f"Car: {self.brand}"
    def __repr__(self):
        return f"Car('{self.brand}')"
```

---

## Question 29: What is data hiding in Python?

Data hiding restricts direct access to attributes of a class to protect its integrity. Python uses naming conventions like single or double underscores to signal that an attribute is intended to be private.

---

## Question 30: What is private, protected, and public access?

Public attributes are accessible from anywhere, protected attributes (prefix `_`) should be accessed with caution, and private attributes (prefix `__`) are meant to be hidden from external access.

---

## Question 31: Can Python enforce data hiding strictly?

Python does not strictly enforce data hiding. It relies on naming conventions and name mangling to discourage access but does not make it impossible.

---

## Question 32: What is composition vs inheritance?

Inheritance represents an "is-a" relationship, while composition represents a "has-a" relationship. Composition involves using objects of other classes as attributes, whereas inheritance derives new classes from existing ones.

---

## Question 33: When should you use composition?

Use composition when a class needs to use functionality from another class without being a subtype. It promotes flexibility and reduces tight coupling compared to inheritance.

---

## Question 34: What is an abstract class?

An abstract class is a class that cannot be instantiated and may contain abstract methods that must be implemented by subclasses. It provides a common interface for derived classes.

---

## Question 35: What is an interface in Python?

Python does not have explicit interfaces like Java, but abstract classes with only abstract methods can serve as interfaces. They define methods that subclasses must implement, ensuring consistent behavior.
