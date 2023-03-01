---
layout: post
title: "[최적화] Google OR-Tools Scheduling (1)Employee Scheduling"
subtitle: "Optimization"
categories: data
tags: opt
comments: true
---

Google OR-Tools Scheduling (1)Employee Scheduling 에 대한 간단한 정리

---
  
### 간호사 예약 문제
다음 예에서 병원 관리자는 다음 조건에 따라 3일 동안 4명의 간호사에 대한 일정을 만들어야 합니다.
  
- 각 날짜는 8시간 단위로 교대됩니다.
- 매일 각 교대근무는 한 명의 간호사에게 배정되고, 한 명의 간호사가 한번보다 많이 교대근무를 하지는 않는다.
- 각 간호사는 3일 동안 최소 두 번의 교대 근무에 배정됩니다.
  
### 변수만들기
교대근무 s가 d일차에 간호사 1명(n)에게 배정되는 경우 shifts[(n, d, s)]가 1이고, 그렇지 않으면 0  
```python
shifts = {}
for n in all_nurses:
    for d in all_days:ㅁ
        for s in all_shifts:
            shifts[(n, d,
                    s)] = model.NewBoolVar('shift_n%id%is%i' % (n, d, s))
```
  
### 교대근무에 간호사 배정
- 각 하나의 교대근무는 하루에 한 명의 간호사에게 할당
- 각 간호사는 하루 교대근무 최대 한번 근무가 가능합니다.
  
각 하나의 교대근무는 하루에 한 명의 간호사에게 할당
```python
for d in all_days:
    for s in all_shifts:
        model.AddExactlyOne(shifts[(n, d, s)] for n in all_nurses)
```
  
각 간호사는 하루 교대근무 최대 한번 근무가 가능
```python
for n in all_nurses:
    for d in all_days:
        model.AddAtMostOne(shifts[(n, d, s)] for s in all_shifts)
```
  
### 교대 근무조를 균등하게 할당
최대한 간호사에게 고르게 교대 근무를 할당함.  
3일동안 교대근무가 9회이므로 각 간호사 4명에게 교대 근무를 2개씩 할당. 그리고 남은 1개의 교대 근무는 어떤 간호사 1명에게 할당  
각 간호사에게 최소 2번의 이동을 할당. ```model.Add(min_shifts_per_nurse <= sum(num_shifts_worked))``` 제약조건으로 가능  
남은 1개의 추가 교대근무는 ```model.Add(sum(num_shifts_worked) <= max_shifts_per_nurse)```로 막으면 됨.  
  
```python
# Try to distribute the shifts evenly, so that each nurse works
# min_shifts_per_nurse shifts. If this is not possible, because the total
# number of shifts is not divisible by the number of nurses, some nurses will
# be assigned one more shift.
min_shifts_per_nurse = (num_shifts * num_days) // num_nurses
if num_shifts * num_days % num_nurses == 0:
    max_shifts_per_nurse = min_shifts_per_nurse
else:
    max_shifts_per_nurse = min_shifts_per_nurse + 1
for n in all_nurses:
    shifts_worked = []
    for d in all_days:
        for s in all_shifts:
            shifts_worked.append(shifts[(n, d, s)])
    model.Add(min_shifts_per_nurse <= sum(shifts_worked))
    model.Add(sum(shifts_worked) <= max_shifts_per_nurse)
```
  
### 솔버 매개변수 업데이트
```python
solver = cp_model.CpSolver()
solver.parameters.linearization_level = 0
# Enumerate all solutions.
solver.parameters.enumerate_all_solutions = True
```
  
### 솔루션 콜백
```python
class NursesPartialSolutionPrinter(cp_model.CpSolverSolutionCallback):
    """Print intermediate solutions."""

    def __init__(self, shifts, num_nurses, num_days, num_shifts, limit):
        cp_model.CpSolverSolutionCallback.__init__(self)
        self._shifts = shifts
        self._num_nurses = num_nurses
        self._num_days = num_days
        self._num_shifts = num_shifts
        self._solution_count = 0
        self._solution_limit = limit

    def on_solution_callback(self):
        self._solution_count += 1
        print('Solution %i' % self._solution_count)
        for d in range(self._num_days):
            print('Day %i' % d)
            for n in range(self._num_nurses):
                is_working = False
                for s in range(self._num_shifts):
                    if self.Value(self._shifts[(n, d, s)]):
                        is_working = True
                        print('  Nurse %i works shift %i' % (n, s))
                if not is_working:
                    print('  Nurse {} does not work'.format(n))
        if self._solution_count >= self._solution_limit:
            print('Stop search after %i solutions' % self._solution_limit)
            self.StopSearch()

    def solution_count(self):
        return self._solution_count

# Display the first five solutions.
solution_limit = 5
solution_printer = NursesPartialSolutionPrinter(shifts, num_nurses,
                                                num_days, num_shifts,
                                                solution_limit)
```
  
### 전체 코드
```python
"""Example of a simple nurse scheduling problem."""
from ortools.sat.python import cp_model


def main():
    # Data.
    num_nurses = 4
    num_shifts = 3
    num_days = 3
    all_nurses = range(num_nurses)
    all_shifts = range(num_shifts)
    all_days = range(num_days)

    # Creates the model.
    model = cp_model.CpModel()

    # Creates shift variables.
    # shifts[(n, d, s)]: nurse 'n' works shift 's' on day 'd'.
    shifts = {}
    for n in all_nurses:
        for d in all_days:
            for s in all_shifts:
                shifts[(n, d,
                        s)] = model.NewBoolVar('shift_n%id%is%i' % (n, d, s))

    # Each shift is assigned to exactly one nurse in the schedule period.
    for d in all_days:
        for s in all_shifts:
            model.AddExactlyOne(shifts[(n, d, s)] for n in all_nurses)

    # Each nurse works at most one shift per day.
    for n in all_nurses:
        for d in all_days:
            model.AddAtMostOne(shifts[(n, d, s)] for s in all_shifts)

    # Try to distribute the shifts evenly, so that each nurse works
    # min_shifts_per_nurse shifts. If this is not possible, because the total
    # number of shifts is not divisible by the number of nurses, some nurses will
    # be assigned one more shift.
    min_shifts_per_nurse = (num_shifts * num_days) // num_nurses
    if num_shifts * num_days % num_nurses == 0:
        max_shifts_per_nurse = min_shifts_per_nurse
    else:
        max_shifts_per_nurse = min_shifts_per_nurse + 1
    for n in all_nurses:
        shifts_worked = []
        for d in all_days:
            for s in all_shifts:
                shifts_worked.append(shifts[(n, d, s)])
        model.Add(min_shifts_per_nurse <= sum(shifts_worked))
        model.Add(sum(shifts_worked) <= max_shifts_per_nurse)

    # Creates the solver and solve.
    solver = cp_model.CpSolver()
    solver.parameters.linearization_level = 0
    # Enumerate all solutions.
    solver.parameters.enumerate_all_solutions = True


    class NursesPartialSolutionPrinter(cp_model.CpSolverSolutionCallback):
        """Print intermediate solutions."""

        def __init__(self, shifts, num_nurses, num_days, num_shifts, limit):
            cp_model.CpSolverSolutionCallback.__init__(self)
            self._shifts = shifts
            self._num_nurses = num_nurses
            self._num_days = num_days
            self._num_shifts = num_shifts
            self._solution_count = 0
            self._solution_limit = limit

        def on_solution_callback(self):
            self._solution_count += 1
            print('Solution %i' % self._solution_count)
            for d in range(self._num_days):
                print('Day %i' % d)
                for n in range(self._num_nurses):
                    is_working = False
                    for s in range(self._num_shifts):
                        if self.Value(self._shifts[(n, d, s)]):
                            is_working = True
                            print('  Nurse %i works shift %i' % (n, s))
                    if not is_working:
                        print('  Nurse {} does not work'.format(n))
            if self._solution_count >= self._solution_limit:
                print('Stop search after %i solutions' % self._solution_limit)
                self.StopSearch()

        def solution_count(self):
            return self._solution_count

    # Display the first five solutions.
    solution_limit = 5
    solution_printer = NursesPartialSolutionPrinter(shifts, num_nurses,
                                                    num_days, num_shifts,
                                                    solution_limit)

    solver.Solve(model, solution_printer)

    # Statistics.
    print('\nStatistics')
    print('  - conflicts      : %i' % solver.NumConflicts())
    print('  - branches       : %i' % solver.NumBranches())
    print('  - wall time      : %f s' % solver.WallTime())
    print('  - solutions found: %i' % solution_printer.solution_count())


if __name__ == '__main__':
    main()
```

---
[ORTOOLS 가이드](https://developers.google.com/optimization/scheduling/employee_scheduling?hl=ko)