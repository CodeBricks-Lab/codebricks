---
layout: post
title: "Basics of Python 02"
date: 2023-10-26 08:00:00 +0600
tags: python-tutorial beginner-guide python-basics
categories: [python]
author: "Code Bricks"
post_image: "/assets/images/blog/python-basic-01.jpg"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

# Control Structures in Python

In this section of the tutorial, we will delve into the core control structures of Python programming: Conditional Statements and Loops. Understanding these concepts is crucial as they control the flow of execution in a program.

## 1.1 Conditional Statements (if, elif, else)

Conditional statements are used to execute code only if a certain condition is met. Here’s how to use them:

```python
if condition:
    # code to execute if condition is True
elif another_condition:
    # code to execute if another_condition is True
else:
    # code to execute if none of the above conditions are True
```

### Example:
```python
age = 20
if age < 18:
    print("You are a minor.")
elif age == 18:
    print("Congratulations! You are 18.")
else:
    print("You are an adult.")
```

## 1.2 Loops (for, while)

Loops are used to execute a block of code repeatedly.

### 1.2.1 The for Loop
The `for` loop is used to iterate over a sequence (list, tuple, string, or range).

```python
for variable in sequence:
    # code to execute for each item in sequence
```

### Example:
```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

### 1.2.2 The while Loop
The `while` loop repeats as long as a certain boolean condition is met.

```python
while condition:
    # code to execute while condition is True
```

### Example:
```python
count = 0
while count < 5:
    print(count)
    count += 1
```

## 1.3 Loop Control Statements (break, continue, pass)

### 1.3.1 break
The `break` statement is used to exit the loop prematurely when a certain condition is met.

### Example:
```python
for i in range(10):
    if i == 5:
        break
    print(i)
```

### 1.3.2 continue
The `continue` statement skips the rest of the code inside the loop for the current iteration.

### Example:
```python
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)
```

### 1.3.3 pass
The `pass` statement is a null operation; nothing happens when it executes. It is used as a placeholder.

### Example:
```python
for i in range(10):
    if i % 2 == 0:
        pass
    else:
        print(i)
```

With these control structures, you can start building more complex and interesting programs in Python. Practice using these concepts to get comfortable with them. Happy coding!
