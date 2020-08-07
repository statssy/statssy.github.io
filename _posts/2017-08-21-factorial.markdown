---
layout: post
title:  "[파이썬]팩토리얼(Factorial) 계산기 만들기"
subtitle:   "Python"
categories: dev
tags: python
comments: true
---
일단 [파이썬 강좌_CS50_K-MOOC(가천대)](https://github.com/TeamLab/Gachon_CS50_Python_KMOOC/blob/0c722362b2c6dbce285acaf1b9f80be15c542d41/lab_assignment/lab_6/READMD.md) 의 과제 중 하나였던 팩토리얼 계산기를 만들어 보도록 하겠습니다.

모든 내용은 [파이썬 강좌_CS50_K-MOOC(가천대)](https://github.com/TeamLab/Gachon_CS50_Python_KMOOC/blob/0c722362b2c6dbce285acaf1b9f80be15c542d41/lab_assignment/lab_6/READMD.md) 링크를 모두 읽었다고 가정하고 써내려가보도록 하겠습니다.


## is_positive_number 함수 만들기

아래 코드를 보자.
```
def is_positive_number(integer_str_value):

    try:
        integer_str_value = int(integer_str_value) # 먼저 숫자형으로 바꾸고 난 후 처리해야 에러를 막을 수 있음
        if int(integer_str_value) == integer_str_value and integer_str_value > 0:
            return True
        else:
            return False
        # ==================================
    except ValueError:
        return False

# x = input("숫자 입력 : ") # 잘돌아가나 체크용
# print(is_positive_number(x))
```
try, except에 대해서는 몰라도 된다고 하지만 오류 예외 처리 기법으로 알면 될거같다. 더 자세히 알고 싶다면 [점프투파이썬](https://wikidocs.net/30)을 참조 하도록 하자.
플로우는 int로 변환했을때 그대로 숫자형이 나오고 0보다 크다면 True를, 나머지는 다 Fasle로 하라는 것이다.


## get_factorial_value 함수 작성하기

아래 코딩을 보도록 하자.
```
def get_factorial_value(integer_value):
    factorial = 1
    for i in range(1, integer_value+1):
        factorial = factorial * i
    return factorial

# x = int(input("숫자 입력 : "))
# print(get_factorial_value(x))
```
팩토리얼을 어떻게 구현할까 고민했다. 결국 factorial 변수를 하나 설정해서 for문으로 1부터 내가 정해준 숫자까지 곱해주는 것을 선택했다.


## main 함수 작성하기

이제 마지막인 메인 함수를 작성해보도록 하자.
```
def main():
    user_input=999
    while(user_input is not 0):
        user_input = input("Input a positive number : ")
        if is_positive_number(user_input) is True:
            user_input = int(user_input)
            real = get_factorial_value(user_input)
            print(real)
        elif is_positive_number(user_input) is False and user_input is '0':
            print("Thank you for using this program")
        else:
            print("Input again, Please")

if __name__ == "__main__":
    main()
```

딱히 어려운 것은 없었던 것 같다. 중요한 점은 이전에 만든 함수들을 main 함수에 넣어서 작성했다.
is_positive_number가 True이면 factorial 값을 나오게 하고 그게 아니라면 끝이나던지 다시한번더 기회를 주는 식으로 만들었다.

다음 시간에는 베이스볼 게임을 만들어 보려 한다. 잘 될진 모르겠지만 화이팅 하자. 아자
