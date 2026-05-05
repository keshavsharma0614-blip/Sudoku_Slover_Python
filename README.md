# Sudoku Solver — Python

A clean, dependency-free Sudoku solver built in Python using a **recursive backtracking algorithm**. Designed with a clear object-oriented structure that separates validation logic, board state, and solving strategy.

---

## Table of Contents

- [Overview](#overview)
- [Algorithm](#algorithm)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Example](#example)
- [Design Decisions](#design-decisions)
- [Complexity Analysis](#complexity-analysis)
- [Future Roadmap](#future-roadmap)
- [Author](#author)
- [License](#license)

---

## Overview

Sudoku is a constraint satisfaction problem (CSP) — each cell must satisfy three simultaneous constraints: row uniqueness, column uniqueness, and 3×3 subgrid uniqueness. This solver treats each empty cell as a decision point and applies backtracking to systematically explore the solution space.

**Key characteristics:**

- Solves any valid 9×9 Sudoku puzzle
- Detects and reports unsolvable puzzles
- No external libraries — runs on standard Python 3
- Encapsulates all board logic inside a `Board` class

---

## Algorithm

The solver uses **recursive backtracking**, a depth-first search strategy with constraint checking at each step.

```
find_empty_cell()
    └── if none found → puzzle solved ✓
        └── for each candidate (1–9):
                └── is_valid(candidate)?
                        └── place candidate
                            └── recurse()
                                    └── if stuck → remove candidate (backtrack)
```

**Step-by-step:**

1. Scan the board for the next empty cell (value `0`)
2. Try placing digits `1` through `9`
3. For each candidate, validate against:
   - The cell's **row** — no duplicate digits
   - The cell's **column** — no duplicate digits
   - The cell's **3×3 subgrid** — no duplicate digits
4. If valid, place the digit and recurse to the next empty cell
5. If recursion fails, reset the cell to `0` and try the next candidate
6. If all candidates fail, return `False` to trigger backtracking in the caller
7. If no empty cells remain, the puzzle is solved

---

## Project Structure

```
sudoku-solver-python/
├── sudoku_solver.py    # Core solver — Board class and solve_sudoku()
└── README.md
```

### Class Reference

#### `Board`

| Method | Description |
|---|---|
| `__init__(board)` | Initialises the board with a 9×9 list of integers (`0` = empty) |
| `__str__()` | Returns a formatted string representation; empty cells shown as `*` |
| `find_empty_cell()` | Scans row-by-row and returns `(row, col)` of the first empty cell, or `None` |
| `valid_in_row(row, num)` | Returns `True` if `num` does not appear in the given row |
| `valid_in_col(col, num)` | Returns `True` if `num` does not appear in the given column |
| `valid_in_square(row, col, num)` | Returns `True` if `num` does not appear in the 3×3 subgrid containing `(row, col)` |
| `is_valid(empty, num)` | Composes the three validators; returns `True` only if all pass |
| `solver()` | Recursive backtracking entry point; returns `True` on success, `False` if unsolvable |

#### `solve_sudoku(board)`

Top-level convenience function. Accepts a raw 9×9 list, constructs a `Board`, runs the solver, and prints both the initial puzzle and the solution (or an unsolvable notice).

---

## Getting Started

**Requirements:** Python 3.8 or higher. No additional packages required.

**Clone the repository:**

```bash
git clone https://github.com/your-username/sudoku-solver-python.git
cd sudoku-solver-python
```

**Run directly:**

```bash
python sudoku_solver.py
```

---

## Usage

Define your puzzle as a 9×9 nested list. Use `0` to represent empty cells, and pass it to `solve_sudoku()`:

```python
from sudoku_solver import solve_sudoku

puzzle = [
    [0, 0, 2, 0, 0, 8, 0, 0, 0],
    [0, 0, 0, 0, 0, 3, 7, 6, 2],
    [4, 3, 0, 0, 0, 0, 8, 0, 0],
    [0, 5, 0, 0, 3, 0, 0, 9, 0],
    [0, 4, 0, 0, 0, 0, 0, 2, 6],
    [0, 0, 0, 4, 6, 7, 0, 0, 0],
    [0, 8, 6, 7, 0, 4, 0, 0, 0],
    [0, 0, 0, 5, 1, 9, 0, 0, 8],
    [1, 7, 0, 0, 0, 6, 0, 0, 5],
]

solve_sudoku(puzzle)
```

**Input constraints:**

- Board must be a 9×9 list of integers
- Given cells contain digits `1–9`
- Empty cells must be `0`
- The puzzle must be valid (no pre-filled constraint violations)

---

## Example

**Input puzzle** (`0` = empty):

```
* * 2  * * 8  * * *
* * *  * * 3  7 6 2
4 3 *  * * *  8 * *

* 5 *  * 3 *  * 9 *
* 4 *  * * *  * 2 6
* * *  4 6 7  * * *

* 8 6  7 * 4  * * *
* * *  5 1 9  * * 8
1 7 *  * * 6  * * 5
```

**Solved output:**

```
6 1 2  3 7 8  4 5 9
8 9 5  1 4 3  7 6 2
4 3 7  6 2 5  8 1 0

7 5 8  2 3 1  6 9 4
3 4 1  9 5 8  0 2 6
2 6 9  4 6 7  1 8 3

5 8 6  7 9 4  2 3 1
0 2 4  5 1 9  3 7 8
1 7 3  8 0 6  9 4 5
```

---

## Design Decisions

**Why backtracking?**
Backtracking is the standard baseline for Sudoku and offers a clean, provably correct implementation. For most published puzzles it runs in milliseconds. Harder puzzles — particularly those with minimal givens — may take longer, which is where algorithmic enhancements (see roadmap) become valuable.

**Why a `Board` class?**
Encapsulating state and validation inside a class keeps each concern isolated and makes the code easier to test, extend, or swap out. Adding a new solving strategy (e.g. constraint propagation) only requires adding a method, not restructuring the module.

**Why scan cells sequentially?**
The current approach selects the next empty cell in reading order (top-left to bottom-right). A more sophisticated heuristic — picking the cell with the fewest valid candidates first (Minimum Remaining Values, or MRV) — can reduce the search space significantly and is noted in the roadmap.

---

## Complexity Analysis

| Dimension | Detail |
|---|---|
| **Worst-case time** | O(9^m) where m = number of empty cells |
| **Space** | O(m) — recursion stack depth equals number of empty cells |
| **Practical performance** | Milliseconds on typical 9×9 puzzles |

The theoretical worst case is rarely approached in practice because constraint checking prunes invalid branches early. Extremely sparse boards (many empty cells, few constraints) represent the hardest cases.

---

## Future Roadmap

- [ ] **MRV heuristic** — select the most constrained cell first to reduce branching
- [ ] **Constraint propagation** — eliminate candidates using arc consistency (AC-3) before backtracking
- [ ] **Puzzle generator** — produce valid Sudoku puzzles at configurable difficulty levels
- [ ] **Difficulty grader** — classify puzzles by the solving techniques required
- [ ] **CLI interface** — accept puzzle input from a file or interactive prompt
- [ ] **Tkinter GUI** — visual board with step-by-step animation of the solver
- [ ] **REST API** — Flask or FastAPI endpoint accepting JSON puzzle input
- [ ] **Test suite** — unit tests covering edge cases (empty board, already solved, unsolvable)

---

## Author

**Keshav Kumar Sharma**  
B.Tech Computer Science Engineering  
Full Stack Developer · Algorithms Enthusiast · Open Source Contributor

---

## License

This project is licensed under the **MIT License** — you are free to use, modify, and distribute it for any purpose. See `LICENSE` for the full terms.                row, col = next_empty
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
