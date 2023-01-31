---
layout: post
title: "[최적화] Google OR-Tools Linear Optimization (4)The N-queens Problem"
subtitle: "Optimization"
categories: data
tags: opt
comments: true
---

Google OR-Tools Linear Optimization (4)The N-queens Problem 에 대한 간단한 정리

---
  
### N-Queens 문제
- 어떤 퀸이 NxN 체스판에 배치되어 두 사람이 서로 공격하지 않도록 할 수 있나요?(가로, 세로, 대각 공격)
  
---

## 파이썬 코드
  
### 솔루션 찾기 
```python
"""OR-Tools solution to the N-queens problem."""
import sys
import time
from ortools.sat.python import cp_model


class NQueenSolutionPrinter(cp_model.CpSolverSolutionCallback):
    """Print intermediate solutions."""

    def __init__(self, queens):
        cp_model.CpSolverSolutionCallback.__init__(self)
        self.__queens = queens
        self.__solution_count = 0
        self.__start_time = time.time()

    def solution_count(self):
        return self.__solution_count

    def on_solution_callback(self):
        current_time = time.time()
        print('Solution %i, time = %f s' %
              (self.__solution_count, current_time - self.__start_time))
        self.__solution_count += 1

        all_queens = range(len(self.__queens))
        for i in all_queens:
            for j in all_queens:
                if self.Value(self.__queens[j]) == i:
                    # There is a queen in column j, row i.
                    print('Q', end=' ')
                else:
                    print('_', end=' ')
            print()
        print()


def main(board_size):
    # Creates the solver.
    model = cp_model.CpModel()

    # Creates the variables.
    # The array index is the column, and the value is the row.
    queens = [
        model.NewIntVar(0, board_size - 1, 'x%i' % i) for i in range(board_size)
    ]

    # Creates the constraints.
    # All rows must be different.
    model.AddAllDifferent(queens)

    # All columns must be different because the indices of queens are all
    # different.

    # No two queens can be on the same diagonal.
    model.AddAllDifferent(queens[i] + i for i in range(board_size))
    model.AddAllDifferent(queens[i] - i for i in range(board_size))

    # Solve the model.
    solver = cp_model.CpSolver()
    solution_printer = NQueenSolutionPrinter(queens)
    solver.parameters.enumerate_all_solutions = True
    solver.Solve(model, solution_printer)

    # Statistics.
    print('\nStatistics')
    print(f'  conflicts      : {solver.NumConflicts()}')
    print(f'  branches       : {solver.NumBranches()}')
    print(f'  wall time      : {solver.WallTime()} s')
    print(f'  solutions found: {solution_printer.solution_count()}')


if __name__ == '__main__':
    # By default, solve the 8x8 problem.
    size = 4
    main(size)
```
  
### 결과값
```
Solution 0, time = 0.011988 s
_ _ Q _ 
Q _ _ _ 
_ _ _ Q 
_ Q _ _ 

Solution 1, time = 0.013119 s
_ Q _ _ 
_ _ _ Q 
Q _ _ _ 
_ _ Q _ 


Statistics
  conflicts      : 0
  branches       : 17
  wall time      : 0.013535707000000001 s
  solutions found: 2
```

---
참고 : https://developers.google.com/optimization/cp/queens