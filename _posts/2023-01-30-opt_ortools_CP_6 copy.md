---
layout: post
title: "[최적화] Google OR-Tools Linear Optimization (5)Setting solver limits"
subtitle: "Optimization"
categories: data
tags: opt
comments: true
---

Google OR-Tools Linear Optimization (6)Setting solver limitsm 에 대한 간단한 정리

---
  
### 시간 제약이나 솔루션 갯수 제한
- 시간 제약이나 솔루션 갯수 제한을 둘수 있음
  
---

## 파이썬 코드
  
### 시간 제한 거는법 
```python


"""Solve a probelem with a time limit."""

from ortools.sat.python import cp_model


def SolveWithTimeLimitSampleSat():
    """Minimal CP-SAT example to showcase calling the solver."""
    # Creates the model.
    model = cp_model.CpModel()
    # Creates the variables.
    num_vals = 3
    x = model.NewIntVar(0, num_vals - 1, 'x')
    y = model.NewIntVar(0, num_vals - 1, 'y')
    z = model.NewIntVar(0, num_vals - 1, 'z')
    # Adds an all-different constraint.
    model.Add(x != y)

    # Creates a solver and solves the model.
    solver = cp_model.CpSolver()

    # Sets a time limit of 10 seceonds.
    solver.parameters.max_time_in_seconds = 10

    status = solver.Solve(model)

    if status == cp_model.OPTIMAL:
        print('x = %i' % solver.Value(x))
        print('y = %i' % solver.Value(y))
        print('z = %i' % solver.Value(z))

SolveWithTimeLimitSampleSat()
```
  
### 지정된 솔루션의 갯수 만큼 찾은후, 종료
```python
"""Code sample that solves a model and displays a small number of solutions."""

from ortools.sat.python import cp_model


class VarArraySolutionPrintWithLimit(cp_model.CpSolverSolutionCallback):
    """Print intermediate solutions."""

    def __init__(self, variables, limit):
        cp_model.CpSolverSolutionCallback.__init__(self)
        self.__variables = variables
        self.__solution_count = 0
        self.__solution_limit = limit

    def on_solution_callback(self):
        self.__solution_count += 1
        for v in self.__variables:
            print('%s=%i' % (v, self.Value(v)), end=' ')
        print()
        if self.__solution_count >= self.__solution_limit:
            print('Stop search after %i solutions' % self.__solution_limit)
            self.StopSearch()

    def solution_count(self):
        return self.__solution_count

def StopAfterNSolutionsSampleSat():
    """Showcases calling the solver to search for small number of solutions."""
    # Creates the model.
    model = cp_model.CpModel()
    # Creates the variables.
    num_vals = 3
    x = model.NewIntVar(0, num_vals - 1, 'x')
    y = model.NewIntVar(0, num_vals - 1, 'y')
    z = model.NewIntVar(0, num_vals - 1, 'z')
    
    # Adds an all-different constraint.
    model.Add(x != y)

    # Create a solver and solve.
    solver = cp_model.CpSolver()
    solution_printer = VarArraySolutionPrintWithLimit([x, y, z], 5)
    # Enumerate all solutions.
    solver.parameters.enumerate_all_solutions = True
    # Solve.
    status = solver.Solve(model, solution_printer)
    print('Status = %s' % solver.StatusName(status))
    print('Number of solutions found: %i' % solution_printer.solution_count())
    assert solution_printer.solution_count() == 5

StopAfterNSolutionsSampleSat()
```
  
### 갯수제한 결과값
```
x=1 y=0 z=0 
x=2 y=0 z=0 
x=2 y=0 z=1 
x=1 y=0 z=1 
x=2 y=1 z=1 
Stop search after 5 solutions
Status = FEASIBLE
Number of solutions found: 5
```
---
참고 : https://developers.google.com/optimization/cp/cp_tasks