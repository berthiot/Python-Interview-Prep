# Python-Interview-Prep

1. **What are the uses of the 'nonlocal' and 'global' keywords?**
   - `nonlocal`: Allows you to modify a variable in a parent function's scope from a nested function.
   - `global`: Declares a variable as global, allowing it to be accessed and modified from any scope.

   ```python
   # Example
   x = 5
   def func1():
       global x
       x = 10
   func1()
   print(x)  # Output: 10
   ```

2. **What is the difference between class methods and static methods?**
   - Both can be called without an instance of the class, but:
     - `staticmethod` does not have access to class or instance variables.
     - `classmethod` has access to the class itself via `cls` argument.

   ```python
   # Example
   class A:
       @staticmethod
       def func1():
           print("Static method")

       @classmethod
       def func2(cls):
           print("Class method")
   ```

3. **What is the GIL (Global Interpreter Lock) and what are some ways to bypass it?**
   - The GIL is a mutex that protects access to Python objects, preventing multiple native threads from executing Python bytecodes at once.
   - Ways to bypass it include using the `multiprocessing` module, which creates separate processes, and using asynchronous programming with `asyncio`.

4. **What are metaclasses and when would you use them?**
   - Metaclasses are the classes of classes. They are used to customize the behavior of class creation and can be useful for implementing frameworks and libraries.
   - A common use case is creating abstract base classes using `ABCMeta` from the `abc` module.

   ```python
   # Example
   from abc import ABC, abstractmethod

   class AbstractCar(ABC):
       @abstractmethod
       def drive(self):
           pass
   ```

5. **What are generator functions? Write your own version of the range function.**
   - Generator functions return an iterator that generates values on-the-fly using the `yield` keyword.
   - Here's an example implementation of a generator function similar to `range`:

   ```python
   # Example
   def my_range(start, end, step=1):
       while start < end:
           yield start
           start += step
   ```

6. **What are decorators in Python?**
   - Decorators are functions that modify the behavior of other functions or methods.
   - They are denoted by the `@decorator_name` syntax and are placed before the function definition.

   ```python
   # Example
   def my_decorator(func):
       def wrapper():
           print("Something is happening before the function is called.")
           func()
           print("Something is happening after the function is called.")
       return wrapper

   @my_decorator
   def say_hello():
       print("Hello!")

   say_hello()
   ```

7. **What is Pickling and Unpickling in Python?**
   - Pickling is the process of serializing objects into a byte stream.
   - Unpickling is the process of deserializing a byte stream back into Python objects.
   - It is useful for saving and loading data, especially in file handling and network communication.

   ```python
   # Example
   import pickle

   cars = {"Subaru": "best car", "Toyota": "no i am the best car"}
   with open("cars.pkl", "wb") as f:
       pickle.dump(cars, f)

   with open("cars.pkl", "rb") as f:
       new_cars = pickle.load(f)
   ```

8. **What are *args and **kwargs in Python functions?**
   - `*args` and `**kwargs` allow a function to accept a variable number of arguments.
   - `*args` collects positional arguments into a tuple, while `**kwargs` collects keyword arguments into a dictionary.

   ```python
   # Example
   def func1(*args, **kwargs):
       for arg in args:
           print(arg)
       for key, value in kwargs.items():
           print(f"{key}: {value}")

   func1(1, 2, 3, name="Alice", age=30)
   ```

9. **What are .pyc files used for?**
   - `.pyc` files are compiled Python files that contain bytecode, which is the low-level representation of your Python code.
   - They are used to improve the performance of Python programs by reducing the need for re-parsing and re-compiling the source code.

10. **How do you define an abstract class in Python?**
    - Abstract classes are classes that cannot be instantiated and are meant to be subclassed.
    - They are defined using the `ABC` class from the `abc` module and the `@abstractmethod` decorator.

    ```python
    # Example
    from abc import ABC, abstractmethod

    class AbstractCar(ABC):
        @abstractmethod
        def drive(self):
            pass

    class RedFlagCar(AbstractCar):
        def drive(self):
            print('go go go')

    if __name__ == '__main__':
        a = RedFlagCar()
        a.drive()
    ```
