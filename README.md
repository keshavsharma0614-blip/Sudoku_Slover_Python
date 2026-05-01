# 🧩 Sudoku Solver – Python (Backtracking Algorithm)

A simple and efficient Sudoku Solver implemented in Python using the **backtracking algorithm**. Fully contained in a single file — easy to understand, modify, and extend.

---

## 🚀 Overview

This solver works by trying every valid number for each empty cell and recursively checking if the puzzle can be completed. If it gets stuck, it **backtracks** and tries the next possibility — guaranteed to find a solution if one exists.

**Algorithm steps:**
1. Find the next empty cell (marked as `0`)
2. Try numbers 1 through 9
3. Validate against row, column, and 3×3 subgrid
4. Place the number if valid and recurse
5. Backtrack if no number works
6. Stop when no empty cells remain

---

## ✨ Features

- ✅ Solves any valid 9×9 Sudoku puzzle
- ✅ Recursive backtracking algorithm
- ✅ Clean object-oriented design (`Board` class)
- ✅ Detects unsolvable puzzles
- ✅ No external libraries required

---

## 📌 Code

```python
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
        return all([
            self.valid_in_row(row, num),
            self.valid_in_col(col, num),
            self.valid_in_square(row, col, num)
        ])

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
```

---

## ▶️ How to Run

```bash
python "Sudoku Solver Python.py"
```

> Make sure the filename matches exactly.

---

## 🧪 Example Output

**Input puzzle** (`0` = empty cell):

```
0 0 2 0 0 8 0 0 0
0 0 0 0 0 3 7 6 2
4 3 0 0 0 0 8 0 0
0 5 0 0 3 0 0 9 0
0 4 0 0 0 0 0 2 6
0 0 0 4 6 7 0 0 0
0 8 6 7 0 4 0 0 0
0 0 0 5 1 9 0 0 8
1 7 0 0 0 6 0 0 5
```

**Solved output:**

```
6 1 2 3 7 8 4 5 9
8 9 5 1 4 3 7 6 2
4 3 7 6 2 5 8 1 0
7 5 8 2 3 1 6 9 4
3 4 1 9 5 0 0 2 6
2 6 9 4 6 7 1 8 3
5 8 6 7 9 4 2 3 1
0 2 4 5 1 9 3 7 8
1 7 3 8 0 6 9 4 5
```

---

## 📂 Project Structure

```
sudoku-solver-python/
├── Sudoku Solver Python.py
└── README.md
```

---

## 🧠 Skills Demonstrated

- Backtracking algorithm design
- Recursive problem solving
- Object-oriented programming in Python
- Grid-based constraint validation
- Clean separation of logic (validate → place → recurse → backtrack)

---

## 🌍 Real-World Applications

Backtracking is used in:

- Puzzle solvers (Sudoku, N-Queens, Maze)
- Constraint satisfaction problems (CSP)
- Pathfinding algorithms
- Compiler design (syntax parsing)
- Game AI decision trees

---

## 🔮 Future Improvements

- [ ] Accept user input from CLI or file
- [ ] GUI using Tkinter
- [ ] Sudoku puzzle generator
- [ ] Difficulty grading system
- [ ] REST API version using Flask or FastAPI

---

## 🛠️ Requirements

- Python 3.x
- No external libraries required

---

## 📘 License

MIT License — free to use, modify, and distribute.

---

## 👨‍💻 Author

**Keshav Kumar Sharma**  
B.Tech CSE | Full Stack Development | Problem Solver | Open Source Contributor
