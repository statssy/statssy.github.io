---
layout: post
title:  "[파이썬] 클래스(class)에 관하여 쉽게 정리"
subtitle:   "PYTHON"
categories: pro
tags: python
comments: true
---

평소에 클래스에 대해 재정리를 하고 싶은 생각이 들어서 글을 씀.  
  
[참고 사이트](https://wikidocs.net/28#_1)  
  
**내 정리**
- 클래스는 과자 틀, 그걸로 인해 만들어진 과자는 객체 ( a= FourCal()에서 a는 객체 )
- 생성자는 객체에 초깃값을 넣어줄때 사용함( a=FourCals(4, 2) 이런식으로 씀 )
- 클래스 상속 : 이미 있는 클래스를 복사해서 고치기 위함
- 메서드 오버라이딩 : 그안에 있는 메서드(def)를 바꾸고 싶을때 ex)0으로 나누기
- 클래스 변수 : 독립적이지않다(객체변수는 독립), ( ex) Family.lastname = "박" 이렇게 클래스이름.클래스변수를 바꾸면 lastname이 다바뀜)

---

# 클래스
  

## 클래스는 왜 필요한가?
---
같은 함수를 반복적으로 쓰고 싶을때 쓴다. ex) add 함수의 결괏값으 유지하면서 add 할 때 의 경우
  

## 클래스와 객체
---
- 과자 틀 : 클래스(class)
- 과자 틀에 의해서 만들어진 과자 -> 객체(object)

## 클래스 만들기
  

```python
class FourCal:
    def setdata(self, first, second):
        self.first = first
        self.second = second
```


```python
a = FourCal()
a.setdata(4, 2)
```


```python
print(a.first) # a객체에 객체변수 first가 생성되었음을 알수 있다.
```

    4
    


```python
class FourCal:
     def setdata(self, first, second):
         self.first = first
         self.second = second
     def add(self):
         result = self.first + self.second
         return result
     def mul(self):
         result = self.first * self.second
         return result
     def sub(self):
         result = self.first - self.second
         return result
     def div(self):
         result = self.first / self.second
         return result

```


```python
a = FourCal()
b = FourCal()
a.setdata(4, 2)
b.setdata(3, 8)
```


```python
a.add()
```




    6




```python
b.add()
```




    11



## 생성자
---


```python
a = FourCal()
a.add()
# setdata가 없어서 에러가 난다.
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-24-93d8790fe05f> in <module>
          1 a = FourCal()
    ----> 2 a.add()
    

    <ipython-input-14-69ef961c238b> in add(self)
          4         self.second = second
          5     def add(self):
    ----> 6         result = self.first + self.second
          7         return result
          8     def mul(self):
    

    AttributeError: 'FourCal' object has no attribute 'first'



```python
# 그래서 초깃값을 설정해야 할때 setdata를 만들어 주는것 보다 생성자를 구현하는게 더 안전한 방법이다.
class FourCal:
     def __init__(self, first, second):
         self.first = first
         self.second = second
     def setdata(self, first, second):
         self.first = first
         self.second = second
     def add(self):
         result = self.first + self.second
         return result
     def mul(self):
         result = self.first * self.second
         return result
     def sub(self):
         result = self.first - self.second
         return result
     def div(self):
         result = self.first / self.second
         return result

```


```python
a = FourCal(4, 2)
print(a.first)
```

    4
    

## 클래스의 상속
: 기존 클래스를 변경하지 않고 기능을 추가할때 쓰임


```python
class MoreFourCal(FourCal):
     def pow(self):
         result = self.first ** self.second
         return result

```


```python
a = MoreFourCal(4, 2)
a.pow()
```




    16



## 메서드 오버라이딩
: 수정할때 그위에 덮기


```python
# 0으로 나눌때 값이 0으로 나오게끔 만들고싶을때,
class SafeFourCal(FourCal):
     def div(self):
         if self.second == 0:  # 나누는 값이 0인 경우 0을 리턴하도록 수정
             return 0
         else:
             return self.first / self.second


```


```python
a = SafeFourCal(4, 0)
a.div()
```




    0



## 클래스 변수
: 객체 변수는 독립적이었다면, 클래스 변수는 독립적이지 않다!!  
**클래스이름.클래스변수** 이렇게 쓰면 된다.


```python
class Family:
     lastname = "김"

```


```python
print(Family.lastname)
```

    김
    


```python
a = Family()
b = Family()
```


```python
print(a.lastname)
```

    김
    


```python
print(b.lastname)
```

    김
    


```python
# "박"으로 바꾼다.
Family.lastname = "박"
```


```python
print(a.lastname)
```

    박
    


```python
print(b.lastname)
```

    박
    
