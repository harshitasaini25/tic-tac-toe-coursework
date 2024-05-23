###INTRODUCTION:

This repository contains a Python implementation of the classic Tic Tac Toe game. The game is designed to be played in the console, providing a simple yet enjoyable gaming experience.

HOW TO RUN THIS PROGRAM

Clone the Repository: Clone this repository to your local machine using the following command:
git clone https://github.com/harshitasaini25/tic-tac-toe.git

Navigate to the Directory: Change your current directory to the cloned repository's directory:
cd tic-tac-toe

##Run the Program: Execute the tic_tac_toe.py file using a Python 3 interpreter:
python3 tic_tac_toe.py

##How to use this program:
Enter the row and column numbers (1-3) to place your symbol (X or O) on the board.
Continue taking turns with your opponent until one player wins or the game ends in a draw.

###Body\Analysis

##Object oriented Programming (OOP) pillars

#Encapsulation
Each segment encapsulates a specific aspect of the game: the board, the players, and the game itself. This enhances modularity and readability, making it easier to understand and maintain the code.

For class Board:

    def print_board(self):
        print("\n  1   2   3")
        for i, row in enumerate(self.board):
            print(f"{i+1} {' | '.join(row)}")
            if i < 2:
                print(" ---|---|---")                

    def update_board(self, row, col, player):
        if self.board[row][col] == ' ':
            self.board[row][col] = player
            return True
        else:
            return False

    def check_winner(self, player):
        for row in self.board:
            if all(s == player for s in row):
                return True
        for col in range(3):
            if all(row[col] == player for row in self.board):
                return True
        if all(self.board[i][i] == player for i in range(3)) or all(self.board[i][2 - i] == player for i in range(3)):
            return True
        return False

    def is_full(self):
        return all(all(cell != ' ' for cell in row) for row in self.board)


For class player:


    def get_move(self):
        while True:
            try:
                row = int(input(f"Player {self.symbol}, enter the row (1-3): ")) - 1
                col = int(input(f"Player {self.symbol}, enter the column (1-3): ")) - 1
                if row in range(3) and col in range(3):
                    return row, col
                else:
                    print("Invalid input. Please enter numbers between 1 and 3.")
            except ValueError:
                print("Invalid input. Please enter numbers between 1 and 3.")

For class Game:



    def switch_player(self):
        self.current_player_idx = 1 - self.current_player_idx

    def play(self):
        while True:
            self.board.print_board()
            current_player = self.players[self.current_player_idx]
            row, col = current_player.get_move()
            
            if self.board.update_board(row, col, current_player.symbol):
                if self.board.check_winner(current_player.symbol):
                    self.board.print_board()
                    print(f"Player {current_player.symbol} wins!")
                    break
                elif self.board.is_full():
                    self.board.print_board()
                    print("The game is a draw!")
                    break
                
                self.switch_player()
            else:
                print("Cell is already occupied. Try again.")


Abstraction

Abstraction involves hiding the complex implementation details and showing only the essential features of an object. In the context of this code, the abstraction is mainly represented by the methods of the Board and Player classes, which provide a simplified interface for interacting with the game components.

For class board:

    def print_board(self):
        print("\n  1   2   3")
        for i, row in enumerate(self.board):
            print(f"{i+1} {' | '.join(row)}")
            if i < 2:
                print(" ---|---|---")

    def update_board(self, row, col, player):
        if self.board[row][col] == ' ':
            self.board[row][col] = player
            return True
        else:
            return False

    def check_winner(self, player):
        for row in self.board:
            if all(s == player for s in row):
                return True
        for col in range(3):
            if all(row[col] == player for row in self.board):
                return True
        if all(self.board[i][i] == player for i in range(3)) or all(self.board[i][2 - i] == player for i in range(3)):
            return True
        return False

    def is_full(self):
        return all(all(cell != ' ' for cell in row) for row in self.board)


For class player:

    def get_move(self):
        while True:
            try:
                row = int(input(f"Player {self.symbol}, enter the row (1-3): ")) - 1
                col = int(input(f"Player {self.symbol}, enter the column (1-3): ")) - 1
                if row in range(3) and col in range(3):
                    return row, col
                else:
                    print("Invalid input. Please enter numbers between 1 and 3.")
            except ValueError:
                print("Invalid input. Please enter numbers between 1 and 3.")

Polymorphism

Polymorphism allows objects of different classes to be treated as objects of a common superclass. In the context of this code, polymorphism is demonstrated through the Player class. Both Player objects are treated interchangeably in the Game class, as they both have the get_move method.

For class player:

    def get_move(self):
        while True:
            try:
                row = int(input(f"Player {self.symbol}, enter the row (1-3): ")) - 1
                col = int(input(f"Player {self.symbol}, enter the column (1-3): ")) - 1
                if row in range(3) and col in range(3):
                    return row, col
                else:
                    print("Invalid input. Please enter numbers between 1 and 3.")
            except ValueError:
                print("Invalid input. Please enter numbers between 1 and 3.")

Inheritance 

Inheritance is a key concept in object-oriented programming where a new class (subclass) is created by deriving properties and behaviors from an existing class (superclass).In this segmentation, the Player class serves as the superclass, and the ComputerPlayer class inherits from it.


For class player:

    def get_move(self):
        while True:
            try:
                row = int(input(f"Player {self.symbol}, enter the row (1-3): ")) - 1
                col = int(input(f"Player {self.symbol}, enter the column (1-3): ")) - 1
                if row in range(3) and col in range(3):
                    return row, col
                else:
                    print("Invalid input. Please enter numbers between 1 and 3.")
            except ValueError:
                print("Invalid input. Please enter numbers between 1 and 3.")

For class ComputerPlayer:




    def get_move(self):
        row = random.randint(0, 2)
        col = random.randint(0, 2)
        return row, col



Design patterns 

Factory method pattern

This implementation allows for the creation of different types of players based on the specified player type, adhering to the Factory Method pattern.

class PlayerFactory:

    def create_player(player_type, symbol):
        if player_type == "human":
            return Player(symbol)
        elif player_type == "computer":
            return ComputerPlayer(symbol)
        else:
            raise ValueError("Invalid player type")


The Template method

This structure allows for easy customization and extension of specific steps without altering the overall structure of the game loop, adhering to the Template Method pattern.

For class game:

    

    def play(self):
        while True:
            self.print_board()
            row, col = self.get_move()
            
            if self.update_board(row, col, self.players[self.current_player_idx].symbol):
                if self.check_winner(self.players[self.current_player_idx].symbol):
                    self.print_board()
                    print(f"Player {self.players[self.current_player_idx].symbol} wins!")
                    break
                elif self.is_full():
                    self.print_board()
                    print("The game is a draw!")
                    break
                
                self.switch_player()
            else:
                print("Cell is already occupied. Try again.")



File reading and writing

In the given application , the get_move function acts as a read function:

    def get_move(self):
        while True:
            try:
                row = int(input(f"Player {self.symbol}, enter the row (1-3): ")) - 1
                col = int(input(f"Player {self.symbol}, enter the column (1-3): ")) - 1
                if row in range(3) and col in range(3):
                    return row, col
                else:
                    print("Invalid input. Please enter numbers between 1 and 3.")
            except ValueError:
                print("Invalid input. Please enter numbers between 1 and 3.")

In the given application, update_board function acts as the write function:

    def update_board(self, row, col, player):
        if self.board[row][col] == ' ':
            self.board[row][col] = player
            return True
        else:
            return False



Results and summary

The code defines classes for a Tic-Tac-Toe game, including the board, players (human and computer), and a factory for player creation.

Players take turns making moves on the board, with human players inputting their moves through the console and computer players making random moves.

After each move, the game checks for a winner by examining rows, columns, and diagonals, ending the game if a winner is found or the board is full.

The main script initializes the game and runs a loop where players alternate turns until the game concludes, displaying the final board state and announcing the winner or a draw.


Conclusion

This code presents a concise implementation of Tic-Tac-Toe in Python, featuring human vs. computer or human vs. human gameplay options. It offers a clear structure with classes for the game board, players, and game management. Players can interact via the console, and the game logic efficiently checks for winners and draws.
