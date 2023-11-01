---
layout: post
title: "Basics of Python 03: Defining a Function"
date: 2023-10-27 08:00:00 +0600
tags: python-tutorial beginner-guide python-basics
categories: [python]
author: "Code Bricks"
post_image: "/assets/images/blog/python-basic-01.jpg"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

# Defining a Function in Python

Functions are reusable pieces of code that perform a specific task. They help to make our programs more modular, allowing us to use the same code multiple times without rewriting it. In this section, we'll learn how to define and call functions in Python.

## Defining a Function

To define a function in Python, we use the `def` keyword, followed by the function name and parentheses. Here's the general syntax:

```python
def function_name(parameters):
    # code to execute when the function is called
    # optionally return a value
```

### Example:

```python
def greet(name):
    print(f"Hello, {name}!")
```

In this example, we've defined a function named `greet` that takes one parameter `name`. When called, it will print a greeting message including the name provided.

## Calling a Function

Once a function is defined, you can call it by using its name followed by parentheses. If the function expects parameters, you'll need to pass them inside the parentheses.

### Example:

```python
greet("Alice")  # This will print: Hello, Alice!
```

## Returning Values

Functions can also return values using the `return` keyword. The returned value can then be used elsewhere in your program.

### Example:

```python
def add(a, b):
    return a + b

result = add(10, 5)
print(result)  # This will print: 15
```

In this example, the `add` function takes two parameters, adds them together, and returns the result. We call this function, store the result in a variable, and print it.

## Conclusion

Defining and using functions is a fundamental part of programming in Python. They help to make your code more modular, reusable, and understandable. Practice creating and using functions to get a better grasp of how they work.

In the next sections, we will cover more advanced topics related to functions such as function parameters, return values, and lambda functions.
