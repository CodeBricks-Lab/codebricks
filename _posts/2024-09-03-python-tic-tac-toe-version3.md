---
layout: post
title: "[Python] 3. Simple Tic-Tac-Toe Game Ver.3 (with Functions!)"
date: 2024-09-03 08:00:00 +0600
tags: python-tutorial examples game
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

# How to Build a Tic-Tac-Toe Game in Python (with Functions!)

Hey there! In this tutorial, we're going to improve our previous Tic-Tac-Toe game by introducing functions. Functions are a great way to organize your code and avoid repetition, which makes everything easier to read and maintain. If you’re familiar with Python basics like lists, loops, and conditionals, you’re ready to take it to the next level.

So, let’s dive in!

## Why Use Functions?

In our first version of Tic-Tac-Toe, everything was in one big block of code. While that works for small programs, it’s not ideal for scalability or readability. Functions help us:

- **Avoid repetition**: We won’t need to repeat the same blocks of code multiple times.
- **Make the code more readable**: Each function will have a clear purpose, making it easier to follow.
- **Easier to modify and debug**: When you need to make changes or fix bugs, it’s easier to work with smaller chunks of code.

Let’s get started by breaking our game down into smaller tasks that we can turn into functions.

## Step 1: Creating the Game Board Function

We need a function to display the game board. This is something we’ll do multiple times during the game, so it’s a perfect candidate for a function.

```python
def display_board(board):
    print("\n" + board[0] + " | " + board[1] + " | " + board[2])
    print("--+---+--")
    print(board[3] + " | " + board[4] + " | " + board[5])
    print("--+---+--")
    print(board[6] + " | " + board[7] + " | " + board[8])
```

This function takes the `board` as an argument and prints it in a 3x3 format. Now, instead of writing this block of code every time we want to show the board, we just call `display_board(board)`.

## Step 2: Checking for a Winner

In our original version, the code to check for a winner was scattered throughout the loop. Let’s put that logic into a function. This will make it easier to read and modify later if needed.

```python
def check_winner(board):
    # All possible winning combinations
    winning_combinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],  # rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8],  # columns
        [0, 4, 8], [2, 4, 6]              # diagonals
    ]
    
    for combo in winning_combinations:
        if board[combo[0]] == board[combo[1]] == board[combo[2]]:
            return True
    return False
```

This function checks if any of the players has won by comparing the positions in the `board` list against the winning combinations. If there’s a winner, it returns `True`. Otherwise, it returns `False`.

## Step 3: Managing Player Turns

We also need to manage which player’s turn it is and check if a move is valid. Let’s write a function that handles the turn-taking process.

```python
def player_turn(player, board):
    while True:
        move = input(f"Player {player}, choose a position (1-9): ")
        if move in board:
            board[int(move) - 1] = player
            break
        else:
            print("Invalid move. Try again.")
```

This function ensures that a player can only choose an available position. It keeps asking for input until a valid move is made.

## Step 4: Checking for a Draw

We need to check if the board is full and there is no winner. If all positions are taken and no player has won, the game ends in a draw.

```python
def check_draw(turns):
    return turns == 9
```

This function returns `True` if the game has gone through 9 turns (which means the board is full).

## Step 5: Putting It All Together

Now that we’ve created our functions, it’s time to put everything together into the main game loop.

```python
def tic_tac_toe():
    board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
    player = "X"
    turns = 0
    game_over = False

    while not game_over:
        display_board(board)
        player_turn(player, board)
        turns += 1

        if check_winner(board):
            display_board(board)
            print(f"\nPlayer {player} wins!")
            game_over = True
        elif check_draw(turns):
            display_board(board)
            print("\nIt's a draw!")
            game_over = True
        else:
            player = "O" if player == "X" else "X"
```

Let’s break down the main loop:

1. **Display the board**: The `display_board(board)` function shows the current state of the game.
2. **Player’s move**: The `player_turn(player, board)` function ensures that the player makes a valid move.
3. **Check for a winner**: The `check_winner(board)` function determines if the current player has won.
4. **Check for a draw**: If there is no winner and the board is full, the game ends in a draw.
5. **Switch players**: If nobody wins, the turn switches to the other player.

## Full Code with Functions

Here’s the complete code for the improved Tic-Tac-Toe game:

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

def player_turn(player, board):
    while True:
        move = input(f"Player {player}, choose a position (1-9): ")
        if move in board:
            board[int(move) - 1] = player
            break
        else:
            print("Invalid move. Try again.")

def check_draw(turns):
    return turns == 9

def tic_tac_toe():
    board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
    player = "X"
    turns = 0
    game_over = False

    while not game_over:
        display_board(board)
        player_turn(player, board)
        turns += 1

        if check_winner(board):
            display_board(board)
            print(f"\nPlayer {player} wins!")
            game_over = True
        elif check_draw(turns):
            display_board(board)
            print("\nIt's a draw!")
            game_over = True
        else:
            player = "O" if player == "X" else "X"
```

## Conclusion

By using functions, we’ve improved the structure and readability of our Tic-Tac-Toe game. Each function has a specific job, which makes the code cleaner and easier to maintain. This version of the game is more organized, and it’s easier to expand with new features later on (like adding a replay option, AI opponents, or even network play!).

Now that you know how to break down a game into smaller parts using functions, you can apply this approach to other projects as well. Keep coding, and have fun! 😄
