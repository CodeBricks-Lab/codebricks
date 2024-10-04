---
layout: post
title: "[Python] 1. Simple Tic-Tac-Toe Game Ver.1"
date: 2024-09-01 08:00:00 +0600
tags: python-tutorial examples game
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

We're going to create a Tic-Tac-Toe game in Python, just like the one we've all played many times before. We'll start by building the simplest version of the game, and over time, we’ll improve it by introducing important concepts like object-oriented programming and networking. The Tic-Tac-Toe I’m introducing today is version 1, the most basic version. If you're familiar with lists, indexes, while loops, and if-else statements, you'll be able to make it. So, shall we get started?

# How to Create a Simple Tic-Tac-Toe Game in Python

Hi there! Welcome to this beginner-friendly tutorial on how to create a simple Tic-Tac-Toe game in Python. If you’re just starting out with Python and want to build something fun, this project is a perfect way to practice your skills! We’re going to keep things really simple—no functions or classes—just basic Python code that you can understand easily.

Ready? Let’s jump in!

## Step 1: Setting Up the Game Board

First, we need a way to represent the Tic-Tac-Toe board. For this, we’ll use a 3x3 grid. You can imagine the grid as a list of 9 elements, where each element is either a number (representing an empty spot), or an "X" or "O" (the player’s moves). Here’s how we can set it up:

```python
board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
```

These numbers represent the positions where players will place their Xs and Os. Eventually, as the game progresses, these numbers will be replaced by either "X" or "O".

## Step 2: Displaying the Game Board

Let’s write some code to print the game board so players can see what’s happening. We’ll arrange the board in a 3x3 format so it looks like a Tic-Tac-Toe grid.

```python
print(board[0] + " | " + board[1] + " | " + board[2])
print("--+---+--")
print(board[3] + " | " + board[4] + " | " + board[5])
print("--+---+--")
print(board[6] + " | " + board[7] + " | " + board[8])
```

This code will display the board in a way that’s easy for the players to understand.

## Step 3: Handling Player Turns

Next, we need to let the players take turns. We’ll use a simple loop to ask each player where they want to place their mark (X or O). Player 1 will always use "X", and Player 2 will use "O".

Let’s write some code to alternate between the two players and allow them to choose their moves:

```python
player = "X"
game_over = False
turns = 0
```

We’ll use the `player` variable to track whose turn it is, and `game_over` to check if someone has won. The `turns` variable will keep track of how many moves have been made.

Here’s how we can ask players for their move:

```python
while not game_over:
    print("\nCurrent board:")
    print(board[0] + " | " + board[1] + " | " + board[2])
    print("--+---+--")
    print(board[3] + " | " + board[4] + " | " + board[5])
    print("--+---+--")
    print(board[6] + " | " + board[7] + " | " + board[8])
    
    move = input(f"\nPlayer {player}, choose a position (1-9): ")
    
    if move in board:
        board[int(move) - 1] = player
        turns += 1
    else:
        print("Invalid move. Try again.")
        continue

    # Check if someone has won or if it's a draw
    if (board[0] == board[1] == board[2]) or (board[3] == board[4] == board[5]) or (board[6] == board[7] == board[8]) or \
       (board[0] == board[3] == board[6]) or (board[1] == board[4] == board[7]) or (board[2] == board[5] == board[8]) or \
       (board[0] == board[4] == board[8]) or (board[2] == board[4] == board[6]):
        game_over = True
        print(f"\nPlayer {player} wins!")
    elif turns == 9:
        game_over = True
        print("\nIt's a draw!")
    else:
        player = "O" if player == "X" else "X"
```

Here’s a quick breakdown of what this code does:

1. **Display the board**: After each move, the current state of the board is printed.
2. **Ask for input**: The player chooses a position between 1 and 9. We check if the input is valid (i.e., the chosen position isn’t already taken).
3. **Place the mark**: The chosen position is updated with the player's mark (either "X" or "O").
4. **Check for a winner**: We check all the possible winning combinations (rows, columns, and diagonals).
5. **Switch turns**: If nobody wins, we switch to the other player and continue the loop.

## Step 4: Checking for a Winner or a Draw

As soon as a player makes a move, we need to check if they’ve won or if the game ends in a draw. In our code, we’re checking all possible win conditions:

- Three in a row (horizontally, vertically, or diagonally).
- If the board is full after 9 moves and no one has won, it’s a draw.

If there’s a winner, we print a message, and if all spots are taken without a winner, it’s declared a draw.

## Conclusion

That’s it! You’ve just built a basic Tic-Tac-Toe game in Python. Let’s look at the full code all together:

```python
# Tic-Tac-Toe Game

board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]

player = "X"
game_over = False
turns = 0

while not game_over:
    print("\nCurrent board:")
    print(board[0] + " | " + board[1] + " | " + board[2])
    print("--+---+--")
    print(board[3] + " | " + board[4] + " | " + board[5])
    print("--+---+--")
    print(board[6] + " | " + board[7] + " | " + board[8])
    
    move = input(f"\nPlayer {player}, choose a position (1-9): ")
    
    if move in board:
        board[int(move) - 1] = player
        turns += 1
    else:
        print("Invalid move. Try again.")
        continue

    if (board[0] == board[1] == board[2]) or (board[3] == board[4] == board[5]) or (board[6] == board[7] == board[8]) or \
       (board[0] == board[3] == board[6]) or (board[1] == board[4] == board[7]) or (board[2] == board[5] == board[8]) or \
       (board[0] == board[4] == board[8]) or (board[2] == board[4] == board[6]):
        game_over = True
        print(f"\nPlayer {player} wins!")
    elif turns == 9:
        game_over = True
        print("\nIt's a draw!")
    else:
        player = "O" if player == "X" else "X"
```

Now you have a complete working Tic-Tac-Toe game! Feel free to play around with it, try adding more features, or customize it as you like. Enjoy coding, and happy gaming! 😄