---
layout: post
title: "[Python] 7. Network Tic-Tac-Toe Game Ver.7 (with socket!)"
date: 2024-09-07 08:00:00 +0600
tags: python-tutorial examples game
categories: [python]
author: "Code Bricks"
badge_color: "bg-meander"
post_format: "general"
trending: true

---

### Introduction to Python `socket` Programming

Before we dive into adding networking to our Tic-Tac-Toe game, let’s first cover the basics of Python's `socket` library. Sockets are used to create a connection between a server and a client, allowing them to communicate over a network.

In a typical client-server architecture:
- **Server**: Listens for incoming connections and handles requests from clients.
- **Client**: Connects to the server and communicates with it.

Let’s start with a simple introduction to `socket` programming.

---

### Step 1: Basic Python `socket` Tutorial

#### Creating a Server

The server listens for incoming connections and processes requests from clients. Here's how you can create a simple server in Python:

```python
import socket

def start_server():
    # Create a socket object
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Bind the socket to a specific IP and port
    server_socket.bind(('localhost', 12345))  # 'localhost' is the server address, and 12345 is the port
    
    # Start listening for incoming connections (up to 5 clients)
    server_socket.listen(5)
    
    print("Server is listening on port 12345...")
    
    # Accept a client connection
    client_socket, address = server_socket.accept()
    print(f"Connection from {address} has been established!")
    
    # Send a welcome message to the client
    client_socket.send(bytes("Welcome to the server!", "utf-8"))
    
    # Close the connection
    client_socket.close()

if __name__ == "__main__":
    start_server()
```

### Explanation:

- **`socket.AF_INET`**: Refers to the address family for IPv4.
- **`socket.SOCK_STREAM`**: Indicates that we want to use TCP (Transmission Control Protocol), which ensures reliable communication.
- **`bind()`**: Binds the socket to an IP address (`localhost` in this case) and a port (`12345`).
- **`listen()`**: Makes the server start listening for incoming connections.
- **`accept()`**: Waits for a client to connect. Once connected, it returns a new socket object (`client_socket`) and the client's address.

#### Creating a Client

Now, let’s create a simple client that can connect to the server:

```python
import socket

def start_client():
    # Create a socket object
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

- **`connect()`**: Connects to the server at the specified address (`localhost`) and port (`12345`).
- **`recv()`**: Receives data from the server. The argument (`1024`) is the buffer size, meaning we can receive up to 1024 bytes at a time.

### Running the Server and Client

1. First, run the server program.
2. Then, run the client program, which will connect to the server and print the welcome message sent by the server.

---

### Step 2: Adding Networking to Tic-Tac-Toe

Now that we have a basic understanding of `socket` programming, let’s add networking capabilities to our Tic-Tac-Toe game. We’ll create two separate programs: one for the **server** and one for the **client**. The server will host the game, and the client will connect to it.

---

### Step 3: Designing the Networked Tic-Tac-Toe

We’ll split the Tic-Tac-Toe game logic between the server and the client:

- **Server**: The server will manage the game board and alternate turns between players. It will also send the game state to clients and process their moves.
- **Client**: The client will receive the game state from the server, make moves, and send those moves back to the server.

#### Server Code

```python
import socket
import threading

class Board:
    def __init__(self):
        self.board = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]

    def display(self):
        return f"\n{self.board[0]} | {self.board[1]} | {self.board[2]}\n--+---+--\n{self.board[3]} | {self.board[4]} | {self.board[5]}\n--+---+--\n{self.board[6]} | {self.board[7]} | {self.board[8]}"

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

class TicTacToeServer:
    def __init__(self):
        self.board = Board()
        self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.server_socket.bind(('localhost', 12345))
        self.server_socket.listen(2)  # Listening for two players
        self.players = []
        self.current_player = 0

    def handle_client(self, client_socket, player_marker):
        client_socket.send(bytes(f"You are Player {player_marker}", "utf-8"))
        while True:
            self.send_board_to_all()
            move = client_socket.recv(1024).decode("utf-8")
            if move.isdigit() and int(move) in range(1, 10):
                move = int(move) - 1
                if self.board.board[move] not in ["X", "O"]:
                    self.board.update(move, player_marker)
                    if self.board.is_winner(player_marker):
                        self.send_board_to_all()
                        client_socket.send(bytes(f"Player {player_marker} wins!", "utf-8"))
                        break
                    elif self.board.is_draw():
                        self.send_board_to_all()
                        client_socket.send(bytes("It's a draw!", "utf-8"))
                        break
                    self.current_player = (self.current_player + 1) % 2
            else:
                client_socket.send(bytes("Invalid move. Try again.", "utf-8"))

    def send_board_to_all(self):
        for player_socket in self.players:
            player_socket.send(bytes(self.board.display(), "utf-8"))

    def start(self):
        print("Server is waiting for players...")
        for _ in range(2):
            client_socket, address = self.server_socket.accept()
            print(f"Player connected from {address}")
            player_marker = "X" if len(self.players) == 0 else "O"
            self.players.append(client_socket)
            threading.Thread(target=self.handle_client, args=(client_socket, player_marker)).start()

if __name__ == "__main__":
    server = TicTacToeServer()
    server.start()
```

### Explanation of Server Code:
- **TicTacToeServer Class**: Manages player connections, sends the game board to players, and processes moves.
- **`handle_client()`**: Handles the game logic for each player, processing moves and checking for a winner.
- **`send_board_to_all()`**: Sends the updated game board to all connected players.
- **Multithreading**: The server creates a new thread for each player, allowing both players to connect and play simultaneously.

#### Client Code

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

### Explanation of Client Code:
- **TicTacToeClient Class**: Manages the connection to the server and interacts with the player.
- **`start()`**: Receives the game board from the server and sends the player’s move back to the server.

### Step 4: Running the Networked Game

1. First, start the **server** program.
2. Then, run the **client** program twice (once for each player).
3. Play Tic-Tac-Toe over the network!

---

### Conclusion

We’ve successfully added networking to our Tic-Tac-Toe game using Python’s `socket` library. With this setup, two players can connect to a central server, play the game, and receive real-time updates