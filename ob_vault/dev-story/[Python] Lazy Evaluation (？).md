---
date: 2022-08-21 02:08
lastmod: 2022-08-21 02:08
tags:
- dev
- python
- Lazy Evaluation
---

## 나는 게으르다...

으으... 역시나 게으른 성격이라 한 달에 한 번 포스팅조차 하지않는 사람... 2월이 가기 전에 하나는 써야지 미루다 결국....
최근에는 기술 서적보다 '소프트웨어 장인', '개발자의 글쓰기', '어떻게 공부할 것인가', 등의 기본소양 위주의 책을 읽다보니 포스팅할 것이 없어서... ~~(그냥 공부를 안 한다.)~~
아무튼 각설하고 최근에 필요해서 공부했지만 결국 스스로 해결을 못해 도움을 받은 함수의 파이프라이닝에 대한 고찰에 대한걸 포스팅하고자 한다.

> [!info] 분명히 FP에서 이야기 하는 Lazy Evaluation과는 차이가 있는것 같지만... 정확히 뭐라 칭해야 할지 잘 몰라서... 본인의 게으름을 강조하고 싶은 마음에... 단어를 선택했지만... Lazy Evaluation에 대한 참교육을 해주신다면 감사드립니다.

---

## 요구사항

1. 여러가지 함수를 필요한 조합과 실행 순서에 따라 실행되어야 함 (함수간 인터페이스는 다 맞는다고 가정함)
2. 모든 단계의 함수의 동작에는 이전 함수의 반환 값을 필요로 할 수 있음
3. 이미 실행된 함수는 두 번 실행되지 않음

#### 함수 목록
```python
def f1():
    print("[f1] running.")
    return "output of f1"


def f2_1(arg1, arg2=None):
    print(f"[f2_1] running. arg1: '{arg1}', arg2: '{arg2}'")
    return "output of f2_1"


def f2_2(arg1, arg2=None):
    print(f"[f2_2] running. arg1: '{arg1}', arg2: '{arg2}'")
    return "output of f2_2"


def f3(arg1, arg2):
    print(f"[f3] running. arg1: '{arg1}', arg2: '{arg2}'")
    return "output of f3"
```

#### 실행 순서

```mermaid
graph LR

    f1-->f2-1
    f1-->f2-2
    f2-1-->f3
    f2-2-->f3
```

> [!info] `f1` 함수가 시작함수로 총 **2번** 호출되고, `f3` 함수가 마지막 함수이다.

---

## 전략

함수를 실행할 때 최종적으로 명시적인 값을 필요로하는 함수에서 부터 필요로 하는 이전 함수 반환 값을 **거꾸로 연산**하면서 수행하도록 만드는 것 ~~(말로 설명하기가 너무 어렵다)~~ 을 단계적으로 보자.

(1) 단순히 반환 값을 참조하는 행위는 실제 함수가 동작하지 않고 명시적으로 값을 검증하는 경우에만 실제 값을 반환하도록 함수의 반환을 객체로 만들기

```python
class FuncOutput:
    def __init__(self, f):
        self.f = f
        pass
        
    def eval(self):
        return self.f.res
```

(2) 위에서 구현한 반환 객체를 적용하고 실행 결과값을 캐싱하게끔 만들기위해 원시함수를 내장하는 새로운 함수 객체로 만들기.

```python
class FuncBlock:
    def __init__(self, f, in_map):
        self.f = f
        self.in_map = in_map
        self._out = None
        self.__result = None
        
    @property
    def res(self):
        # 명시적인 값 검증에 사용될 연산부
        if self.__result is None:
            related_kwargs = {k: v.eval() for k, v in self.in_map.items()}
            res = self.f(**related_kwargs)
            self.__result = res
        return self.__result
        
    @property
    def out(self):
        # 단순 결과값 참조에 사용될 참조부
        if self._out is None:
            self._out = FuncOutput(self)
        return self._out
```

> [!info] 위의 코드는 **불필요한 부분을 덜어낸 코드**로 함수의 결과값을 외부에서 할당하지 않게 하기 위해 `@res.setter` 및 `@out.setter`를 설정하여 프로퍼티를 보호할 필요가 있다. 자세한건 `@property` 관련된 [지난 포스팅]([Python]%20About%20property.md)을 참조.

(3) 함수들을 연결해보자!

```python
if __name__ == '__main__':
    a = FuncBlock(f1, {})
    print(f"[refer to f1.out] {a.out}")
    
    b = FuncBlock(f2_1, {'arg1': a.out})
    print(f"[refer to f2_1.out] {b.out}")
    
    c = FuncBlock(f2_2, {'arg1': a.out})
    print(f"[refer to f2_2.out] {c.out}")
    
    d = FuncBlock(f3, {'arg1': b.out, 'arg2': c.out})
    print(f"[final result] {d.out.eval()}")
    
    pass

# 결과
# [refer to f1.out] <__main__.FuncOutput object at 0x7fe477f675c0>
# [refer to f2_1.out] <__main__.FuncOutput object at 0x7fe477fb3ba8>
# [refer to f2_2.out] <__main__.FuncOutput object at 0x7fe477f67da0>
# [f1] running.
# [f2_1] running. arg1: [output of f1], arg2: [None]
# [f2_2] running. arg1: [output of f1], arg2: [None]
# [f3] running. arg1: [output of f2_1], arg2: [output of f2_2]
# [final result] output of f3
```

위에서 보이듯이 `__main__`에서 각 함수의 인스턴스 `a, b, c`에 결과값을 참조 할 때는 `FuncOutput` 인스턴스가 반환되고 마지막에 `f3` 함수의 결과를 명시적으로 연산하는 `eval()`이 호출되었을 때 함수들이 수행되는 것을 볼 수 있다.

더하여 `b`와 `c` 양쪽에 모두 사용되는 `a`는 단 한번만 실행되는 것으로 결과값의 캐싱이 잘 되는것을 확인할 수 있다.

---

### 응용...?

실행 순서를 조금 바꿔보자.

```mermaid
graph LR

f1 --> f2-1
f1 --> f3
f2-1 --> f2-2
f2-1 --> f3
```

```python
if __name__ == '__main__':
    a = FuncBlock(f1, {})
    print(f"[refer to f1.out] {a.out}")
    
    b = FuncBlock(f2_1, {'arg1': a.out})
    print(f"[refer to f2_1.out] {b.out}")
    
    c = FuncBlock(f2_2, {'arg1': b.out})
    print(f"[refer to f2_2.out] {c.out}")
    
    d = FuncBlock(f3, {'arg1': a.out, 'arg2': b.out})
    print(f"[final result] {d.out.eval()}")
    
    pass

# 결과
# [refer to f1.out] <__main__.FuncOutput object at 0x7f9fb9177208>
# [refer to f2_1.out] <__main__.FuncOutput object at 0x7f9fb9177470>
# [refer to f2_2.out] <__main__.FuncOutput object at 0x7f9fb9177550>
# [f1] running.
# [f2_1] running. arg1: [output of f1], arg2: [None]
# [f3] running. arg1: [output of f1], arg2: [output of f2_1]
# [final result] output of f3
```

마지막에 실행되는 `f3` 함수가 이제는 `f2_2`대신 `f1`을 필요로 하게 되고 실제 `f3`의 결과 값 연산 시 f2_2는 실행되지 않는 것을 볼 수 있다. 이렇게 `FuncBlock`을 생성할 때 실행에 필요한 이전 함수를 바꾸는 것 만으로 간단하게(?) 함수를 연쇄실행 할 수 있게 된다.

여기서 실행 결과를 유심히 보셨다면 다소 불편한 분이 있을 수 있다. `f2_1`와 `f2_2` 함수의 정의에서 `arg2`의 기본값을 `None` 으로 설정해서 계속해서 결과에 `None`이 찍히게 되는데, 이를 이전 함수의 결과 외에 실행에 필요한 인자를 받도록 `FuncBlock`을 개선해보자.

```python
class FuncBlock:
    def __init__(self, f, input_map, **kwargs):
        self.f = f
        self.input_map = input_map
        self._out = None
        self.__result = None
        self.kwargs = kwargs
        
    @property
    def res(self):
        if self.__result is None:
            related_kwargs = {k: v.eval() for k, v in self.input_map.items()}
            self.kwargs.update(related_kwargs)
            res = self.f(**self.kwargs)
            self.__result = res
        return self.__result
        
    @property
    def out(self):
        if self._out is None:
            self._out = FuncOutput(self)
        return self._out


if __name__ == '__main__':
    a = FuncBlock(f1, {})
    print(f"[refer to f1.out] {a.out}")
    
    b = FuncBlock(f2_1, {'arg1': a.out}, arg2=10)
    print(f"[refer to f2_1.out] {b.out}")
    
    c = FuncBlock(f2_2, {'arg1': a.out}, arg2=20)
    print(f"[refer to f2_2.out] {c.out}")
    
    d = FuncBlock(f3, {'arg1': b.out, 'arg2': c.out})
    print(f"[final result] {d.out.eval()}")
    
    pass

# 실행 결과
# [refer to f1.out] <__main__.FuncOutput object at 0x7f9fb9183080>
# [refer to f2_1.out] <__main__.FuncOutput object at 0x7f9fb91750f0>
# [refer to f2_2.out] <__main__.FuncOutput object at 0x7f9fb9183860>
# [f1] running.
# [f2_1] running. arg1: 'output of f1', arg2: '10'
# [f2_2] running. arg1: 'output of f1', arg2: '20'
# [f3] running. arg1: 'output of f2_1', arg2: 'output of f2_2'
# [final result] output of f3
```

편안해진 모습...  이전 함수의 결과인 `related_kwargs`와 외부에서 입력받는 다른 인자를 하나의 `**kwargs`로 만들어 실행에 사용하여 조금 더 유연한 실행방법을 만들어 보았다.

이를 좀 더 개선하면 원시 함수의 반환 값이 여러개인 경우에 특정 출력만 선택하여 파이프라이닝을 할 수도 있다.... 그렇지만 오늘은 여기까지...