---
layout: post
title: "[Python] 5. Advanced Tic-Tac-Toe Game Ver.5 (with more Classes!)"
date: 2024-09-05 08:00:00 +0600
tags: python-tutorial examples game
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

Fantastic! Now it’s time to fully transition your Tic-Tac-Toe game into a completely object-oriented design by adding a `Board` class and a `Game` class. This will make your code more organized, modular, and easier to expand in the future. Let’s go step by step and refactor the game using these new classes.


# Building an Object-Oriented Tic-Tac-Toe Game in Python

Welcome back! In this tutorial, we’ll complete the transition of our Tic-Tac-Toe game to an object-oriented approach by adding `Board` and `Game` classes. This will give us a clean, structured program that’s easy to maintain and extend. We’ve already worked with player classes, so now we’re going to add even more layers of OOP goodness.

Let’s break it down!

## Step 1: Creating the `Board` Class

First, we’ll define the `Board` class. The board is responsible for maintaining the game state, displaying itself, and checking for winners or draws.

### What the `Board` Class Will Do:

- Store the current state of the game board.
- Display the board to the players.
- Check for a winner or a draw.

Here’s how we can implement it:

```python
class Board:
    def __init__(self):
        self.board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]

    def display(self):
        print("\n" + self.board[0] + " | " + self.board[1] + " | " + self.board[2])
        print("--+---+--")
        print(self.board[3] + " | " + self.board[4] + " | " + self.board[5])
        print("--+---+--")
        print(self.board[6] + " | " + self.board[7] + " | " + self.board[8])

    def update(self, position, marker):
        self.board[position] = marker

    def is_winner(self, marker):
        winning_combinations = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],
            [0, 3, 6], [1, 4, 7], [2, 5, 8],
            [0, 4, 8], [2, 4, 6]
        ]
        for combo in winning_combinations:
            if self.board[combo[0]] == self.board[combo[1]] == self.board[combo[2]] == marker:
                return True
        return False

    def is_draw(self):
        return all(spot in ["X", "O"] for spot in self.board)
```

### Explanation:

- **`__init__()`**: Initializes the board as a list of strings, each representing a position from 1 to 9.
- **`display()`**: Prints the current state of the board.
- **`update()`**: Updates the board when a player makes a move by placing their marker in the correct position.
- **`is_winner()`**: Checks whether the given marker (X or O) has a winning combination on the board.
- **`is_draw()`**: Checks whether all spots are filled without a winner, resulting in a draw.

## Step 2: Creating the `Game` Class

Now, we’ll create a `Game` class that will handle the flow of the game. This class will manage turns, check for a winner, and coordinate interactions between players and the board.

### What the `Game` Class Will Do:

- Manage the game loop.
- Alternate turns between the players.
- Determine when the game ends (either win or draw).

Here’s the implementation:

```python
class Game:
    def __init__(self, player1, player2):
        self.board = Board()  # Create a new board
        self.players = [player1, player2]
        self.turns = 0
        self.current_player = self.players[self.turns % 2]

    def play(self):
        game_over = False

        while not game_over:
            self.board.display()  # Display the board
            print(f"It's {self.current_player.marker}'s turn.")
            move = self.current_player.get_move(self.board.board)  # Get the move from the current player
            self.board.update(move, self.current_player.marker)  # Update the board with the move
            self.turns += 1

            if self.board.is_winner(self.current_player.marker):
                self.board.display()
                print(f"\nPlayer {self.current_player.marker} wins!")
                game_over = True
            elif self.board.is_draw():
                self.board.display()
                print("\nIt's a draw!")
                game_over = True
            else:
                self.current_player = self.players[self.turns % 2]  # Switch player for the next turn
```

### Explanation:

- **`__init__()`**: The game initializes with two players and a fresh `Board` object. It tracks whose turn it is and alternates players.
- **`play()`**: This method contains the game loop. It displays the board, gets the current player’s move, updates the board, checks for a winner or a draw, and alternates turns until the game ends.

## Step 3: Refactoring the `Player`, `HumanPlayer`, and `RandomPlayer` Classes

Now that we have the `Board` and `Game` classes in place, we can easily integrate the player classes we’ve already built.

Here’s a quick recap of the player classes with minor updates:

```python
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
```

These classes remain the same, with the `get_move()` method responsible for determining how each player makes their move. The `Game` class calls this method to get the player’s choice during each turn.

## Step 4: Running the Game

Now that we have all our pieces in place (pun intended), we can create a simple script to run the game.

```python
def main():
    player1 = HumanPlayer("X")
    player2 = RandomPlayer("O")
    game = Game(player1, player2)
    game.play()

if __name__ == "__main__":
    main()
```

### How It Works:

1. We create two player objects: `player1` is a `HumanPlayer` and `player2` is a `RandomPlayer`.
2. The `Game` class is initialized with these two players.
3. The `game.play()` method starts the game loop, alternating turns until there’s a winner or a draw.

## Full Code with OOP

Here’s the complete code with all the classes:

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

class Board:
    def __init__(self):
        self.board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]

    def display(self):
        print("\n" + self.board[0] + " | " + self.board[1] + " | " + self.board[2])
        print("--+---+--")
        print(self.board[3] + " | " + self.board[4] + " | " + self.board[5])
        print("--+---+--")
        print(self.board[6] + " | " + self.board[7] + " | " + self.board[8])

    def update(self, position, marker):
        self.board[position] = marker

    def is_winner(self, marker):
        winning_combinations = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],
            [0, 3, 6], [1, 4, 7], [2, 5, 8],
            [0, 4, 8], [2, 4, 6]
        ]
        for combo in winning_combinations:
            if self.board[combo[0]] == self.board[combo[1]] == self.board[combo[2]] == marker:
                return True
        return False

    def is_draw(self):
        return all(spot in ["X", "O"]