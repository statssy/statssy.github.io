---
layout: post
title: "[최적화] Google ORTools로 운송비용 최적화 하기"
subtitle: "Optimization"
categories: data
tags: opt
comments: true
---

Google ORTools로 운송비용 최적화 하기 대한 간단한 코드

---
  
### 참고사항
- Ortools 가이드 보고 체크
[ORTOOLS 가이드](https://developers.google.com/optimization/mip/mip_example?hl=ko)
- 파이썬 데이터분석 실무 테크닉 100 책에서 나온 내용을 Google Ortools이용해서 코드 짜보기  
[파이썬 데이터 분석 실무 테크닉 100](http://www.yes24.com/Product/Goods/91302724)  
- 정리 잘 하신분 블로그가 있어서 참고
[정리 블로그](https://suy379.tistory.com/73)  
  
### 코드
```python
# 운송최적화 ortools로 바꾸기

import pandas as pd
import numpy as np
from itertools import product
from ortools.linear_solver import pywraplp

df_tc = pd.DataFrame({'F1':[10, 18, 15],
                    'F2':[10, 21, 12],
                    'F3':[11, 12, 14],
                    'F4':[27, 14, 12]},
                    index = ['W1', 'W2', 'W3'])   #index 지정
df_demand = pd.DataFrame({'F1':[28],
                        'F2':[29],
                        'F3':[31],
                        'F4':[25]}
                        ) 
df_supply = pd.DataFrame({'W1':[35],
                        'W2':[41],
                        'W3':[42]}
                        ) 
#초기 설정
np.random.seed(1)
nw = len(df_tc.index) #W 창고 개수 : 3
nf = len(df_tc.columns) #F 공장 개수 :4
pr = list(product(range(nw), range(nf))) #product 함수는 W(0~2)와 F(0~3)넘버를 중복 없이 짝지어줌 

# Create the mip solver with the SCIP backend.
solver = pywraplp.Solver.CreateSolver('SCIP')
if not solver:
    print('Not Sovler')

infinity = solver.infinity()
v1 = {}
for w, f in pr:
    v1[(w, f)] = solver.IntVar(0, infinity, 'x[w%if%i]' % (w, f))
print('Number of variables =', solver.NumVariables())


objective = solver.Objective()
for i, j in pr:
    objective.SetCoefficient(v1[i,j], int(df_tc.iloc[i][j]))
objective.SetMinimization()

for i in range(nw):
    constraint = solver.Constraint(0, int(df_supply.iloc[0][i]))
    for j in range(nf):
        constraint.SetCoefficient(v1[i, j], 1)

for j in range(nf):
    constraint = solver.Constraint(int(df_demand.iloc[0][j]), solver.infinity())
    for i in range(nw):
        constraint.SetCoefficient(v1[i, j], 1)

status = solver.Solve()

if status == pywraplp.Solver.OPTIMAL:
    print('Objective value =', solver.Objective().Value())
    for i,j in pr:
        print(v1[(i,j)].name(), ' = ', v1[(i,j)].solution_value())
    print()
    print('Problem solved in %f milliseconds' % solver.wall_time())
    print('Problem solved in %d iterations' % solver.iterations())
    print('Problem solved in %d branch-and-bound nodes' % solver.nodes())
else:
    print('The problem does not have an optimal solution.')
```