# sudoku_solver.py
# Sudoku Solver using Backtracking Algorithm

SIZE = 9


def print_board(board):
    """Print the Sudoku board in a readable format."""
    for row in range(SIZE):
        if row % 3 == 0 and row != 0:
            print("-" * 21)

        for col in range(SIZE):
            if col % 3 == 0 and col != 0:
                print("|", end=" ")

            print(board[row][col], end=" ")

        print()


def is_valid(board, row, col, num):
    """Check whether num can be placed at board[row][col]."""

    # Check the row
    for x in range(SIZE):
        if board[row][x] == num:
            return False

    # Check the column
    for x in range(SIZE):
        if board[x][col] == num:
            return False

    # Check the 3x3 box
    start_row = row - row % 3
    start_col = col - col % 3

    for i in range(3):
        for j in range(3):
            if board[start_row + i][start_col + j] == num:
                return False

    return True


def find_empty_cell(board):
    """Find an empty cell. Empty cells are represented by 0."""
    for row in range(SIZE):
        for col in range(SIZE):
            if board[row][col] == 0:
                return row, col

    return None


def solve_sudoku(board):
    """Solve the Sudoku puzzle using backtracking."""

    empty_cell = find_empty_cell(board)

    # No empty cells means the puzzle is solved
    if empty_cell is None:
        return True

    row, col = empty_cell

    # Try numbers 1 through 9
    for num in range(1, 10):

        if is_valid(board, row, col, num):
            # Place the number
            board[row][col] = num

            # Recursively solve the remaining puzzle
            if solve_sudoku(board):
                return True

            # Backtrack if the choice was incorrect
            board[row][col] = 0

    return False


def validate_board(board):
    """Check that the input board has the correct format."""

    if len(board) != 9:
        return False

    for row in board:
        if len(row) != 9:
            return False

        for value in row:
            if not isinstance(value, int) or value < 0 or value > 9:
                return False

    return True


# Example Sudoku puzzle
# 0 represents an empty cell
board = [
    [5, 3, 0, 0, 7, 0, 0, 0, 0],
    [6, 0, 0, 1, 9, 5, 0, 0, 0],
    [0, 9, 8, 0, 0, 0, 0, 6, 0],

    [8, 0, 0, 0, 6, 0, 0, 0, 3],
    [4, 0, 0, 8, 0, 3, 0, 0, 1],
    [7, 0, 0, 0, 2, 0, 0, 0, 6],

    [0, 6, 0, 0, 0, 0, 2, 8, 0],
    [0, 0, 0, 4, 1, 9, 0, 0, 5],
    [0, 0, 0, 0, 8, 0, 0, 7, 9]
]


# Validate the board
if not validate_board(board):
    print("Invalid Sudoku board.")
else:
    print("Original Sudoku:")
    print_board(board)

    if solve_sudoku(board):
        print("\nSolved Sudoku:")
        print_board(board)
    else:
        print("\nNo solution exists for this Sudoku puzzle.")
