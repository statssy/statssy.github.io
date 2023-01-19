---
layout: post
title: "[최적화] Google OR-Tools 제약조건 최적화 (2)Solving a CP Problem"
subtitle: "Optimization"
categories: data
tags: opt
comments: true
---

Google OR-Tools 제약조건 최적화 (2)Solving a CP Problem 에 대한 간단한 정리

---
  
### CP 문제 해결
- CP-SAT은 연산속드를 높히기 위해 정수를 사용해야한다. 그래서 모든 제약조건이 정수로 이루어져야함으로 정수화 시킨다.

### 목표함수 및 제약조건 
- 다음 제약조건에 따라 2x + 2y + 3z 최대화  
- x + 7⁄2y + 3⁄2z	≤ 25  
- 3x - 5y + 7z ≤ 45  
- 5x + 2y -6z	≤ 37  
- x, y, z	≥ 0  
- x, y, z 는 정수


---

## 파이썬 코드
  
### 솔루션 찾기 
```python
"""Simple solve."""
from ortools.sat.python import cp_model


def main():
    """Minimal CP-SAT example to showcase calling the solver."""
    # Creates the model.
    model = cp_model.CpModel()

    # Creates the variables.
    var_upper_bound = max(50, 45, 37)
    x = model.NewIntVar(0, var_upper_bound, 'x')
    y = model.NewIntVar(0, var_upper_bound, 'y')
    z = model.NewIntVar(0, var_upper_bound, 'z')

    # Creates the constraints.
    model.Add(2 * x + 7 * y + 3 * z <= 50)
    model.Add(3 * x - 5 * y + 7 * z <= 45)
    model.Add(5 * x + 2 * y - 6 * z <= 37)

    model.Maximize(2 * x + 2 * y + 3 * z)

    # Creates a solver and solves the model.
    solver = cp_model.CpSolver()
    status = solver.Solve(model)

    if status == cp_model.OPTIMAL or status == cp_model.FEASIBLE:
        print(f'Maximum of objective function: {solver.ObjectiveValue()}\n')
        print(f'x = {solver.Value(x)}')
        print(f'y = {solver.Value(y)}')
        print(f'z = {solver.Value(z)}')
    else:
        print('No solution found.')

    # Statistics.
    print('\nStatistics')
    print(f'  status   : {solver.StatusName(status)}')
    print(f'  conflicts: {solver.NumConflicts()}')
    print(f'  branches : {solver.NumBranches()}')
    print(f'  wall time: {solver.WallTime()} s')


if __name__ == '__main__':
    main()
```

---
참고 : https://developers.google.com/optimization/cp/cp_example