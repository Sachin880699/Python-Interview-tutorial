# 4+ Experience Interview Questions 


# what is static method ?

A static method in Python is a method defined within a class using the **@staticmethod** decorator. It does not depend on the class or instance for its operation,  meaning it cannot access or modify instance attributes (self) or class attributes (cls). Static methods are used for general-purpose functionality related to the class but do not require any class or instance-specific data. <br>
**Static Method:** Belongs to the class, not tied to any specific object of the class. It is like a standalone function grouped inside the class for organizational purposes. <br>
**Static Method:** Used for tasks that don’t depend on the instance or class but are logically related to the class.


    class MathHelper:
        @staticmethod
        def add(a, b):
            return a + b
    
        @staticmethod
        def is_even(number):
            return number % 2 == 0
    
    # Call static methods directly
    print(MathHelper.add(2, 3))        # Output: 5
    print(MathHelper.is_even(4))      # Output: True


# Can a static method call another static method in the same class?

Yes, a static method can call another static method within the same class.

Since both methods are static, they belong to the class itself rather than to any specific instance. This means they can directly access each other within the class using the class name (or even using self in the case of an instance method, but that's not applicable here)

    class Calculator:
        @staticmethod
        def add(x, y):
            """Adds two numbers."""
            return x + y
    
        @staticmethod
        def multiply(x, y):
            """Multiplies two numbers."""
            return x * y
    
        @staticmethod
        def calculate_area_of_rectangle(length, width):
            """Calculates the area of a rectangle."""
            # Calling the add method inside another static method
            area = Calculator.multiply(length, width)
            return area
    
    # Example usage

    length = 5
    width = 3
    area = Calculator.calculate_area_of_rectangle(length, width)
    print(f"The area of the rectangle is {area}")



