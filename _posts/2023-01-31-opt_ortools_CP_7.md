---
layout: post
title: "[최적화] Google OR-Tools Integer Optimization (2)Using Arrays to Define a Model"
subtitle: "Optimization"
categories: data
tags: opt
comments: true
---

Google OR-Tools Integer Optimization (2)Using Arrays to Define a Model에 대한 간단한 정리

---
  
### 배열을 사용하여 모델 정의
- 제약조건 너무 복잡시러우면 배열 이용해서 풀기
- 루프로 정의 싹다 해버리기
  
```
5 x1 + 7 x2 + 9 x3 + 2 x4 + 1 x5 ≤ 250
19 x1 + 4 x2 - 9 x3 + 10 x4 + 12 x5 ≤ 285
4 x1 + 7 x2 + 3 x3 + 8 x4 + 5 x5 ≤ 211
5 x1 + 13 x2 + 16 x3 + 3 x4 - 7 x5 ≤ 315
```
  
## 예시
```python
def create_data_model():
    """Stores the data for the problem."""
    data = {}
    data['constraint_coeffs'] = [
        [5, 7, 9, 2, 1],
        [18, 4, -9, 10, 12],
        [4, 7, 3, 8, 5],
        [5, 13, 16, 3, -7],
    ]
    data['bounds'] = [250, 285, 211, 315]
    data['obj_coeffs'] = [7, 8, 2, 9, 6]
    data['num_vars'] = 5
    data['num_constraints'] = 4
    return data
```

## 파이썬 코드
```python
from ortools.linear_solver import pywraplp


def create_data_model():
    """Stores the data for the problem."""
    data = {}
    data['constraint_coeffs'] = [
        [5, 7, 9, 2, 1],
        [18, 4, -9, 10, 12],
        [4, 7, 3, 8, 5],
        [5, 13, 16, 3, -7],
    ]
    data['bounds'] = [250, 285, 211, 315]
    data['obj_coeffs'] = [7, 8, 2, 9, 6]
    data['num_vars'] = 5
    data['num_constraints'] = 4
    return data



def main():
    data = create_data_model()
    # Create the mip solver with the SCIP backend.
    solver = pywraplp.Solver.CreateSolver('SCIP')
    if not solver:
        return

    infinity = solver.infinity()
    x = {}
    for j in range(data['num_vars']):
        x[j] = solver.IntVar(0, infinity, 'x[%i]' % j)
    print('Number of variables =', solver.NumVariables())

    for i in range(data['num_constraints']):
        constraint = solver.RowConstraint(0, data['bounds'][i], '')
        for j in range(data['num_vars']):
            constraint.SetCoefficient(x[j], data['constraint_coeffs'][i][j])
    print('Number of constraints =', solver.NumConstraints())
    # In Python, you can also set the constraints as follows.
    # for i in range(data['num_constraints']):
    #  constraint_expr = \
    # [data['constraint_coeffs'][i][j] * x[j] for j in range(data['num_vars'])]
    #  solver.Add(sum(constraint_expr) <= data['bounds'][i])

    objective = solver.Objective()
    for j in range(data['num_vars']):
        objective.SetCoefficient(x[j], data['obj_coeffs'][j])
    objective.SetMaximization()
    # In Python, you can also set the objective as follows.
    # obj_expr = [data['obj_coeffs'][j] * x[j] for j in range(data['num_vars'])]
    # solver.Maximize(solver.Sum(obj_expr))

    status = solver.Solve()

    if status == pywraplp.Solver.OPTIMAL:
        print('Objective value =', solver.Objective().Value())
        for j in range(data['num_vars']):
            print(x[j].name(), ' = ', x[j].solution_value())
        print()
        print('Problem solved in %f milliseconds' % solver.wall_time())
        print('Problem solved in %d iterations' % solver.iterations())
        print('Problem solved in %d branch-and-bound nodes' % solver.nodes())
    else:
        print('The problem does not have an optimal solution.')


if __name__ == '__main__':
    main()
```

---
참고 : https://developers.google.com/optimization/mip/mip_var_array?hl=ko