---
layout: post
title: "[Python] 8. Network Multiplay Tic-Tac-Toe Game Ver.8 (with socket!)"
date: 2024-09-08 08:00:00 +0600
tags: python-tutorial examples game
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

## Step-by-Step Breakdown of the Multiplayer Tic-Tac-Toe Server

In this enhanced version of our Tic-Tac-Toe game, we’re building a server that can manage multiple games between human players over the network. To achieve this, we rely on Python’s `socket` and `threading` libraries.

---

# Introduction to `socket` and `threading` in Python

Before we begin creating a networked Tic-Tac-Toe game, it's important to understand two core Python libraries that make networking and multithreading possible: `socket` and `threading`. These libraries are essential for building client-server applications that can handle multiple connections simultaneously.

## 1. Python `socket` Programming

The `socket` module in Python provides a way to create connections between two machines, allowing them to communicate over a network. Sockets are the backbone of any network communication, including client-server architectures like the one we'll be using.

### Basic Concepts:

- **Server**: Listens for incoming client connections and manages communication.
- **Client**: Connects to a server and interacts with it by sending and receiving data.

### Socket Types:

- **`socket.AF_INET`**: Refers to the address family for IPv4 addresses (e.g., 127.0.0.1).
- **`socket.SOCK_STREAM`**: Specifies that we are using TCP (Transmission Control Protocol), which ensures reliable and ordered communication between the server and client.

### Basic Socket Operations

Let's look at how to create a simple server and client using the `socket` module.

#### Server Example

```python
import socket

def start_server():
    # Create a TCP/IP socket
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Bind the socket to a public host, and a specific port
    server_socket.bind(('localhost', 12345))
    
    # Listen for incoming connections
    server_socket.listen(5)  # The argument specifies the maximum number of queued connections
    
    print("Server is listening on port 12345...")
    
    # Wait for a connection
    client_socket, address = server_socket.accept()
    print(f"Connection from {address} has been established!")
    
    # Send a message to the client
    client_socket.send(bytes("Hello from the server!", "utf-8"))
    
    # Close the connection
    client_socket.close()

if __name__ == "__main__":
    start_server()
```

### Explanation:

1. **`socket.socket()`**: Creates a new socket object. The parameters specify the use of IPv4 (`AF_INET`) and TCP (`SOCK_STREAM`).
2. **`bind()`**: Binds the socket to the localhost (`127.0.0.1`) on port `12345`.
3. **`listen()`**: Tells the server to listen for incoming connections. The argument (`5`) specifies the maximum number of queued connections.
4. **`accept()`**: Waits for a client to connect. When a client connects, it returns a new socket object (`client_socket`) and the address of the client.
5. **`send()`**: Sends data to the client over the socket.

#### Client Example

```python
import socket

def start_client():
    # Create a TCP/IP socket
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Connect to the server
    client_socket.connect(('localhost', 12345))
    
    # Receive data from the server
    message = client_socket.recv(1024)  # Buffer size of 1024 bytes
    print(message.decode("utf-8"))
    
    # Close the connection
    client_socket.close()

if __name__ == "__main__":
    start_client()
```

### Explanation:

1. **`connect()`**: Connects to the server at the specified IP address and port (`localhost`, `12345`).
2. **`recv()`**: Receives data from the server. The argument specifies the buffer size (i.e., the maximum number of bytes to read at once).
3. **`close()`**: Closes the socket connection.

### Running the Server and Client

1. Run the server program first.
2. Then, run the client program to connect to the server.
3. The client will receive the message from the server, and the connection will close.

---

## 2. Python `threading` Programming

The `threading` module allows us to run multiple threads (smaller units of a process) concurrently. This is especially useful for networked applications where we want to handle multiple clients at the same time.

### Why Use `threading`?

In networked applications, the server needs to handle multiple client connections at once. If the server only handled one client at a time, it would be inefficient because it would block other clients from connecting until the current client disconnects. With `threading`, we can spawn a new thread for each client, allowing them to interact with the server simultaneously.

### Basic Example of `threading`

Here’s a simple example that shows how to use `threading` to run multiple tasks at the same time:

```python
import threading
import time

def print_numbers():
    for i in range(5):
        time.sleep(1)
        print(f"Number: {i}")

def print_letters():
    for letter in "abcde":
        time.sleep(1.5)
        print(f"Letter: {letter}")

# Create two threads
thread1 = threading.Thread(target=print_numbers)
thread2 = threading.Thread(target=print_letters)

# Start the threads
thread1.start()
thread2.start()

# Wait for both threads to complete
thread1.join()
thread2.join()

print("Both threads have finished.")
```

### Explanation:

1. **`threading.Thread()`**: Creates a new thread. The `target` parameter specifies the function to run in that thread.
2. **`start()`**: Starts the execution of the thread.
3. **`join()`**: Waits for the thread to finish before moving on to the next line of code.

In this example, `print_numbers()` and `print_letters()` are executed concurrently in separate threads.

---

## 3. Combining `socket` and `threading`

Now that we understand the basics of both `socket` and `threading`, we can combine these concepts to create a server that can handle multiple clients simultaneously. Each time a new client connects, we’ll create a new thread to handle that client, allowing the server to communicate with multiple clients concurrently.

### Multithreaded Server Example

```python
import socket
import threading

def handle_client(client_socket):
    client_socket.send(bytes("Hello, Client!", "utf-8"))
    while True:
        message = client_socket.recv(1024).decode("utf-8")
        if not message:
            break
        print(f"Received from client: {message}")
        client_socket.send(bytes(f"Echo: {message}", "utf-8"))
    client_socket.close()

def start_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind(('localhost', 12345))
    server_socket.listen(5)
    print("Server is listening on port 12345...")
    
    while True:
        client_socket, address = server_socket.accept()
        print(f"Connection established with {address}")
        
        # Create a new thread for each client
        client_thread = threading.Thread(target=handle_client, args=(client_socket,))
        client_thread.start()

if __name__ == "__main__":
    start_server()
```

### Explanation:

1. **`handle_client()`**: This function runs in a separate thread for each client. It listens for messages from the client, echoes them back, and closes the connection when the client disconnects.
2. **`start_server()`**: The main server function that listens for connections and starts a new thread for each client.


## 4. Multiplayer Tic Tac Toe Game

You can now move on to the next section, where we will apply the concepts of `socket` and `threading` to build a fully functioning multiplayer Tic-Tac-Toe game!



### Classes Overview:

1. **`Board` Class**:
   - Manages the game board's state and provides methods to update the board, check for a winner, and check for a draw.
   
2. **`GameRoom` Class**:
   - Manages a single game session between two players, sending board updates and processing moves.
   
3. **`TicTacToeServer` Class**:
   - Manages incoming player connections and pairs them into game rooms. It handles multiple clients by creating separate threads for each game room.

### Detailed Class Explanations

---

### 1. `Board` Class

This class is responsible for maintaining and updating the state of the Tic-Tac-Toe board. It provides methods for displaying the board, updating it when a player makes a move, and checking for a winner or a draw.

```python
class Board:
    def __init__(self):
        self.board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
```

- **`__init__()`**: Initializes the board as a list of strings, where each element is a position (from "1" to "9"). These positions will be replaced with "X" or "O" when a player makes a move.

```python
    def display(self):
        return f"\n{self.board[0]} | {self.board[1]} | {self.board[2]}\n--+---+--\n{self.board[3]} | {self.board[4]} | {self.board[5]}\n--+---+--\n{self.board[6]} | {self.board[7]} | {self.board[8]}"
```

- **`display()`**: This method returns the current state of the board as a formatted string, making it easy for both players to see the game’s current progress.

```python
    def update(self, position, marker):
        self.board[position] = marker
```

- **`update()`**: Updates the board at a specific position (0 to 8) with either "X" or "O". This is called when a player makes a move.

```python
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
```

- **`is_winner()`**: This method checks whether the current player has won. It compares all possible winning combinations (rows, columns, and diagonals) to see if any contain the same marker (either "X" or "O").

```python
    def is_draw(self):
        return all(spot in ["X", "O"] for spot in self.board)
```

- **`is_draw()`**: Checks if the board is full (i.e., all positions are either "X" or "O"). If the board is full and there’s no winner, the game is declared a draw.

---

### 2. `GameRoom` Class

The `GameRoom` class is where the actual game between two players takes place. This class handles game logic, communication between players, and the flow of the game (turns, checking for a winner, etc.).

```python
class GameRoom:
    def __init__(self, player1_socket, player2_socket):
        self.board = Board()
        self.players = [(player1_socket, "X"), (player2_socket, "O")]
        self.current_player = 0
```

- **`__init__()`**: Initializes a new game room with two players. The `players` list stores each player’s socket and their respective marker ("X" or "O"). The `current_player` is used to track whose turn it is.
  
- **Player Sockets**: Each player has a socket connection. This allows the server to send and receive data (like moves) from the client.

```python
    def send_board_to_players(self):
        board_state = self.board.display()
        for player_socket, _ in self.players:
            player_socket.send(bytes(board_state, "utf-8"))
```

- **`send_board_to_players()`**: Sends the current state of the board to both players. The board is displayed after each move to keep both players updated.

```python
    def handle_game(self):
        game_over = False
        while not game_over:
            current_socket, current_marker = self.players[self.current_player]
            opponent_socket, _ = self.players[(self.current_player + 1) % 2]

            self.send_board_to_players()

            current_socket.send(bytes("Your move: ", "utf-8"))
            move = current_socket.recv(1024).decode("utf-8")
```

- **`handle_game()`**: This is the core of the game. It runs in a loop until the game is over. It alternates between players, prompting each player for their move and processing it.
- **Player Turns**: The current player is prompted to make a move using `recv()`, which listens for data from the player's socket. The move is then processed, and the board is updated.

```python
            if move.isdigit() and int(move) in range(1, 10):
                move = int(move) - 1
                if self.board.board[move] not in ["X", "O"]:
                    self.board.update(move, current_marker)
                    if self.board.is_winner(current_marker):
                        self.send_board_to_players()
                        current_socket.send(bytes(f"Player {current_marker} wins!", "utf-8"))
                        opponent_socket.send(bytes(f"Player {current_marker} wins!", "utf-8"))
                        game_over = True
                    elif self.board.is_draw():
                        self.send_board_to_players()
                        current_socket.send(bytes("It's a draw!", "utf-8"))
                        opponent_socket.send(bytes("It's a draw!", "utf-8"))
                        game_over = True
                    else:
                        self.current_player = (self.current_player + 1) % 2
                else:
                    current_socket.send(bytes("Invalid move. Try again.", "utf-8"))
            else:
                current_socket.send(bytes("Invalid move. Try again.", "utf-8"))
```

- **Move Validation**: It checks if the move is valid (i.e., it’s a number between 1 and 9, and the position hasn’t already been taken). If valid, the board is updated.
- **Winner or Draw**: After each move, it checks whether the player has won or if the game has ended in a draw. If so, both players are informed, and the game ends.
- **Turn Switching**: If the game isn’t over, the turn switches to the next player using `self.current_player = (self.current_player + 1) % 2`.

---

### 3. `TicTacToeServer` Class

The `TicTacToeServer` class manages incoming player connections and pairs them up for games. Once two players are connected, they are placed into a game room to play against each other.

```python
class TicTacToeServer:
    def __init__(self):
        self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.server_socket.bind(('localhost', 12345))
        self.server_socket.listen(10)  # Allow multiple clients to connect
        self.waiting_player = None  # Keep track of waiting players
```

- **`__init__()`**: Initializes the server, binds it to `localhost` on port `12345`, and starts listening for connections. The server can handle up to 10 clients simultaneously. The `waiting_player` variable stores a player who is waiting for an opponent.

```python
    def handle_client(self, client_socket):
        if self.waiting_player:
            print("Pairing players to start a game...")
            game_room = GameRoom(self.waiting_player, client_socket)
            threading.Thread(target=game_room.handle_game).start()
            self.waiting_player = None  # Reset waiting player
        else:
            print("Waiting for another player to join...")
            self.waiting_player = client_socket
```

- **`handle_client()`**: This method is responsible for pairing players. When a client connects, if there’s already a player waiting, they are paired up and placed into a `GameRoom`. A new thread is started to handle the game in that room. If no players are waiting, the current player is stored in `waiting_player` until another player joins.

```python
    def start(self):
        print("Server is running and waiting for players...")
        while True:
            client_socket, address = self.server_socket.accept()
            print(f"Player

 connected from {address}")
            threading.Thread(target=self.handle_client, args=(client_socket,)).start()
```

- **`start()`**: The main server loop waits for player connections using `accept()`. When a player connects, a new thread is created to handle that player using `handle_client()`.

---

## Step 3: Updating the Client

The client code allows a player to connect to the server, wait for an opponent, and play the game. The client receives the current state of the board, makes a move, and sends that move back to the server.

```python
import socket

class TicTacToeClient:
    def __init__(self):
        self.client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.client_socket.connect(('localhost', 12345))

    def start(self):
        while True:
            board_state = self.client_socket.recv(1024).decode("utf-8")
            print(board_state)
            if "wins" in board_state or "draw" in board_state:
                break
            move = input("Enter your move (1-9): ")
            self.client_socket.send(bytes(move, "utf-8"))

if __name__ == "__main__":
    client = TicTacToeClient()
    client.start()
```

### Client Explanation:
- **Receiving Board State**: The client receives updates about the game board from the server using `recv()`, which allows the player to see the current state of the game.
- **Sending Moves**: The player inputs a move, which is then sent to the server via `send()` for validation and processing.

---

### Conclusion

We’ve now expanded the Tic-Tac-Toe game server to support multiple games at once, allowing any number of players to connect, be paired up, and play games simultaneously. Each game is handled in its own `GameRoom` thread, and the server manages all connections efficiently. This architecture is highly scalable and can be further expanded with more features like game statistics, matchmaking, or even AI players mixed into the network!

By implementing socket programming and threading, you’ve built a robust multiplayer game server. Keep experimenting and building on this foundation for more advanced projects!

Happy coding! 😄