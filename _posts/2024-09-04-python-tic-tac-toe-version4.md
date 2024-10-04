---
layout: post
title: "[Python] 4. Advanced Tic-Tac-Toe Game Ver.4 (with Classes!)"
date: 2024-09-04 08:00:00 +0600
tags: python-tutorial examples game
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

Great! Now that you’re ready to introduce object-oriented programming (OOP) into your Tic-Tac-Toe game, it’s the perfect opportunity to learn about classes and inheritance in Python. We’re going to enhance the game by creating a `Player` class and then two subclasses: `HumanPlayer` and `RandomPlayer`. This approach will help you understand how to create reusable code and apply the principles of OOP, such as inheritance and polymorphism.

# Building a Tic-Tac-Toe Game in Python Using Classes

In this tutorial, we’re going to take our Tic-Tac-Toe game to the next level by applying object-oriented programming concepts. We’ll create player objects that interact with the game board, and each type of player will have its own behavior. By using classes, we can make the code cleaner, more scalable, and easier to maintain.

So, let’s break it down and dive into the world of classes in Python!

## Step 1: Understanding Classes and Inheritance

Before we dive into the code, let’s quickly review some OOP concepts:

- **Classes**: A class is like a blueprint for creating objects (instances). In our case, we’ll have a `Player` class that defines common behaviors for all players.
- **Inheritance**: Subclasses can inherit properties and methods from a parent class. For example, `HumanPlayer` and `RandomPlayer` will inherit from the `Player` class.
- **Polymorphism**: This allows different objects to be treated as instances of the same class. In our game, both human and random players will be able to take a turn, but they’ll do it differently.

## Step 2: Creating the `Player` Class

Let’s start by defining a parent class called `Player`. This class will be a template for both human and random players. We’ll also give it a method called `get_move()` that will be overridden by subclasses to define how each player chooses their move.

```python
class Player:
    def __init__(self, marker):
        self.marker = marker
    
    def get_move(self, board):
        pass  # This will be overridden by subclasses
```

- **`__init__` method**: This is the constructor method, and it will set the player’s marker (`X` or `O`).
- **`get_move()` method**: This method will be defined differently depending on the type of player (human or random).

## Step 3: Defining the `HumanPlayer` Class

Now, let’s create the `HumanPlayer` class, which inherits from `Player`. The human player will be prompted to input their move manually.

```python
class HumanPlayer(Player):
    def get_move(self, board):
        while True:
            move = input(f"Player {self.marker}, choose a position (1-9): ")
            if move in board:
                return int(move) - 1
            else:
                print("Invalid move. Try again.")
```

Here’s how it works:

- **Inheritance**: The `HumanPlayer` class inherits from `Player`, meaning it automatically gets all the methods and properties of the parent class.
- **`get_move()`**: This method asks the human player to choose a position, validates the input, and returns the selected index on the board.

## Step 4: Creating the `RandomPlayer` Class

Next, let’s add a `RandomPlayer` class, which will choose a move randomly from the available positions on the board. This is great for testing the game or when you want to add AI later.

```python
import random

class RandomPlayer(Player):
    def get_move(self, board):
        available_moves = [i for i, spot in enumerate(board) if spot.isdigit()]
        move = random.choice(available_moves)
        print(f"Random player {self.marker} chooses position {move + 1}")
        return move
```

- **`get_move()`**: Instead of prompting the user for input, the random player selects a move from the available spots on the board using Python’s `random.choice()`.

## Step 5: Updating the Game Logic to Use Players

Now that we have the player classes, we can update the main game logic to handle these player objects. Let’s rewrite the `tic_tac_toe()` function to integrate human and random players.

```python
def display_board(board):
    print("\n" + board[0] + " | " + board[1] + " | " + board[2])
    print("--+---+--")
    print(board[3] + " | " + board[4] + " | " + board[5])
    print("--+---+--")
    print(board[6] + " | " + board[7] + " | " + board[8])

def check_winner(board):
    winning_combinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6]
    ]
    for combo in winning_combinations:
        if board[combo[0]] == board[combo[1]] == board[combo[2]]:
            return True
    return False

def check_draw(turns):
    return turns == 9

def tic_tac_toe():
    board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
    player1 = HumanPlayer("X")
    player2 = RandomPlayer("O")
    players = [player1, player2]
    turns = 0
    game_over = False

    while not game_over:
        current_player = players[turns % 2]
        display_board(board)
        move = current_player.get_move(board)
        board[move] = current_player.marker
        turns += 1

        if check_winner(board):
            display_board(board)
            print(f"\nPlayer {current_player.marker} wins!")
            game_over = True
        elif check_draw(turns):
            display_board(board)
            print("\nIt's a draw!")
            game_over = True
```

### Key Changes:

1. **Player Objects**: We create instances of `HumanPlayer` and `RandomPlayer`, each with its own marker ("X" and "O").
2. **Alternating Turns**: We use the `players` list and `turns % 2` to alternate between the two players.
3. **Player Actions**: The `get_move()` method is called on each player's turn, making the code flexible and reusable. If you want to add more types of players in the future (like a smarter AI), you just need to create a new subclass!

## Full Code

Here’s the complete code with object-oriented design and player classes:

```python
import random

class Player:
    def __init__(self, marker):
        self.marker = marker
    
    def get_move(self, board):
        pass

class HumanPlayer(Player):
    def get_move(self, board):
        while True:
            move = input(f"Player {self.marker}, choose a position (1-9): ")
            if move in board:
                return int(move) - 1
            else:
                print("Invalid move. Try again.")

class RandomPlayer(Player):
    def get_move(self, board):
        available_moves = [i for i, spot in enumerate(board) if spot.isdigit()]
        move = random.choice(available_moves)
        print(f"Random player {self.marker} chooses position {move + 1}")
        return move

def display_board(board):
    print("\n" + board[0] + " | " + board[1] + " | " + board[2])
    print("--+---+--")
    print(board[3] + " | " + board[4] + " | " + board[5])
    print("--+---+--")
    print(board[6] + " | " + board[7] + " | " + board[8])

def check_winner(board):
    winning_combinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6]
    ]
    for combo in winning_combinations:
        if board[combo[0]] == board[combo[1]] == board[combo[2]]:
            return True
    return False

def check_draw(turns):
    return turns == 9

def tic_tac_toe():
    board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
    player1 = HumanPlayer("X")
    player2 = RandomPlayer("O")
    players = [player1, player2]
    turns = 0
    game_over = False

    while not game_over:
        current_player = players[turns % 2]
        display_board(board)
        move = current_player.get_move(board)
        board[move] = current_player.marker
        turns += 1

        if check_winner(board):
            display_board(board)
            print(f"\nPlayer {current_player.marker} wins!")
            game_over = True
        elif check_draw(turns):
            display_board(board)
            print("\nIt's a draw!")
            game_over = True
```

##