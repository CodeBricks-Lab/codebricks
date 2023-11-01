---
layout: post
title: "Basics of Python 05: Object-Oriented Programming (OOP)"
date: 2023-10-29 08:00:00 +0600
tags: python-tutorial beginner-guide python-basics
categories: [python]
author: "Code Bricks"
post_image: "/assets/images/blog/python-basic-01.jpg"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

# Object-Oriented Programming (OOP) in Python

Object-Oriented Programming is a programming paradigm that uses objects and classes in programming. It aims to implement real-world entities like inheritance, hiding, polymorphism, etc in programming. The main aim of OOP is to bind together the data and the functions that operate on them so that no other part of the code can access this data except that function.

## 1.1 Introduction to OOP

In OOP, we define classes that represent real-world things and situations, and then we create objects based on these classes.

### Example:

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def sit(self):
        print(f"{self.name} is now sitting.")
        
    def roll_over(self):
        print(f"{self.name} rolled over!")
```

In this example, `Dog` is a simple attempt to model a dog. `__init__` is a special method that Python runs automatically whenever we create a new instance based on the `Dog` class. The `self` parameter is required in the method definition, and it must come first before the other parameters. It must be included in the definition because when Python calls this method later (to create an instance of Dog), the method call will automatically pass the self argument.

## 1.2 Classes and Objects

Classes define functions called methods, which identify the behaviors and actions that an object created from the class can perform with its data.

### Example:

```python
my_dog = Dog("Willie", 6)
print(f"My dog's name is {my_dog.name}.")
print(f"My dog is {my_dog.age} years old.")
```

In this example, `my_dog` is an object created from the `Dog` class, which has two attributes `name` and `age`.

## 1.3 Inheritance and Polymorphism

Inheritance is the capability of one class to derive or inherit the properties from another class.

### Example:

```python
class GermanShepherd(Dog):
    def __init__(self, name, age):
        super().__init__(name, age)
        
    def bark(self):
        print("Loud Bark!")
```

In this example, `GermanShepherd` is a child class of `Dog`, and it inherits all of the `Dog` class’s capabilities.

Polymorphism allows the same function name to be used for different types. This can be achieved by function overloading or function overriding.

### Example of Overriding:

```python
class Bulldog(Dog):
    def __init__(self, name, age):
        super().__init__(name, age)
        
    def bark(self):
        print("Soft Bark!")
```

In this example, the `bark` method in `Bulldog` overrides the `bark` method in `Dog` (if it existed).

## Conclusion

Object-Oriented Programming makes the program easy to understand as well as efficient. Practice creating and using classes and objects, and try implementing inheritance and polymorphism to get a better understanding of these concepts.

In the next sections, we will delve into more advanced topics and explore how OOP concepts can be used in real-world applications.
