---
date: 2020-03-31 01:08
lastmod: 2020-03-31 01:08
tags:
- dev
- python
- test
- unit test
- tdd
---

## 나는 여전히 게으르다...

으으... 이번달도 역시나 월말이 되어서야 포스팅을 하고자 늦은 시간이 되어서야 책상 앞에 앉았다. ~~(아마도 만우절 포스팅이 될거같은데...)~~ 특별히 뭔가 주제를 잡고 장편 연재를 하는게 아니다 보니 매번 뭘 써야할까 고민도 많은데... 지금은 포스팅을 그만두지 않고 꾸준히 하는것에 의미를 두도록 하자...


### 그래서 뭐냐..

각설하고 최근에 진행중인 프로젝트의 코드베이스가 많이 커지게 되어 코드베이스를 소규모의 여러 프로젝트로 분리하고 각각의 레포지토리에 관리를 하려는 시도를 하려고 하다가... 커밋 분리를 할 때 모든 브랜치를 꼼꼼히 살피지 못하여 분리를 잘못해버렸고 2주가 지나서야 코드베이스 분리가 잘못 되었음을 인지하고 꼬인 커밋을 찾아 최신버전의 코드를 복구하는데 애를 먹었다... 이후 대격변(?)을 통해 고생을 겪고 난 이후에 코드 테스트 자동화에 대한 관심이 생겼고 기억 속 저편 어딘가에 Java를 공부하던 시절에 경험했던 TDD가 떠올랐다.

우선은 커버리지가 어떻고 Mocking을 어떻게 하니 빨간불로 시작하니 뭐 그런... TDD를 완벽히 적용하고자 함은 아니고 앞으로 리팩토링이 조금이나마 진행되는 부분부터 기능 단위의 테스트 케이스를 코드화를 하고 Merge Request(또는 Pull Request, 이하 MR)나 Jenkins를 통한 CI가 진행될 때 이를 자동적으로 테스트하는 것을 목표로 해야하지만...

일단은 개인이 티켓을 처리하고 커밋을 할 때마다 개발,수정된 부분에 대해 스스로 테스트를 진행하고 이상이 없을 때 Push를 하는 이상적인 모습을 그려보고 프로젝트의 구성을 어떻게 할지, 테스트 자체를 어떻게 할지 고민을 해보았다.

---

## 프로젝트 구성

개인적으로 프로젝트 디렉토리를 구성할 때 소스 코드와 테스트 코드를 분리하여 구성하고 싶었고 IDE 같은 강려크한 기능이 장착된 툴을 활용하여 하기 보다는 어느 환경에서나 Python만 있다면 테스트가 가능하게 하고 싶었다. 우선은 프로젝트 구성을 살펴보면 아래와 같다.

``` shell
$ tree unittest_tuto
unittest_tuto
├── init.py
├── setup.py
├── src
│   ├── init.py
│   └── foo.py
└── tests
    ├── init.py
    └── foo_test.py
```

프로젝트 명은 뭐... 그냥 간단하게 `unittest_tuto`로 만들고 하위 디렉토리로 소스 코드에 해당하는 `src`와 테스트 코드에 해당하는 `tests`로 나누었다. `setup.py`는 다룰 대상은 아니지만 일단 구색 맞추기 용도로... 내가 개발하고 배포하는 전체 패키지가 저런식으로 구성되지 않을까 한다. init.py는 각 디렉토리를 하위 패키지로 인식 시키기 위함인데 이는 unittest 모듈을 활용하여 전체 테스트를 수행할 때 필요하다.

### 코드 작성

코드 자체에 뭔가 살펴볼 것은 없다.. 어차피 구색 맞추기 용이니까...

```python
# src/foo.py

class BasicFunction(object):
    def __init__(self):
        self.state = 0
        
    def increment_state(self):
        self.state += 1
        
    def clear_state(self):
        self.state = 0
```

```python
# tests/foo_test.py

import unittest
from ..src.foo import BasicFunction


class TestScenario1(unittest.TestCase):
    def setUp(self):
        self.func = BasicFunction()
        
    def test_case_1(self):
        self.assertTrue(True)
        
    def test_case_2(self):
        self.assertTrue(True)
        
    def test_case_3(self):
        self.assertEqual(self.func.state, 0)


class TestScenario2(unittest.TestCase):
    def setUp(self):
        self.func = BasicFunction()
        
    def test_case_1(self):
        self.assertTrue(True)
        
    def test_case_2(self):
        self.assertTrue(True)
        
    def test_case_3(self):
        self.assertEqual(self.func.state, 0)


if __name__ == '__main__':
    unittest.main()
```

이제 위와 같이 구성된 프로젝트를 CLI를 이용하여 테스트를 진행하면 된다.

``` shell
$ python3 -m unittest unittest_tuto.tests.foo_test -v
test_func1_of_case1 (unittest_tuto.tests.foo_test.TestCase1) ... Unit Test Initialized
Testing test_func1_of_case1...
Unit Test End
ok
test_func2_of_case1 (unittest_tuto.tests.foo_test.TestCase1) ... Unit Test Initialized
Testing test_func2_of_case1...
Unit Test End
FAIL
test_func3_of_case1 (unittest_tuto.tests.foo_test.TestCase1) ... Unit Test Initialized
Testing test_func3_of_case1...
Unit Test End
ok
test_func1_of_case2 (unittest_tuto.tests.foo_test.TestCase2) ... Unit Test Initialized
Testing test_func1_of_case2...
Unit Test End
ERROR
test_func2_of_case2 (unittest_tuto.tests.foo_test.TestCase2) ... Unit Test Initialized
Testing test_func2_of_case2...
Unit Test End
ok
test_func3_of_case2 (unittest_tuto.tests.foo_test.TestCase2) ... Unit Test Initialized
Testing test_func3_of_case2...
Unit Test End
ok

======================================================================
ERROR: test_func1_of_case2 (unittest_tuto.tests.foo_test.TestCase2)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/haru/dev/workspace/unittest_tuto/tests/foo_test.py", line 47, in test_func1_of_case2
    raise RuntimeError('Test error!')
RuntimeError: Test error!

======================================================================
FAIL: test_func2_of_case1 (unittest_tuto.tests.foo_test.TestCase1)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/haru/dev/workspace/unittest_tuto/tests/foo_test.py", line 29, in test_func2_of_case1
    self.assertTrue(False)
AssertionError: False is not true

----------------------------------------------------------------------
Ran 6 tests in 0.001s

FAILED (failures=1, errors=1)
----------------------------------------------------------------------
Ran 6 tests in 0.000s

OK
```

테스트 코드상에서 소스코드를 `import`만하고 전혀 테스트를 하고 있진 않지만... 이런식으로 테스트 코드 안에서 소스 코드의 특정 모듈을 테스트 할 수 있음을 볼 수 있다. 뭐... `unittest` API안에 있는 테스트 케이스 클래스나 관련 함수를 다루면 내용이 좀 더 많아 지겠지만이는 공식 문서를 직접 참조하는 것이 더 좋을 것 같다. 본 글에서는 `unittest`의 API를 상세히 다루기 보다는 그냥 이런식으로 전체 테스트를 작성하고 UAT(User Acceptance Testing) 수준의 완벽한 시나리오 테스트를 한다기 보다는 정말 단일 기능 중심으로 코드 수준의 단위 테스트를 이렇게 진행할 수도 있구나... 라는걸 정리해보았다.

---

### 관련자료

- [unittest — 단위 테스트 프레임워크 — Python 3.8.2 문서](https://docs.python.org/ko/3/library/unittest.html)
- [unittest — Unit testing framework - Python 3.8.2 Documentation](https://docs.python.org/3/library/unittest.html)