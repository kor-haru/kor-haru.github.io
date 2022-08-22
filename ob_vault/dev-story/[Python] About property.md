---
date: 2020-01-07 01:08
lastmod: 2020-01-07 01:08
tags:
- dev
- python
- Class
- OOP
- Object Oriented Programming
---

## 객체의 캡슐화

흔히 `객체 지향 프로그래밍(Object Oriented Programming, 이하 OOP)`을 할 때, 객체와 관련된 속성(멤버변수, 메소드)을 포함한 Class로 정의하여 활용한다.

여기서 개발자는 객체를 캡슐화하여 속성을 보호하고 속성을 다루는 기능(멤버함수, 메소드)을 제공하여 의도와 다른 동작을 막을 필요가 있다.

이를 위해 일반적인 객체지향 언어들은 직접적으로 객체의 속성에 접근하여 조작하지 못하도록 `접근제어자(access modifier)`를 제공하여 속성을 보호(private)하고 getter/setter 메소드를 활용하여 안정적인 동작을 하게한다. 이때 접근제어를 필요로하는 멤버변수를 `Property`라고 한다.

---

## Python의 접근 제어

그러나 Python에서는 특이하게 Class 접근제어자의 형태로 보호하는 기능을 제공하지 않는다. 흔히 속성이름 앞에 `_`을 붙여서 속성을 보호할 수 있다고 생각할 수 있는데 이는 개발자의 의도를 나타내줄 뿐이지 언어적인 차원에서 접근제어를 처리해 주진 않는다.

``` python
>>> class Foo:
...     def __init__(self):
...         self.bar = 1
...         self._bar = 2
...     def get_public_bar(self):
...         return self.bar
...     def get_private_bar(self):
...         return self._bar
...
>>> foo = Foo()
>>>
>>> foo.get_public_bar()
10
>>> foo.get_private_bar()
20
>>> foo.bar = 10
>>> foo._bar = 20
>>>
>>> foo.get_public_bar()
10
>>> foo.get_private_bar()
20
```

---

## Python 객체의 속성 참조 제어

우선적으로 Python이 객체의 속성을 참조할 때 동작방식을 알 필요가 있다. Python에서 객체는 속성에 값을 할당하거나 획득할 때 속성의 이름과 값을 Key-Value 형태로 관리하는 `__dict__`라는 객체에 내장된 dictionary를 활용한다.

```python
>>> foo.__dict__
{'bar': 10, '_bar': 20}
```

즉, 객체 내부의 속성을 참조하는 `foo.bar`는 실제로 `foo.__dict__['bar']`로 동작하는 것을 알 수 있다. 그렇다면 Python은 어떻게 속성에 대한 캡슐화 혹은 접근 제어를 제공할까? 이를 위해 `property`라는 내장 함수가 제공된다. 우선 코드를 보자.

```python
>>> class Foo:
...     def __init__(self, value=0):
...         self._bar = value
...     def get_bar(self):
...         print("속성 'bar'가 참조됩니다.")
...         return self._bar
...     def set_bar(self, value):
...         print("속성 'bar'가 할당됩니다.")
...         self._bar = value
...     bar = property(get_bar, set_bar)
...
>>> foo = Foo(10)
>>>
>>> foo.__dict__
{'_bar': 10}
>>>
>>> foo.bar
속성 'bar'가 참조됩니다.
10
>>>
>>> foo.get_bar()
속성 'bar'가 참조됩니다.
10
```

Class `Foo`의 마지막 라인은 `bar`에 대한 `property` 객체를 만들게 되는데, 이 녀석은 대상 속성에 대한 참조 및 할당 동작에 특정 코드를 결합한다. 위의 예에선 속성 `bar`에 대해 참조는 `get_bar()`를, 할당은 `set_bar()`를 결합해 해당 동작들이 수행될 때 특정 코드가 실행되는 모습을 볼 수 있다.

여기서 눈치가 빠른 사람이라면 객체 `foo` 생성 시 속성 `bar`의 할당을 할 때는 "왜 print()가 동작하지 않느냐?" 혹은 "왜 `foo.__dict__`에는 `_bar`라고 나오죠?"라고 할 수 있다.

코드를 다시 세심히 보게 되면 Class 정의 시에 참조되는 값은 `self._bar`이다. `get_bar()`도 그렇고 `set_bar()`도 마찬가지로 실제 참조하는 속성은 객체 `foo`의 `_bar`인 것을 알 수 있다. 즉, `property`는 객체의 속성에 대한 접근의 기본동작 방식을 `__dict__`를 사용한 방식으로부터 특정 함수를 통해 접근하도록 변경하여 속성 `_bar`를 객체 안팎으로 숨기면서 `bar`로 보이게 만드는 것이다. 이를 통해 캡슐화와 비슷한 효과를 볼 수 있다.

> [!help] `__init__` 함수에서 `self._bar`를 `self.bar`로 바꾼 뒤 실행해 보자. 출력이 어떻게 바뀌는가?

---

## Python의 내장함수인 Property

위에서 말했다시피 내장함수인 `property()`는 `property` 객체를 생성하는 함수로 명세는 다음과 같다.

```python
property(fget=None, fset=None, fdel=None, doc=None)
```

`fget`, `fset`, `fdel` 각각은 속성을 참조할 때, 값을 할당할 때, 제거될 때 동작할 함수고 `doc`은 속성의 doc string을 인자로 한다. 각각은 필수가 아니며 필요에 따라 선택적으로 설정하여 사용하면 된다.

공식문서의 API 설명을 읽어보면 다양한 사용법을 알 수 있는데...

```python
# 위의 예제에서 사용한 방식
bar = property(get_bar, set_bar)

# property 객체 생성 후 각각의 함수를 할당하는 방식
bar = property()
bar = bar.getter(get_bar)
bar = bar.setter(set_bar)

# 위의 두 방식은 완전히 같은 동작방식을 갖는다
```

이제껏 봐온 getter/setter 방식은 속성과 속성을 조작하기 위한 함수를 필요로 하기에 이는 불필요하게 클래스의 네임스페이스를 차지하고 복잡하게 만든다.

이제 대망의 마지막 방법으로 가장 `pythonic`하며 이때 까지의 많은 부족한 점을 해결하는 방법으로 데코레이터를 활용하는 방법이 있다. 데코레이터를 사용하면 속성과 같은 이름으로 `getter`와 `setter`를 설정할 수 있다.

```python
>>> class Foo:
...    def __init__(self, value=0):
...        self._bar = value
...    @property
...    def bar(self):
...        print("속성 'bar'가 참조됩니다.")
...        return self._bar
...    @bar.setter
...    def bar(self, value):
...        print("속성 'bar'가 할당됩니다.")
...        self._bar = value
...
>>> foo = Foo(10)
>>> foo.bar
속성 'bar'가 참조됩니다.
10
>>> foo.bar = 100
속성 'bar'가 할당됩니다.
>>> foo.bar
속성 'bar'가 참조됩니다.
100
```

---

## 관련링크
  
- [파이썬 문서 - 내장함수 (번역)](https://docs.python.org/ko/3/library/functions.html#property)
- [Python Documentation - Built-in Functions](https://docs.python.org/3/library/functions.html#property)
- [Programiz - Python @property](https://www.programiz.com/python-programming/property)
- [파이썬 언더스코어(_)에 대하여](https://mingrammer.com/underscore-in-python/)
- [파이썬 코딩도장 - 47.13 프로퍼티 사용하기](https://dojang.io/mod/page/view.php?id=2476)