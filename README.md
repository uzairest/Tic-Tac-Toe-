# Tic-Tac-Toe-
Tic Tac Toe (OOP in C++) — Description Overview Tic Tac Toe is a two-player game played on a 3×3 grid. One player uses X and the other uses O. Players take turns placing their symbol in an empty cell. The player who first places three of their symbols in a row, column, or diagonal wins the game. If all cells are filled and no player wins,
CODE
#include <iostream>
#include <vector>
#include <string>

using namespace std; // Use standard namespace

// Player class to manage player details.
class Player {
public:
    string name;
    char symbol;

    Player(string playerName, char playerSymbol)
        : name(playerName), symbol(playerSymbol) {}
};

// Board class to represent the Tic-Tac-Toe board.
class Board {
private:
    vector<vector<char>> board;
public:
    Board() : board(3, vector<char>(3, ' ')) {}

    // Display the current state of the board.
    void display() {
        cout << "  0 1 2\n";  // Column headers
        for (int i = 0; i < 3; ++i) {
            cout << i << ' ';  // Row header
            for (int j = 0; j < 3; ++j) {
                cout << board[i][j] << ' ';
            }
            cout << '\n';
        }
    }

    // Place a move on the board.
    bool placeMove(int row, int col, char symbol) {
        if (row < 0 || row >= 3 || col < 0 || col >= 3 || board[row][col] != ' ')
            return false;
        board[row][col] = symbol;
        return true;
    }

    // Check for win condition.
    bool checkWin(char symbol) {
        for (int i = 0; i < 3; ++i) {
            // Check rows and columns.
            if ((board[i][0] == symbol && board[i][1] == symbol && board[i][2] == symbol) ||
                (board[0][i] == symbol && board[1][i] == symbol && board[2][i] == symbol))
                return true;
        }
        // Check diagonals.
        return (board[0][0] == symbol && board[1][1] == symbol && board[2][2] == symbol) ||
               (board[0][2] == symbol && board[1][1] == symbol && board[2][0] == symbol);
    }

    // Check for tie condition.
    bool checkTie() {
        for (const auto& row : board) {
            for (char cell : row) {
                if (cell == ' ')
                    return false;
            }
        }
        return true;
    }

    // Reset the board.
    void reset() {
        for (auto& row : board) {
            fill(row.begin(), row.end(), ' ');
        }
    }
};

// Game class to manage the game flow.
class Game {
private:
    Board board;
    Player player1;
    Player player2;
    Player* currentPlayer;

public:
    Game(string name1, string name2) 
        : player1(name1, 'X'), player2(name2, 'O'), currentPlayer(&player1) {}

    // Start the game.
    void start() {
        while (true) {
            board.display();
            int row, col;

            // Get a valid move from the player.
            while (true) {
                cout << currentPlayer->name << "'s turn (" << currentPlayer->symbol << "). ";
                cout << "Enter row (0-2): ";
                cin >> row;
                cout << "Enter column (0-2): ";
                cin >> col;

                if (board.placeMove(row, col, currentPlayer->symbol)) break;
                cout << "Invalid move. Try again.\n";
            }

            // Check for win or tie.
            if (board.checkWin(currentPlayer->symbol)) {
                board.display();
                cout << currentPlayer->name << " wins!\n";
                break;
            }
            if (board.checkTie()) {
                board.display();
                cout << "It's a tie!\n";
                break;
            }

            // Switch players.
            currentPlayer = (currentPlayer == &player1) ? &player2 : &player1;
        }
        // Offer to play again.
        char choice;
        cout << "Do you want to play again? (y/n): ";
        cin >> choice;
        if (choice == 'y' || choice == 'Y') {
            board.reset();
            start();
        }
    }
};

int main() {
    string player1Name, player2Name;
    cout << "Enter player 1 name: ";
    getline(cin, player1Name);
    cout << "Enter player 2 name: ";
    getline(cin, player2Name);

    Game game(player1Name, player2Name);
    game.start();

    return 0;
}
