---
layout: post
title: "[최적화] Google ORTools로 생산계획 최적화 하기"
subtitle: "Optimization"
categories: data
tags: opt
comments: true
---

Google ORTools로 생산계획 최적화 하기 대한 간단한 코드

---
  
### 참고사항
- Ortools 가이드 보고 체크
[ORTOOLS 가이드](https://developers.google.com/optimization/mip/mip_example?hl=ko)
- 파이썬 데이터분석 실무 테크닉 100 책에서 나온 내용을 Google Ortools이용해서 코드 짜보기  
[파이썬 데이터 분석 실무 테크닉 100](http://www.yes24.com/Product/Goods/91302724)  
- 정리 잘 하신분 블로그가 있어서 참고
[정리 블로그](https://suy379.tistory.com/64)  
  
### 코드
```python
# 생산최적화 ortools로 바꾸기

import pandas as pd
import numpy as np
from itertools import product
from ortools.linear_solver import pywraplp

df_m = pd.DataFrame({'원료1':[1, 2],
                    '원료2':[4, 4],
                    '원료3':[3, 1]},
                    index = ['제품1', '제품2'])   #index 지정
df_p = pd.DataFrame({'이익':[5, 4]},
                    index = ['제품1', '제품2'])   #index 지정
df_s = pd.DataFrame({'원료1':[40],
                    '원료2':[80],
                    '원료3':[50]},
                    index = ['재고'])   #index 지정
df_plan = pd.DataFrame({'생산량':[16, 0]},
                    index = ['제품1', '제품2'])   #index 지정

def product_plan(df_profit, df_plan):
    profit = 0
    for i in range(len(df_profit.index)):
        profit += df_profit.iloc[i][0] * df_plan.iloc[i][0] #이익 * 생산량 
    return profit
    
print('이전 계획 총이익: ', product_plan(df_p, df_plan))

# Create the mip solver with the SCIP backend.
solver = pywraplp.Solver.CreateSolver('SCIP')
if not solver:
    print('Not Sovler')

infinity = solver.infinity()
v2 = {}
for i in range(len(df_p)):
    v2[i] = solver.IntVar(0, infinity, 'v%i' % i)
print('Number of variables =', solver.NumVariables())

objective = solver.Objective()
for i in range(len(df_p)):
    objective.SetCoefficient(v2[i], int(df_p.iloc[i]))
objective.SetMaximization()

for j in range(len(df_m.columns)): #j: 원료 0,1,2 / i: 제품 0, 1
    constraint = solver.Constraint(0, int(df_s.iloc[:, j]))
    for i in range(len(df_p)):
        constraint.SetCoefficient(v2[i], int(df_m.iloc[i, j]))

status = solver.Solve()
if status == pywraplp.Solver.OPTIMAL:
    print('Objective value =', int(solver.Objective().Value()))
    for i in range(len(v2)):
        print(v2[i].name(), ' = ', v2[i].solution_value())
    print()
    print('Problem solved in %f milliseconds' % solver.wall_time())
    print('Problem solved in %d iterations' % solver.iterations())
    print('Problem solved in %d branch-and-bound nodes' % solver.nodes())
else:
    print('The problem does not have an optimal solution.')
```