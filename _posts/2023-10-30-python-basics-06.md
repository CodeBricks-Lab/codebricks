---
layout: post
title: "Basics of Python 06: File Handling"
date: 2023-10-30 08:00:00 +0600
tags: python-tutorial beginner-guide python-basics
categories: [python]
author: "Code Bricks"
post_image: "/assets/images/blog/python-basic-01.jpg"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

# File Handling in Python

File handling in Python enables developers to read from and write to files, allowing for persistent storage of data. This is crucial for programs that need to save user data, configuration settings, or any other information that should be retained between program runs.

## 1.1 Reading from a File

Reading from a file is one of the most common file operations. Here’s how you can do it in Python.

### Example:

```python
with open('example.txt', 'r') as file:
    content = file.read()
    print(content)
```

In this example, we use the `open` function to open the file `example.txt` in read mode (`'r'`). The `with` statement is used to ensure that the file is properly closed after it’s been read, even if an error occurs during the process. The `read` method reads the entire content of the file into a string.

### Reading Line by Line:

```python
with open('example.txt', 'r') as file:
    for line in file:
        print(line.rstrip())
```

This code prints each line in the file, one at a time.

## 1.2 Writing to a File

Writing to a file is just as simple as reading from one.

### Example:

```python
with open('example.txt', 'w') as file:
    file.write("Hello, World!")
```

Here, `'w'` opens the file in write mode. If `example.txt` already exists, Python will erase its contents before returning the file object. If the file does not exist, Python will create it.

### Appending to a File:

If you want to add content to a file instead of writing over existing content, you can open the file in append mode (`'a'`).

```python
with open('example.txt', 'a') as file:
    file.write("\nAppending a new line.")
```

This code adds “Appending a new line.” to `example.txt` without erasing its current content.

## 1.3 File Modes and Operations

Here are some of the different modes you can use to open a file:

- `'r'`: Read mode (default)
- `'w'`: Write mode
- `'a'`: Append mode
- `'b'`: Binary mode
- `'x'`: Create and open a new file, or error if it exists

### Closing a File:

While the `with` statement takes care of closing the file, you can close it manually using the `close` method.

```python
file = open('example.txt', 'r')
content = file.read()
print(content)
file.close()
```

## Conclusion and Next Steps

File handling is a powerful feature in Python, enabling programs to work with external files on the user’s computer. Practice reading from and writing to files, and you’ll find that you can create more versatile and powerful Python programs.

In the next sections, we will delve into error and exception handling, ensuring that your programs run smoothly even when things go wrong.
