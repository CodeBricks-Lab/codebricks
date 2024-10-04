---
layout: post
title: "[Python OOP Examples] 3. Improve Bank System with SQLite "
date: 2024-06-08 08:00:00 +0600
tags: python-tutorial object-oriented-programming examples database
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

# Building a Simple Bank System with Python and SQLite

In this tutorial, we'll build a simple bank system using Python and SQLite. This system will allow users to have multiple bank accounts, perform transactions like deposits and withdrawals, and even transfer money between accounts. Let's get started!

### Step 1: Set Up the Database

First, we need to set up our database to store user and account information. We'll use SQLite, which is a simple and easy-to-use database.

```python
import sqlite3

# Connect to SQLite database (or create it if it doesn't exist)
conn = sqlite3.connect('bank.db')
cursor = conn.cursor()

# Create tables for users and accounts
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
    interest_rate REAL,
    user_id INTEGER,
    FOREIGN KEY (user_id) REFERENCES users (id)
)
''')
conn.commit()
```

### Step 2: Create User and Account Classes

Next, let's create classes to represent users and bank accounts. We'll also add methods to save data to the database.

```python
class User:
    def __init__(self, name, user_id=None):
        # Initialize the User object with a name and optional user_id
        self.user_id = user_id
        self.name = name
        self.accounts = []

    def save_to_db(self):
        # Save the user to the database
        if self.user_id is None:
            # Insert a new user if user_id is not set
            cursor.execute('INSERT INTO users (name) VALUES (?)', (self.name,))
            self.user_id = cursor.lastrowid
        else:
            # Update the user if user_id is set
            cursor.execute('UPDATE users SET name = ? WHERE id = ?', (self.name, self.user_id))
        conn.commit()

    def add_account(self, account):
        # Add an account to the user and save it to the database
        self.accounts.append(account)
        account.user_id = self.user_id
        account.save_to_db()
        print(f"Account {account.account_number} added to user {self.name}")

    def get_accounts(self):
        # Retrieve all accounts for the user from the database
        cursor.execute('SELECT account_number, balance, interest_rate FROM accounts WHERE user_id = ?', (self.user_id,))
        rows = cursor.fetchall()
        self.accounts = [BankAccount(balance=row[1], account_number=row[0], interest_rate=row[2], user_id=self.user_id) for row in rows]
        return self.accounts

class BankAccount:
    account_number_counter = 1000

    def __init__(self, balance=0, account_number=None, interest_rate=0.0, user_id=None):
        # Initialize the BankAccount object with balance, account_number, interest_rate, and user_id
        self.account_number = account_number if account_number is not None else BankAccount.account_number_counter
        BankAccount.account_number_counter += 1
        self.balance = balance
        self.interest_rate = interest_rate
        self.user_id = user_id

    def save_to_db(self):
        # Save the account to the database
        cursor.execute('''
        INSERT OR REPLACE INTO accounts (account_number, balance, interest_rate, user_id)
        VALUES (?, ?, ?, ?)
        ''', (self.account_number, self.balance, self.interest_rate, self.user_id))
        conn.commit()

    def deposit(self, amount):
        # Deposit money into the account
        if amount > 0:
            self.balance += amount
            self.save_to_db()
            print(f"Deposited {amount}. New balance: {self.balance}")
        else:
            print("Deposit amount must be positive")

    def withdraw(self, amount):
        # Withdraw money from the account
        if 0 < amount <= self.balance:
            self.balance -= amount
            self.save_to_db()
            print(f"Withdrew {amount}. New balance: {self.balance}")
        else:
            print("Insufficient funds or invalid amount")

    def check_balance(self):
        # Check the balance of the account
        return self.balance

class SavingsAccount(BankAccount):
    def __init__(self, balance=0, interest_rate=0.01, account_number=None, user_id=None):
        # Initialize the SavingsAccount object with balance, interest_rate, account_number, and user_id
        super().__init__(balance, account_number, interest_rate, user_id)

    def apply_interest(self):
        # Apply interest to the savings account
        interest = self.balance * self.interest_rate
        self.balance += interest
        self.save_to_db()
        print(f"Applied interest {interest}. New balance: {self.balance}")
```

### Step 3: Create the Bank Class

Now, let's create the `Bank` class to manage users and handle transfers between accounts.

```python
class Bank:
    def __init__(self):
        # Initialize the Bank object
        self.users = []

    def add_user(self, user):
        # Add a user to the bank and save to the database
        user.save_to_db()
        self.users.append(user)
        print(f"User {user.name} added")

    def find_user(self, user_id):
        # Find a user by user_id in the database
        cursor.execute('SELECT id, name FROM users WHERE id = ?', (user_id,))
        row = cursor.fetchone()
        if row:
            user = User(row[1], row[0])
            user.get_accounts()
            return user
        return None

    def transfer(self, from_account, to_account, amount):
        # Transfer money from one account to another
        if from_account.balance >= amount:
            from_account.withdraw(amount)
            to_account.deposit(amount)
            print(f"Transferred {amount} from account {from_account.account_number} to account {to_account.account_number}")
        else:
            print("Insufficient funds for transfer")
```

### Step 4: Test the Bank System

Finally, let's test our bank system by creating users, adding accounts, performing transactions, and transferring money.

```python
# Testing the implementation
# Create a bank instance
bank = Bank()

# Create users
user1 = User("Alice")
user2 = User("Bob")

# Add users to the bank
bank.add_user(user1)
bank.add_user(user2)

# Create multiple accounts for each user
account1 = BankAccount()
account2 = SavingsAccount(interest_rate=0.05)
account3 = BankAccount(balance=500)
account4 = BankAccount()
account5 = SavingsAccount(interest_rate=0.03)

# Add accounts to users
user1.add_account(account1)
user1.add_account(account2)
user1.add_account(account3)
user2.add_account(account4)
user2.add_account(account5)

# Perform transactions for user1
account1.deposit(1000)
account1.withdraw(500)
account2.deposit(2000)
account2.apply_interest()
account3.deposit(300)
account3.withdraw(100)

# Perform transactions for user2
account4.deposit(1500)
account4.withdraw(700)
account5.deposit(2500)
account5.apply_interest()

# Transfer money between accounts of the same user
bank.transfer(account1, account2, 200)

# Transfer money between users
bank.transfer(account1, account4, 300)

# Retrieve user and account information
retrieved_user1 = bank.find_user(user1.user_id)
retrieved_user2 = bank.find_user(user2.user_id)
print(f"User: {retrieved_user1.name}, Accounts: {[acc.account_number for acc in retrieved_user1.get_accounts()]}")
print(f"User: {retrieved_user2.name}, Accounts: {[acc.account_number for acc in retrieved_user2.get_accounts()]}")

# Check balances of retrieved accounts
for account in retrieved_user1.get_accounts():
    print(f"User: {retrieved_user1.name}, Account {account.account_number} balance: {account.check_balance()}")

for account in retrieved_user2.get_accounts():
    print(f"User: {retrieved_user2.name}, Account {account.account_number} balance: {account.check_balance()}")
```

### Conclusion

In this tutorial, we've improved our simple bank system that allows users to have multiple accounts, perform transactions, and transfer money. We've used Python and SQLite to manage our data. You can extend this system further by adding more features like account statements, user authentication, and more. Happy coding!