🧩 Sudoku Solver – Python (Backtracking Algorithm)

This repository contains a simple and efficient Sudoku Solver implemented in Python using the backtracking algorithm.
The project is fully contained in a single Python file, making it easy to understand, modify, and extend.

📌 Features

✔️ Solves any valid 9×9 Sudoku puzzle

✔️ Uses recursive backtracking

✔️ Clean object-oriented implementation (Board class)

✔️ Detects if a puzzle is unsolvable

✔️ Professional, readable console output

📄 File Included
Sudoku_Solver_Python.py


This single file contains:

Board class

Sudoku validation methods

Backtracking solver

Example puzzle

Automatic solving and printed output

No additional files or dependencies required.

▶️ How to Run

Simply run the Python file:

python "Sudoku Solver Python.py"


Make sure the filename matches exactly.

🧠 How the Solver Works

The algorithm:

Finds the next empty cell (marked as 0)

Tries numbers 1 through 9

Checks:

Row validity

Column validity

3×3 subgrid validity

Places a number if valid

Recursively continues

Backtracks when stuck

Finishes when no empty cells remain

This ensures a correct solution whenever one exists.

🧪 Example Puzzle

The script includes this puzzle by default:

0 0 2 0 0 8 0 0 0
0 0 0 0 0 3 7 6 2
4 3 0 0 0 0 8 0 0
0 5 0 0 3 0 0 9 0
0 4 0 0 0 0 0 2 6
0 0 0 4 6 7 0 0 0
0 8 6 7 0 4 0 0 0
0 0 0 5 1 9 0 0 8
1 7 0 0 0 6 0 0 5


The solver prints the solved Sudoku grid.

📄 Code:
   class Board:
    def __init__(self, board):
        self.board = board

    def __str__(self):
        board_str = ''
        for row in self.board:
            row_str = [str(i) if i else '*' for i in row]
            board_str += ' '.join(row_str)
            board_str += '\n'
        return board_str

    def find_empty_cell(self):
        for row, contents in enumerate(self.board):
            try:
                col = contents.index(0)
                return row, col
            except ValueError:
                pass
        return None

    def valid_in_row(self, row, num):
        return num not in self.board[row]

    def valid_in_col(self, col, num):
        return all(self.board[row][col] != num for row in range(9))

    def valid_in_square(self, row, col, num):
        row_start = (row // 3) * 3
        col_start = (col // 3) * 3
        for row_no in range(row_start, row_start + 3):
            for col_no in range(col_start, col_start + 3):
                if self.board[row_no][col_no] == num:
                    return False
        return True

    def is_valid(self, empty, num):
        row, col = empty
        valid_in_row = self.valid_in_row(row, num)
        valid_in_col = self.valid_in_col(col, num)
        valid_in_square = self.valid_in_square(row, col, num)
        return all([valid_in_row, valid_in_col, valid_in_square])

    def solver(self):
        if (next_empty := self.find_empty_cell()) is None:
            return True
        for guess in range(1, 10):
            if self.is_valid(next_empty, guess):
                row, col = next_empty
                self.board[row][col] = guess
                if self.solver():
                    return True
                self.board[row][col] = 0
        return False

def solve_sudoku(board):
    gameboard = Board(board)
    print(f'Puzzle to solve:\n{gameboard}')
    if gameboard.solver():
        print(f'Solved puzzle:\n{gameboard}')
    else:
        print('The provided puzzle is unsolvable.')
    return gameboard

puzzle = [
  [0, 0, 2, 0, 0, 8, 0, 0, 0],
  [0, 0, 0, 0, 0, 3, 7, 6, 2],
  [4, 3, 0, 0, 0, 0, 8, 0, 0],
  [0, 5, 0, 0, 3, 0, 0, 9, 0],
  [0, 4, 0, 0, 0, 0, 0, 2, 6],
  [0, 0, 0, 4, 6, 7, 0, 0, 0],
  [0, 8, 6, 7, 0, 4, 0, 0, 0],
  [0, 0, 0, 5, 1, 9, 0, 0, 8],
  [1, 7, 0, 0, 0, 6, 0, 0, 5]
]
solve_sudoku(puzzle)


📦 Requirements

Python 3.x
(No external libraries needed.)

📘 Future Improvements (Optional)

GUI using Tkinter

Sudoku generator

Difficulty grading

API version using Flask or FastAPI

📝 License

MIT License — free to use, modify, and distribute.
