import numpy as np

# Constants for the board
ROW_COUNT = 6
COLUMN_COUNT = 7
EMPTY = 0
PLAYER_1 = 1
PLAYER_2 = 2

# Create an empty board
def create_board():
    board = np.zeros((ROW_COUNT, COLUMN_COUNT), dtype=int)
    return board

# Print the board
def print_board(board):
    print(np.flip(board, 0))

# Check if a column is valid to drop a disc
def is_valid_location(board, col):
    return board[ROW_COUNT - 1][col] == EMPTY

# Drop a disc in a column
def drop_piece(board, row, col, piece):
    board[row][col] = piece

# Get the next available row in the column
def get_next_open_row(board, col):
    for r in range(ROW_COUNT):
        if board[r][col] == EMPTY:
            return r

# Check if a player has won
def winning_move(board, piece):
    # Check horizontal locations
    for c in range(COLUMN_COUNT - 3):
        for r in range(ROW_COUNT):
            if board[r][c] == piece and board[r][c + 1] == piece and board[r][c + 2] == piece and board[r][c + 3] == piece:
                return True

    # Check vertical locations
    for c in range(COLUMN_COUNT):
        for r in range(ROW_COUNT =3):
            if board[r][c] == piece and board[r + 1][c] == piece and board[r + 2][c] == piece and board[r + 3][c]==piece:
                return True

    # Check negatively sloped diagonals
    for c in range(COLUMN_COUNT - 3):
        for r in range(3, ROW_COUNT):
            if board[r][c] == piece and board[r - 1][c + 1] == piece and board[r - 2][c + 2] == piece and board[r - 3][c + 3] == piece:
                return True

# Main game loop
def play_game():
    board = create_board()
    print_board(board)
    
    game_over = False
    turn = 0
    
    while not game_over:
        if turn % 2 == 0:
            player = PLAYER_1
            print("Player 1's turn (X)")
        else:
            player = PLAYER_2
            print("Player 2's turn (O)")

        valid_input = False
        while not valid_input:
            try:
                col = int(input(f"Player {player}, select a column (0-{COLUMN_COUNT - 1}): "))
                if 0 <= col < COLUMN_COUNT and is_valid_location(board, col):
                    valid_input = True
                else:
                    print("Column is full or invalid. Try again.")
            except ValueError:
                print("Invalid input. Please enter a valid column number.")
        
        row = get_next_open_row(board, col)
        drop_piece(board, row, col, player)
        
        print_board(board)

        if winning_move(board, player):
            print(f"Player {player} wins!")
            game_over = True
        else:
            turn += 1

# Run the game
if __name__ == "__main__":
    play_game()
