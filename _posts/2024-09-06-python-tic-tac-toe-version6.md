---
layout: post
title: "[Python] 6. AI Tic-Tac-Toe Game Ver.6 (with ChatGPT!)"
date: 2024-09-06 08:00:00 +0600
tags: python-tutorial examples game
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

Great! Now that we’re ready to add even more players to our Tic-Tac-Toe game, let’s take it up a notch by introducing a `GPTPlayer` that uses ChatGPT-like logic for its moves. This will showcase the flexibility of our object-oriented design and make the game more interesting.

In this tutorial, we’ll walk through how to add this new AI-powered player to the game. This player will use GPT-like logic to determine its moves based on the current state of the game board. We’ll also maintain the simplicity of the design, while making it clear how to integrate more complex AI players.

---

# Adding a GPT AI Player to Your Tic-Tac-Toe Game in Python

Welcome back! In this tutorial, we’re going to add an exciting new player to our Tic-Tac-Toe game: `GPTPlayer`. This player will simulate an AI (like ChatGPT) that "thinks" about its moves based on the current game state.

We’ll keep things flexible so that you can replace the GPT logic with a real AI model in the future if you want. But for now, let’s focus on how to add this type of player using object-oriented programming principles.

## Step 1: Creating the `GPTPlayer` Class

We’re going to create a new class called `GPTPlayer` that will inherit from the `Player` class, just like the `HumanPlayer` and `RandomPlayer`. The `GPTPlayer` will be a more advanced player, simulating AI logic.

Here’s the implementation:

```python
class GPTPlayer(Player):
    def get_move(self, board):
        print(f"GPT player {self.marker} is thinking...")
        
        # Simulating GPT's thinking process
        # For now, let's have GPTPlayer make a smart choice by picking the first available spot
        available_moves = [i for i, spot in enumerate(board) if spot.isdigit()]
        move = available_moves[0]  # Simulate "intelligent" behavior by picking the first available move
        print(f"GPT player {self.marker} chooses position {move + 1}")
        return move
```

### Explanation:

- **`GPTPlayer` class**: This class extends the `Player` class and overrides the `get_move()` method.
- **Simulated GPT Logic**: To simulate an AI's thinking process, we print a message indicating the AI is "thinking." In this version, the `GPTPlayer` picks the first available move from the board, but you can expand this with more complex decision-making logic later (e.g., using machine learning or GPT models).

For now, this is a simple implementation, but it lays the groundwork for more sophisticated AI logic that you could implement in the future.

## Step 2: Integrating `GPTPlayer` into the Game

Now that we’ve defined our `GPTPlayer`, we need to update the `Game` class to allow the use of this player. The good news is that our `Game` class is already flexible enough to handle any type of player, thanks to its object-oriented design.

Here’s how we can set up the game with the new `GPTPlayer`:

```python
def main():
    player1 = HumanPlayer("X")
    player2 = GPTPlayer("O")  # Use GPTPlayer as the opponent
    game = Game(player1, player2)
    game.play()

if __name__ == "__main__":
    main()
```

### Explanation:

- **`player2 = GPTPlayer("O")`**: We create an instance of `GPTPlayer` and assign it the marker "O". This makes it the second player in the game, playing against the human player.

Now, when the game runs, the human player will play against the `GPTPlayer`, which uses a basic decision-making process to make its moves.

## Step 3: Complete Code with `GPTPlayer`

Here’s the complete code, integrating the new `GPTPlayer` into our object-oriented Tic-Tac-Toe game:

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

class GPTPlayer(Player):
    def get_move(self, board):
        print(f"GPT player {self.marker} is thinking...")
        available_moves = [i for i, spot in enumerate(board) if spot.isdigit()]
        move = available_moves[0]  # Pick the first available move for now
        print(f"GPT player {self.marker} chooses position {move + 1}")
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
        return all(spot in ["X", "O"] for spot in self.board)

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

def main():
    player1 = HumanPlayer("X")
    player2 = GPTPlayer("O")  # Use GPTPlayer as the opponent
    game = Game(player1, player2)
    game.play()

if __name__ == "__main__":
    main()
```

## Step 4: Future Improvements for `GPTPlayer`

The `GPTPlayer` in this version is quite simple, but you can easily extend it with more complex logic. For example, you could integrate a real AI model or create custom decision-making algorithms to simulate smarter gameplay. Here are some ideas:

- **Minimax Algorithm**: Implement a minimax algorithm for the `GPTPlayer` to simulate "perfect" play.
- **API Integration**: Replace the `GPTPlayer`'s logic with a real API call to a model like OpenAI’s GPT for move suggestions.
- **Machine Learning**: Train an AI to play Tic-Tac-Toe by learning from games it plays.

## Conclusion

By adding the `GPTPlayer` to our Tic-Tac-Toe game, we’ve shown how flexible and powerful object-oriented programming can be. Our `Game` class remains unchanged even though we’ve added a more advanced player. This is the beauty of OOP—new features can be integrated seamlessly without disrupting the rest of the system.

Feel free to experiment with different types of players, AI logic, and improvements. Happy coding! 😄