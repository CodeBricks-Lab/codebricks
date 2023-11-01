---
layout: post
title: "Basics of Python 04: Data Structures"
date: 2023-10-28 08:00:00 +0600
tags: python-tutorial beginner-guide python-basics
categories: [python]
author: "Code Bricks"
post_image: "/assets/images/blog/python-basic-01.jpg"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

# Data Structures in Python

Data structures are a way of organizing and storing data so that they can be accessed and worked with efficiently. Python has several built-in data structures such as lists, tuples, sets, and dictionaries that you can use to build more complex data structures. In this section, we will explore these fundamental data structures.

## 1.1 Lists

Lists in Python are ordered collections that can hold a variety of object types. They are defined with square brackets `[]`.

### Example:

```python
fruits = ["apple", "banana", "cherry"]
print(fruits)
```

### Common List Operations:

- `append()`: Adds an item to the end of the list.
- `insert()`: Adds an item at a specific position.
- `remove()`: Removes the first item with the specified value.
- `pop()`: Removes the item at a specific position.
- `sort()`: Sorts the list.

## 1.2 Tuples

Tuples are similar to lists but they are immutable, which means once a tuple is created, its contents cannot be changed.

### Example:

```python
coordinates = (4, 5)
print(coordinates)
```

Tuples are often used to store related pieces of information.

## 1.3 Sets

Sets are unordered collections of unique elements. They are defined with curly brackets `{}`.

### Example:

```python
fruits = {"apple", "banana", "cherry"}
print(fruits)
```

### Common Set Operations:

- `add()`: Adds an item to the set.
- `remove()`: Removes a specified item from the set.
- `union()`: Returns a set that is the union of two sets.
- `intersection()`: Returns a set that is the intersection of two sets.

## 1.4 Dictionaries

Dictionaries are collections of key-value pairs. They are defined with curly brackets `{}` and each key is separated from its value by a colon `:`.

### Example:

```python
person = {"name": "John", "age": 30, "city": "New York"}
print(person)
```

### Common Dictionary Operations:

- `keys()`: Returns a list of all the keys.
- `values()`: Returns a list of all the values.
- `items()`: Returns a list of tuples, each containing a key and value pair.
- `get()`: Returns the value of a specified key.

## Conclusion

Understanding these basic data structures is crucial for programming in Python. They are the building blocks for creating more complex data structures and algorithms. Practice working with lists, tuples, sets, and dictionaries to get a better grasp of how to use them efficiently.

In the next sections, we will explore more advanced topics and see how these data structures can be used to solve real-world problems.
