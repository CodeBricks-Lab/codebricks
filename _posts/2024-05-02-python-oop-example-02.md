---
layout: post
title: "[Python OOP Examples] 2. Bank System with SQLite "
date: 2024-05-02 08:00:00 +0600
tags: python-tutorial object-oriented-programming examples database
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

# Extending Your Bank System with SQLite

Welcome to the next part of our tutorial series on building a bank system using Python! In this post, we'll learn how to use SQLite to save user and account information, making our bank system more robust and persistent. Let's get started!

## Table of Contents

1. Introduction
2. Understanding SQLite
3. Setting Up SQLite in Python
4. Designing the Database Schema
5. Implementing SQLite in Your Bank System
6. Testing Your SQLite Integration
7. Conclusion

## 1. Introduction

Previously, we built a basic bank system using Python's OOP principles. Now, we'll extend this system to store data in a SQLite database. This will allow us to save and retrieve user and account information persistently.

## 2. Understanding SQLite

SQLite is a lightweight, disk-based database that doesn't require a separate server process. It's perfect for small to medium-sized applications and is integrated into Python through the `sqlite3` module.

### Key Features of SQLite

- **Self-contained**: SQLite is a single library that runs with minimal setup.
- **Serverless**: No server required, making it easy to set up and manage.
- **Zero configuration**: No setup or administration needed.
- **Cross-platform**: Works on all major operating systems.

## 3. Setting Up SQLite in Python

First, ensure you have Python installed. The `sqlite3` module is included with Python's standard library, so no additional installation is needed.

### Connecting to SQLite

Here's how to connect to a SQLite database:

```python
import sqlite3

# Connect to the database (or create it if it doesn't exist)
conn = sqlite3.connect('bank_system.db')

# Create a cursor object to interact with the database
cursor = conn.cursor()
```

## 4. Designing the Database Schema

We'll design a simple schema with two tables: `users` and `accounts`.

### Users Table

- `id` (INTEGER, PRIMARY KEY, AUTOINCREMENT)
- `name` (TEXT)

### Accounts Table

- `account_number` (INTEGER, PRIMARY KEY)
- `balance` (REAL)
- `interest_rate` (REAL, default 0.0)
- `user_id` (INTEGER, FOREIGN KEY referencing `users`)

### Creating Tables

```python
def create_tables():
    cursor.execute('''
    CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL
    )
    ''')

    cursor.execute('''
    CREATE TABLE IF NOT EXISTS accounts (
        account_number INTEGER PRIMARY KEY,
        balance REAL NOT NULL,
        interest_rate REAL DEFAULT 0.0,
        user_id INTEGER,
        FOREIGN KEY (user_id) REFERENCES users (id)
    )
    ''')

    # Commit changes
    conn.commit()

create_tables()
```

## 5. Implementing SQLite in Your Bank System

### Saving Users

First, let's modify our `User` class to save user information to the database.

```python
class User:
    def __init__(self, name, user_id=None):
        self.user_id = user_id
        self.name = name
        self.accounts = []

    def save_to_db(self):
        if self.user_id is None:
            cursor.execute('INSERT INTO users (name) VALUES (?)', (self.name,))
            self.user_id = cursor.lastrowid
        else:
            cursor.execute('UPDATE users SET name = ? WHERE id = ?', (self.name, self.user_id))
        conn.commit()

    def add_account(self, account):
        self.accounts.append(account)
        account.user_id = self.user_id
        account.save_to_db()
        print(f"Account {account.account_number} added to user {self.name}")

    def remove_account(self, account_number):
        self.accounts = [acc for acc in self.accounts if acc.account_number != account_number]
        cursor.execute('DELETE FROM accounts WHERE account_number = ?', (account_number,))
        conn.commit()
        print(f"Account {account_number} removed from user {self.name}")

    def get_accounts(self):
        cursor.execute('SELECT account_number, balance, interest_rate FROM accounts WHERE user_id = ?', (self.user_id,))
        rows = cursor.fetchall()
        self.accounts = [BankAccount(balance=row[1], account_number=row[0], interest_rate=row[2]) for row in rows]
        return self.accounts
```

### Saving Accounts

Next, let's modify our `BankAccount` and `SavingsAccount` classes to save account information to the database.

```python
class BankAccount:
    account_number_counter = 1000

    def __init__(self, balance=0, account_number=None, interest_rate=0.0, user_id=None):
        self.account_number = account_number if account_number is not None else BankAccount.account_number_counter
        BankAccount.account_number_counter += 1
        self.balance = balance
        self.interest_rate = interest_rate
        self.user_id = user_id

    def save_to_db(self):
        cursor.execute('''
        INSERT OR REPLACE INTO accounts (account_number, balance, interest_rate, user_id)
        VALUES (?, ?, ?, ?)
        ''', (self.account_number, self.balance, self.interest_rate, self.user_id))
        conn.commit()

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            self.save_to_db()
            print(f"Deposited {amount}. New balance: {self.balance}")
        else:
            print("Deposit amount must be positive")

    def withdraw(self, amount):
        if 0 < amount <= self.balance:
            self.balance -= amount
            self.save_to_db()
            print(f"Withdrew {amount}. New balance: {self.balance}")
        else:
            print("Insufficient funds or invalid amount")

    def check_balance(self):
        return self.balance

class SavingsAccount(BankAccount):
    def __init__(self, balance=0, interest_rate=0.01, account_number=None, user_id=None):
        super().__init__(balance, account_number, interest_rate, user_id)

    def apply_interest(self):
        interest = self.balance * self.interest_rate
        self.balance += interest
        self.save_to_db()
        print(f"Applied interest {interest}. New balance: {self.balance}")
```

### Bank Class with SQLite Integration

Now, let's update the `Bank` class to handle user and account information using SQLite.

```python
class Bank:
    def __init__(self):
        self.users = []

    def add_user(self, user):
        user.save_to_db()
        self.users.append(user)
        print(f"User {user.name} added")

    def find_user(self, user_id):
        cursor.execute('SELECT id, name FROM users WHERE id = ?', (user_id,))
        row = cursor.fetchone()
        if row:
            user = User(row[1], row[0])
            user.get_accounts()
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

## 6. Testing Your SQLite Integration

Let's test our updated bank system to ensure everything works correctly.

```python
# Create bank instance
bank = Bank()

# Create users
user1 = User("Alice")
user2 = User("Bob")

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

# Retrieve user and account information
retrieved_user1 = bank.find_user(user1.user_id)
retrieved_user2 = bank.find_user(user2.user_id)
print(f"User: {retrieved_user1.name}, Accounts: {[acc.account_number for acc in retrieved_user1.get_accounts()]}")
print(f"User: {retrieved_user2.name}, Accounts: {[acc.account_number for acc in retrieved_user2.get_accounts()]}")
```

## 7. Conclusion

In this tutorial, we extended our bank system to use SQLite for storing user and account information. We covered setting up SQLite, designing the database schema, and integrating it into our existing bank system. By saving data to a database, our system is now more robust and can persist data across sessions. Happy coding!

Feel free to add more features, such as transaction history and user authentication, to make your system even more powerful!