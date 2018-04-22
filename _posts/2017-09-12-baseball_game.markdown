---
layout: post
title:  "[파이썬]베이스볼 게임 만들기"
subtitle:   "PYTHON"
categories: pro
tags: python
comments: true
---
[파이썬 강좌_CS50_K-MOOC(가천대)](https://github.com/TeamLab/Gachon_CS50_Python_KMOOC/blob/master/lab_assignment/lab_7/READMD.md) 의 과제 중 하나였던 베이스볼 게임을 만들어 보도록 하겠습니다.


## 

랜덤한 3자리 정수를 만든다.
import random

```
def get_random_number():
    # Helper Function - 지우지 말 것
    # 100부터 999까지 수를 램덤하게 반환함
    return random.randrange(100, 1000)
```

정수인지를 체킹할때 사용할 함수를 만든다.
```
def is_digit(user_input_number):
    # '''
    # Input:
    #   - user_input_number : 문자열 값
    # Output:
    #   - user_input_number가 정수로 변환 가능할 경우는 True,
    #     그렇지 않을 경우는 False
    # Examples:
    #   >>> import baseball_game as bg
    #   >>> bg.is_digit("551")
    #   True
    #   >>> bg.is_digit("103943")
    #   True
    #   >>> bg.is_digit("472")
    #   True
    #   >>> bg.is_digit("1032.203")
    #   False
    #   >>> bg.is_digit("abc")
    #   False
    # '''
    # ===Modify codes below=============
    # 조건에 따라 변환되어야 할 결과를 result 변수에 할당

    result = user_input_number.isdigit()

    # ==================================
    return result
```

3자리 숫자이면 True를 출력한다.
```
def is_between_100_and_999(user_input_number):
    # '''
    # Input:
    #   - user_input_number : 문자열 값
    #                         입력된 값은 숫자형태의 문자열 값임이 보장된다.
    # Output:
    #   - user_input_number가 정수로 변환하여 100이상 1000미만일 경우 True,
    #     그렇지 않을 경우는 False
    # Examples:
    #   >>> import baseball_game as bg
    #   >>> bg.is_between_100_and_999("551")
    #   True
    #   >>> bg.is_between_100_and_999("103943")
    #   False
    #   >>> bg.is_between_100_and_999("472")
    #   True
    #   >>> bg.is_between_100_and_999("0")
    #   False
    # '''
    # ===Modify codes below=============
    # 조건에 따라 변환되어야 할 결과를 result 변수에 할당
    result = False
    if len(user_input_number) == 3:
        result = True


    # ==================================
    return result
```

중복되는 수가 있으면 True를 출력하게 한다.
```
def is_duplicated_number(three_digit):
    # '''
    # Input:
    #   - three_digit : 문자열로 된 세자리 양의 정수 값
    #                   문자열로 된 세자리 양의 정수값의 입력이 보장된다.
    # Output:
    #   - three_digit 정수로 변환하였을 경우 중복되는 수가 있으면 True,
    #     그렇지 않을 경우는 False
    #   ex) 117 - True, 123 - False, 103 - False, 113 - True
    # Examples:
    #   >>> import baseball_game as bg
    #   >>> bg.is_duplicated_number("551")
    #   True
    #   >>> bg.is_duplicated_number("402")
    #   False
    #   >>> bg.is_duplicated_number("472")
    #   False
    #   >>> bg.is_duplicated_number("100")
    #   True
    # '''
    # ===Modify codes below=============
    # 조건에 따라 변환되어야 할 결과를 result 변수에 할당

    result = False
    if three_digit[0] == three_digit[1] or three_digit[1] == three_digit[2] or three_digit[0] == three_digit[2]:
        result = True

    # ==================================
    return result
```

지금까지 했던 내용을 조합하여 1) 숫자형 문자열이며, 2) 100이상 1000미만이며, 3) 중복되는 숫자가 없을 경우 True를 출력해준다.
```
def is_validated_number(user_input_number):
    # '''
    # Input:
    #   - user_input_number : 문자열 값
    # Output:
    #   - user_input_number 값이 아래 조건이면 True, 그렇지 않으면 False를 반환
    #        1) 숫자형 문자열이며, 2) 100이상 1000미만이며, 3) 중복되는 숫자가 없을 경우
    # Examples:
    #   >>> import baseball_game as bg
    #   >>> bg.is_validated_number("amvd")
    #   False
    #   >>> bg.is_validated_number("402")
    #   True
    #   >>> bg.is_validated_number("472")
    #   True
    #   >>> bg.is_validated_number("100")
    #   False
    #   >>> bg.is_validated_number("1000")
    #   False
    # '''
    # ===Modify codes below=============
    # 조건에 따라 변환되어야 할 결과를 result 변수에 할당

    result = False
    if is_digit(user_input_number) is True and is_between_100_and_999(user_input_number) is True and is_duplicated_number(user_input_number) is False:
        result = True
    # ==================================
    return result
```


중복이 없는 3자리 정수를 랜덤하게 출력한다.
```
def get_not_duplicated_three_digit_number():
    # '''
    # Input:
    #   - None : 입력값이 없음
    # Output:
    #   - 중복되는 숫자가 없는 3자리 정수값을 램덤하게 생성하여 반환함
    #     정수값으로 문자열이 아님
    # Examples:
    #   >>> import baseball_game as bg
    #   >>> bg.get_not_duplicated_three_digit_number()
    #   125
    #   >>> bg.get_not_duplicated_three_digit_number()
    #   634
    #   >>> bg.get_not_duplicated_three_digit_number()
    #   583
    #   >>> bg.get_not_duplicated_three_digit_number()
    #   381
    # '''
    # ===Modify codes below=============
    # 조건에 따라 변환되어야 할 결과를 result 변수에 할당
    # get_random_number() 함수를 사용하여 random number 생성

    result = None

    while True:
        result = str(get_random_number())
        if is_duplicated_number(result) is False:
            result
            break
    # ==================================
    result = int(result)
    return result
```

읽어가면서 strike인 경우 하나씩 늘리고 ball인 경우 하나씩 늘린다.
```
def get_strikes_or_ball(user_input_number, random_number):
    # '''
    # Input:
    #   - user_input_number : 문자열값으로 사용자가 입력하는 세자리 정수
    #   - random_number : 문자열값으로 컴퓨터가 자동으로 생성된 숫자
    # Output:
    #   - [strikes, ball] : 규칙에 따라 정수형 값인 strikes와 ball이 반환됨
    #   변환 규칙은 아래와 같음
    #   - 사용자가 입력한 숫자와 컴퓨터가 생성한 숫자의
    #     한 숫자와 자릿수가 모두 일치하면 1 Strike
    #   - 자릿수는 다르나 입력한 한 숫자가 존재하면 1 Ball
    #   - 세자리 숫자를 정확히 입력하면 3 Strike
    # Examples:
    #   >>> import baseball_game as bg
    #   >>> bg.get_strikes_or_ball("123", "472")
    #   [0, 1]
    #   >>> bg.get_strikes_or_ball("547", "472")
    #   [0, 2]
    #   >>> bg.get_strikes_or_ball("247", "472")
    #   [0, 3]
    #   >>> bg.get_strikes_or_ball("742", "472")
    #   [1, 2]
    #   >>> bg.get_strikes_or_ball("472", "472")
    #   [3, 0]
    # '''
    # ===Modify codes below=============
    # 조건에 따라 변환되어야 할 결과를 result 변수에 할당

    strike = 0
    ball = 0
    for i in range(0, len(user_input_number)):
        if user_input_number[i] in random_number[i]:
            strike += 1
        elif user_input_number[i] in random_number:
            ball += 1
    result = [strike, ball]
    # ==================================
    return result
```


Yes의 다른 표현도 포함해준다.
```
def is_yes(one_more_input):
    # '''
    # Input:
    #   - one_more_input : 문자열값으로 사용자가 입력하는 문자
    # Output:
    #   - 입력한 값이 대소문자 구분없이 "Y" 또는 "YES"일 경우 True,
    #     그렇지 않을 경우 False를 반환함
    # Examples:
    #   >>> import baseball_game as bg
    # >>> bg.is_yes("Y")
    # True
    # >>> bg.is_yes("y")
    # True
    # >>> bg.is_yes("Yes")
    # True
    # >>> bg.is_yes("YES")
    # True
    # >>> bg.is_yes("abc")
    # False
    # >>> bg.is_yes("213")
    # False
    # >>> bg.is_yes("4562")
    # False
    # '''
    # ===Modify codes below=============
    # 조건에 따라 변환되어야 할 결과를 result 변수에 할당

    result = False
    upper_input = one_more_input.upper()
    if upper_input == "Y" or upper_input == "YES":
        result = True
    # ==================================
    return result
```

No의 다른 표현도 포함해준다.
```
def is_no(one_more_input):
    # '''
    # Input:
    #   - one_more_input : 문자열값으로 사용자가 입력하는 문자
    # Output:
    #   - 입력한 값이 대소문자 구분없이 "N" 또는 "NO"일 경우 True,
    #     그렇지 않을 경우 False를 반환함
    # Examples:
    #   >>> import baseball_game as bg
    # >>> bg.is_no("Y")
    # False
    # >>> bg.is_no("b")
    # False
    # >>> bg.is_no("n")
    # True
    # >>> bg.is_no("NO")
    # True
    # >>> bg.is_no("nO")
    # True
    # >>> bg.is_no("1234")
    # False
    # >>> bg.is_no("yes")
    # False
    # '''
    # ===Modify codes below=============
    # 조건에 따라 변환되어야 할 결과를 result 변수에 할당

    result = False
    one_more_input = one_more_input.upper()
    if one_more_input == "NO" or one_more_input == "N":
        result = True
    # ==================================
    return result
```

스트라이크가 3개 미만일때까지 돌아가는 while문을 짜준다. 그리고 게임이 끝났을때 다시 할지에 대한 반복문을 만들어준다. 여기서 가장 중요한 부분.
```
def main():
    print("Play Baseball")
    user_input = 999
    EndGame = False
    while EndGame is False:
        strikeAndBalls = [0, 0]
        random_number = str(get_not_duplicated_three_digit_number())
        print("Random Number is : ", random_number)
        # ===Modify codes below=============
        # 위의 코드를 포함하여 자유로운 수정이 가능함
        while strikeAndBalls[0] < 3: #스트라이크가 3개 미만일 때에만
            user_input = input("Input guess number : ")
            if user_input == "0":
                EndGame = True
                break
            elif is_validated_number(user_input):
                strikeAndBalls = get_strikes_or_ball(user_input, random_number)
                print("Strikes : ", strikeAndBalls[0], " , Balls : ", strikeAndBalls[1])
            else:
                print("Wrong Input, Input again")

        while EndGame is False:
            OneMore = input("You win, one more(Y/N) ?")
            if is_no(OneMore):
                EndGame = True
                break
            elif is_yes(OneMore):
                break
            else:
                print("Wrong Input, Input again")
    # ==================================
    print("Thank you for using this program")
    print("End of the Game")


if __name__ == "__main__":
    main()
```
