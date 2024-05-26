---
layout: post
title: "[Python OOP Examples] 1. Bank System"
date: 2024-05-01 08:00:00 +0600
tags: python-tutorial object-oriented-programming examples
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

# Building a Simple Bank System in Python with OOP

Welcome to our tutorial on building a simple bank system using Object-Oriented Programming (OOP) in Python! In this guide, we'll create a system that manages bank accounts for multiple users, performs transactions, and includes a savings account with interest rates. We'll also include unit tests to ensure our system works correctly. Let's dive in!

## Table of Contents

1. Introduction
2. Understanding OOP Concepts
3. Setting Up Your Project
4. Designing the Classes
5. Implementing the Classes
6. Connecting the Classes
7. Adding Advanced Features
8. Testing Your System with Unit Tests
9. Conclusion

## 1. Introduction

We'll be creating a basic bank system using Python's OOP capabilities. This system will manage multiple users and their accounts, allowing for deposits, withdrawals, and interest calculations for savings accounts.

## 2. Understanding OOP Concepts

Before we start coding, let's quickly review the main OOP concepts:

### Classes and Objects

- **Class**: A blueprint for creating objects, defining attributes and methods.
- **Object**: An instance of a class.

### Inheritance

Inheritance allows a class to inherit attributes and methods from another class. For instance, a `SavingsAccount` class can inherit from the `BankAccount` class.

### Encapsulation

Encapsulation bundles data (attributes) and methods that operate on the data into a single unit (class), restricting direct access to some components.

### Polymorphism

Polymorphism allows methods to do different things based on the object they are acting upon. For example, the `deposit` method can behave differently in a `BankAccount` and a `SavingsAccount`.

## 3. Setting Up Your Project

Ensure you have Python installed. Create a new directory for your project, and inside it, create a file named `bank_system.py`.

## 4. Designing the Classes

### BankAccount Class

Represents individual bank accounts, with attributes like account number (automatically generated) and balance, and methods for deposits, withdrawals, and checking balance.

### SavingsAccount Class

Inherits from `BankAccount` and adds an interest rate attribute, with methods to apply interest.

### User Class

Represents a bank customer, with attributes like user ID, name, and a list of associated bank accounts. Methods include adding and removing accounts.

### Bank Class

Manages multiple users and their accounts, providing functionalities to add users, find users, and handle transactions.

## 5. Implementing the Classes

### BankAccount Class

First, we'll implement the `BankAccount` class. We'll use a class variable to generate unique account numbers automatically.

```python
class BankAccount:
    account_number_counter = 1000

    def __init__(self, balance=0):
        self.account_number = BankAccount.account_number_counter
        BankAccount.account_number_counter += 1
        self.balance = balance

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            print(f"Deposited {amount}. New balance: {self.balance}")
        else:
            print("Deposit amount must be positive")

    def withdraw(self, amount):
        if 0 < amount <= self.balance:
            self.balance -= amount
            print(f"Withdrew {amount}. New balance: {self.balance}")
        else:
            print("Insufficient funds or invalid amount")

    def check_balance(self):
        return self.balance
```

### SavingsAccount Class

Next, we'll implement the `SavingsAccount` class that inherits from `BankAccount`:

```python
class SavingsAccount(BankAccount):
    def __init__(self, balance=0, interest_rate=0.01):
        super().__init__(balance)
        self.interest_rate = interest_rate

    def apply_interest(self):
        interest = self.balance * self.interest_rate
        self.balance += interest
        print(f"Applied interest {interest}. New balance: {self.balance}")
```

### User Class

Next, we'll implement the `User` class:

```python
class User:
    def __init__(self, user_id, name):
        self.user_id = user_id
        self.name = name
        self.accounts = []

    def add_account(self, account):
        self.accounts.append(account)
        print(f"Account {account.account_number} added to user {self.name}")

    def remove_account(self, account_number):
        self.accounts = [acc for acc in self.accounts if acc.account_number != account_number]
        print(f"Account {account_number} removed from user {self.name}")

    def get_accounts(self):
        return self.accounts
```

### Bank Class

Now, let's implement the `Bank` class:

```python
class Bank:
    def __init__(self):
        self.users = []

    def add_user(self, user):
        self.users.append(user)
        print(f"User {user.name} added")

    def find_user(self, user_id):
        for user in self.users:
            if user.user_id == user_id:
                return user
        return None

    def transfer(self, from_account, to_account, amount):
        if from_account.balance >= amount:
            from_account.withdraw(amount)
            to_account.deposit(amount)
            print(f"Transferred {amount} from account {from_account.account_number} to account {to_account.account_number}")
        else:
            print("Insufficient funds for transfer")
```

## 6. Connecting the Classes

Let's see how these classes can work together:

```python
# Create bank instance
bank = Bank()

# Create users
user1 = User(1, "Alice")
user2 = User(2, "Bob")

# Add users to the bank
bank.add_user(user1)
bank.add_user(user2)

# Create accounts for users
account1 = BankAccount()
account2 = SavingsAccount(interest_rate=0.05)

# Add accounts to users
user1.add_account(account1)
user2.add_account(account2)

# Perform transactions
account1.deposit(1000)
account1.withdraw(500)
account2.deposit(2000)
account2.apply_interest()

# Transfer money
bank.transfer(account1, account2, 200)
```

## 7. Adding Advanced Features

### Transaction History

Enhance the `BankAccount` class to keep track of transaction history.

### Overdraft Protection

Add logic to handle overdraft protection in the `BankAccount` class.

## 8. Testing Your System with Unit Tests

We will now add unit tests to ensure our system works correctly. Create a new file named `test_bank_system.py` and add the following code:

```python
import unittest
from bank_system import Bank, User, BankAccount, SavingsAccount

class TestBankSystem(unittest.TestCase):

    def test_bank_account(self):
        account = BankAccount()
        self.assertEqual(account.balance, 0)
        account.deposit(100)
        self.assertEqual(account.balance, 100)
        account.withdraw(50)
        self.assertEqual(account.balance, 50)

    def test_savings_account(self):
        account = SavingsAccount(balance=1000, interest_rate=0.05)
        account.apply_interest()
        self.assertEqual(account.balance, 1050)

    def test_user(self):
        user = User(1, "Alice")
        self.assertEqual(user.name, "Alice")
        account = BankAccount()
        user.add_account(account)
        self.assertEqual(len(user.get_accounts()), 1)
        user.remove_account(account.account_number)
        self.assertEqual(len(user.get_accounts()), 0)

    def test_bank(self):
        bank = Bank()
        user1 = User(1, "Alice")
        user2 = User(2, "Bob")
        bank.add_user(user1)
        bank.add_user(user2)
        self.assertEqual(len(bank.users), 2)

        account1 = BankAccount()
        account2 = SavingsAccount(interest_rate=0.05)
        user1.add_account(account1)
        user2.add_account(account2)
        bank.transfer(account1, account2, 200)
        self.assertEqual(account1.balance, 0)
        self.assertEqual(account2.balance, 200)

if __name__ == '__main__':
    unittest.main()
```

## 9. Conclusion

In this tutorial, we built a simple bank system using OOP principles in Python. We covered class creation for managing users and their bank accounts, including a savings account with interest rates. We also added unit tests to ensure our system works as expected. You can extend this system with more features as needed. Happy coding!