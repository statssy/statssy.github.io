---
layout: post
title: "[최적화] Google OR-Tools Integer Optimization (1)Solving a MIP Problem"
subtitle: "Optimization"
categories: data
tags: opt
comments: true
---

Google OR-Tools Integer Optimization (1)Solving a MIP Problem 에 대한 간단한 정리

---
  
### MIP 문제 해결
- 제약 조건이 선형 + 답이 정수여야하는 선형 최적화 문제라고 보면된다.
- 만약에 MIP문제를 LP문제로 바꾸려면 pywraplp 솔버에서 SCIP->GLOP으로 바꿔주면된다.
  
- 문제
    - ```x + 10```를 최대화
    - ```x + 7y <= 17.5```
    - ```0 <= x <= 3.5```
    - ```0 <= y```
    - ```x```, ```y```는 정수
  
- MIP 문제 해결 기본 순서
    - MIP 솔버를 선언하고
    - 변수를 정의하고
    - 제약 조건을 정의
    - 목표를 정의
    - MIP 솔버를 호출
    - 솔루션 print

## 파이썬 코드
```python
from ortools.linear_solver import pywraplp


def main():
    # Create the mip solver with the SCIP backend.
    solver = pywraplp.Solver.CreateSolver('SCIP')
    if not solver:
        return

    infinity = solver.infinity()
    # x and y are integer non-negative variables.
    x = solver.IntVar(0.0, infinity, 'x')
    y = solver.IntVar(0.0, infinity, 'y')

    print('Number of variables =', solver.NumVariables())

    # x + 7 * y <= 17.5.
    solver.Add(x + 7 * y <= 17.5)

    # x <= 3.5.
    solver.Add(x <= 3.5)

    print('Number of constraints =', solver.NumConstraints())

    # Maximize x + 10 * y.
    solver.Maximize(x + 10 * y)

    status = solver.Solve()

    if status == pywraplp.Solver.OPTIMAL:
        print('Solution:')
        print('Objective value =', solver.Objective().Value())
        print('x =', x.solution_value())
        print('y =', y.solution_value())
    else:
        print('The problem does not have an optimal solution.')

    print('\nAdvanced usage:')
    print('Problem solved in %f milliseconds' % solver.wall_time())
    print('Problem solved in %d iterations' % solver.iterations())
    print('Problem solved in %d branch-and-bound nodes' % solver.nodes())


if __name__ == '__main__':
    main()
```



---
참고 : https://developers.google.com/optimization/mip/mip_example?hl=ko