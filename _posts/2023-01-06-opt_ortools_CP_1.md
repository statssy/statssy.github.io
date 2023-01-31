---
layout: post
title: "[최적화] Google OR-Tools Linear Optimization (1)CP-SAT Solver"
subtitle: "Optimization"
categories: data
tags: opt
comments: true
---

Google OR-Tools Linear Optimization CP-SAT Solver 에 대한 간단한 정리

---
  
## 제약조건 최적화
- 제약조건 프로그래밍(CP)는 제약조건 및 변수를 중점으로 둔다.
- 제약조건을 추가하여 하위 집합으로 좁히는게 목표다.
- 경우의 수가 많은(직원 배분 문제)에서도 제약조건으로 가능 솔루션을 줄인다.
- 선형 제약조건 문제에서는 MPSolver를 쓰고, 라우팅 문제는 Routing Library를 쓰는게 가장 좋다.
  
### CP-SAT Solver를 이용하여 실행가능한 솔루션 찾기 예시
- 세 개의 변수, x, y, z는 각각 0, 1, 2 값을 가질수 있음
- 제약 조건 : x != y
  
## 순서
- 모델 정의 -> 변수 정의 -> 제약 조건 정의 -> OPtimal or Feasible한 값 찾기 -> Callback함수 만들어서 전체 솔루션 찾아서 Print
  
---

## 파이썬 코드
  
### 솔루션 찾기 
```python
# CP-SAT 솔버
"""Simple solve."""
from ortools.sat.python import cp_model


def SimpleSatProgram():
    """Minimal CP-SAT example to showcase calling the solver."""
    # 모델 만들기
    model = cp_model.CpModel()

    # 변수 값 들어갈수 있는거 지정
    num_vals = 3
    x = model.NewIntVar(0, num_vals - 1, 'x')
    y = model.NewIntVar(0, num_vals - 1, 'y')
    z = model.NewIntVar(0, num_vals - 1, 'z')

    # 제약조건 추가
    model.Add(x != y)

    # 솔버를 만들고, 솔버로 문제 풀기
    solver = cp_model.CpSolver()
    status = solver.Solve(model)

    # status가 Optimal이거나 Feasible 한 게 있다면
    if status == cp_model.OPTIMAL or status == cp_model.FEASIBLE:
        print('x = %i' % solver.Value(x))
        print('y = %i' % solver.Value(y))
        print('z = %i' % solver.Value(z))
    else:
        print('No solution found.')

SimpleSatProgram()
```
  
### Callback 추가
```python
# 모든 솔루션을 찾게 수정하기
# 솔버에 전달하는 콜백하는 솔루션 프린터를 추가하고, 솔루션이 발견될때마다 결과 표시
class VarArraySolutionPrinter(cp_model.CpSolverSolutionCallback):
    """Print intermediate solutions."""

    def __init__(self, variables):
        cp_model.CpSolverSolutionCallback.__init__(self)
        self.__variables = variables
        self.__solution_count = 0

    def on_solution_callback(self):
        self.__solution_count += 1
        for v in self.__variables:
            print('%s=%i' % (v, self.Value(v)), end=' ')
        print()

    def solution_count(self):
        return self.__solution_count

solver = cp_model.CpSolver()
solution_printer = VarArraySolutionPrinter([x, y, z])
# Enumerate all solutions.
solver.parameters.enumerate_all_solutions = True
# Solve.
status = solver.Solve(model, solution_printer)
```
  
### 코드 전문
```python
# 전체 다 합쳐서

from ortools.sat.python import cp_model


class VarArraySolutionPrinter(cp_model.CpSolverSolutionCallback):
    """Print intermediate solutions."""

    def __init__(self, variables):
        cp_model.CpSolverSolutionCallback.__init__(self)
        self.__variables = variables
        self.__solution_count = 0

    def on_solution_callback(self):
        self.__solution_count += 1
        for v in self.__variables:
            print('%s=%i' % (v, self.Value(v)), end=' ')
        print()

    def solution_count(self):
        return self.__solution_count


def SearchForAllSolutionsSampleSat():
    """Showcases calling the solver to search for all solutions."""
    # Creates the model.
    model = cp_model.CpModel()

    # Creates the variables.
    num_vals = 3
    x = model.NewIntVar(0, num_vals - 1, 'x')
    y = model.NewIntVar(0, num_vals - 1, 'y')
    z = model.NewIntVar(0, num_vals - 1, 'z')

    # Create the constraints.
    model.Add(x != y)

    # Create a solver and solve.
    solver = cp_model.CpSolver()
    solution_printer = VarArraySolutionPrinter([x, y, z])
    # Enumerate all solutions.
    solver.parameters.enumerate_all_solutions = True
    # Solve.
    status = solver.Solve(model, solution_printer)

    print('Status = %s' % solver.StatusName(status))
    print('Number of solutions found: %i' % solution_printer.solution_count())


SearchForAllSolutionsSampleSat()
```

---
참고 : https://developers.google.com/optimization/cp/cp_solver