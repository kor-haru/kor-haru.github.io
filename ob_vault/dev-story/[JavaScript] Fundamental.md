---
date: 2022-08-22 11:08
lastmod: 2022-08-22 11:08
tags:
- 101
- JavaScript
- Language
- knowledge
---

# JS 엔진

- V8 - Chrome, Opera, MS Edge (ver. 79 ~)
- SpiderMonkey - Firefox
- Trident - IE
- Chakra - IE
- ChakraCore - MS Edge (~ ver. 78)
- SquirrelFish - Safari

# JS 엔진의 동작

- 기본 원리
    1.  엔진이 스크립트를 읽음 (파싱)
    2.  읽은 스크립트를 기계어로 변환 (컴파일)
    3.  기계어로 변환된 코드가 실행
- 모든 단계마다 최적화가 진행됨
- 심지어 기계어로 변환된 뒤에도 동작과정에서 최적화 되기도 함

# 실행환경에 따른 차이

- 브라우저
    - 웹 페이지에 새로운 HTML을 추가하거나 기존 HTML 및 스타일 수정
    - 마우스 클릭, 포인터 움직임, 키보드 키 입력 등 사용자 이벤트에 반응
    - 네트워크를 통해 원격 서버에 요청, 파일 다운로드, 업로드 (AJAX, COMET 등)
    - 쿠키 관리 및 대화창을 통한 사용자와 상호작용 (e.g. alert, prompt 등)
    - 클라이언트 측에 데이터 저장 (e.g. 로컬 스토리지, 세션 스토리지, 등)
- 서버 (e.g. Node.js)
    - 호스트 파일시스템의 파일관리
    - 네트워크 요청 수행

# 브라우저에서 할 수 없는 일

- 브라우저 환경은 보안을 위해 제약이 걸려있음
- 클라이언트 운영체제의 파일 시스템에 대한 기능을 직접 사용하지 못하게 함
    - 사용자가 직접 드래그 드롭 기능을 이용하거나 `<input>` 태그를 이용하는 경우에 파일 접근이 가능함
    - 카메라, 마이크 등 다른 장치와 상호작용을 위해서는 사용자의 명시적인 사용권한의 허가가 필요함
- 기본적으로 브라우저 내 탭과 창은 독립적으로 상호 정보를 알 수 없음
    - JS를 사용해 한 창에서 다른 창을 열게되는 경우 예외
    - 그러나 도메인이나 프로토콜, 포트가 다르면 알 수 없음
    - 이 제약사항을 `동일 출처 정책(Same Origin Policy)`라고 함
    - 이 제약사항을 피하기 위해서는 두 페이지 간 데이터 교환에 동의가 필요하고 동의와 관련된 특수한 JS 코드를 포함해야함
- 기본적으로 타 사이트나 도메인에서 데이터를 받아오는 것이 불가능
    - 이 역시 가능하기 위해선 대상 서버에서 명확히 승인을 해줘야함

# 엄격모드

- ES5 부터 갱생된 JS의 최신 스펙을 활성화하는 지시자인 `use strict`를 JS 문서 최상단에 선언
- 브라우저의 콘솔은 기본적으로 엄격모드가 아님
    - `use strict` 입력 후 개행하여 작성

# 자료형

- [Primitive Type](https://www.notion.so/f89ec3aadb894c4e9f3f7bc45ebda307)

# 변수

- 같은 이름의 변수를 두 번 선언하면 에러가 발생
- 함수형 프로그래밍 언어는 변숫값 변경을 금지함

```javascript
let message = "This";

let message = "That"; // SyntaxError: 'message' has already been declared
```

# 할당 연산자 (`=`)

- 다른 기본적인(?) 사항을 뒤로하고 할당 연산자 또한 값을 반환한다는 것을 알아만두자
- 개인적으로... 가독성이 떨어지고 직관적이지 않아 사용하지 않는 편이 좋으나 모르면 읽을 수도 없음...

```javascript
let a;
let b = 10;
let c = 30 - (a = b+20); // a에 할당 및 반환
console.log(a)
>> 30

console.log(c)
>> 0
```

# 쉼표 연산자 (`,`)

- 여러 표현식을 코드 한 줄에서 평가하는 연산자로 표현식 각각이 모두 평가되지만, **마지막 표현식의 평가 결과만 반환**되는 점에 유의
- 또한 연산자의 우선순위가 매우 낮기 때문에 (할당 연산자 `=` 보다 더 낮음) 괄호가 중요함

```javascript
let a = (1+2, 3+4);
console.log(a);
>> 7 // (3 + 4의 결과)
```

- 한 줄에 여러 동작을 처리할 때 쓰지만... 이것도 **알아만두자**... 가독성이 떨어진

```javascript
// 한 줄에서 세 개의 연산이 수행됨
for (a=1, b=3, c=a*b; a<10; a++) {
	...
}
```

# JS에서 값을 비교한다는건...

```javascript
let a = "0";  // 문자열은 빈 문자열이 아닌이상 true
let b = 0;    // 숫자 0은 false

console.log(Boolean(a));
>> true

console.log(Boolean(b));
>> false

console.log(a == b);
>> true

console.log(a === b)
>> false
```

# `undefined` vs. `null` vs. `NaN`

```javascript
let a;
let b = null;

console.log(a == b);
>> true

console.log(a === b);
>> false

console.log(Number(a));
>> NaN

console.log(Number(b));
>> 0

console.log(a == 0);
>> false

console.log(a > 0);
>> false

console.log(a >= 0);
>> false

console.log(b == 0);
>> false

console.log(b > 0);
>> false

console.log(b >= 0);
>> true
```

- 동등연산자인 `==` 는 피연산자가 `undefined` 혹은 `null` 일 때 형변환을 하지 않음
- 반면, 비교연산자 `>`, `<`, `>=`, `<=` 연산자는 피연산자가 `null` 인 경우 0으로 변환함

# 레이블

- 반복문에서 사용되는 식별자

```javascript
lableName: for(...) {
	...
}
```

긴 말이 필요있을까...

```javascript
let breakPoint = 5;

outer: for(let i=0; i<10; i++) {
	for(let j=0; j<10; j++) {
		if(i > breakPoint) break outer;
	}
}

// 위에다 써도됨
outer:
for(let i=0; i<10; i++) {
	for(let j=0; j<10; j++) {
		if(i > breakPoint) break outer;
	}
}
```

# 함수

- 함수는 특수한 객체, 구조체로 취급되지 않고 값으로 취급됨 (동작을 나타내는 값으로 생각하자)
- 따라서 괄호 없이 함수의 이름, 표현식이 할당된 변수명을 참조 시 함수 내부 코드가 출력됨
- 즉, `functionName()`과 같이 괄호와 함께 사용 시 호출이 가능한 특수한 값으로 인지함
- 함수에 `return` 문이 없거나 `return` 만 있는 경우 `undefined` 가 반환됨

## `함수 선언문` vs. `함수 표현식`

- 함수를 만드는 방법이 크게 2가지 있음
- 함수 선언문
    
    ```javascript
    function foo(arg1, arg2) {
    	// do something...;
    	return something;
    }
    ```
- 함수 표현식
    ```javascript
    let foo = function(arg1, arg2) {
    	// do something...;
    	return something;
    }
    ```
- 두 방식은 JS 엔진이 함수를 실제로 생성하는 시점이 다름
    - 함수 선언문 방식은 런타임이 실제 함수 정의 전에도 호출이 가능
    ```javascript
    foo('World');  // Hello, World
    
    function foo(arg) {
        console.log(`Hello, ${arg}`);
    }
    ```
    - 함수 표현식 방식은 런타임이 실제 함수 정의에 도달해야만 실행 가능
    ```javascript
    foo('World');  // error!
    
    let foo = function(arg) {  // * 실제 런타임이 여기까지 진행되어야 실행가능
        console.log(`Hello, ${arg}`);
    }
    ```

# ECMA262 (표준 명세서)

- 명세 제안 -[ECMAScript® 2021 Language Specification](https://tc39.es/ecma262/)
- JS 표준 명세서 - [Standard ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- 브라우저 별 명세 호환성 - [ECMAScript 6 compatibility table](https://kangax.github.io/compat-table/es6/)

## 바벨 (Babel)

- 트랜스파일러(transpiler)로 명세서에 최근에 추가된 API를 사용하게 될 때, 구 표준 코드로 변환해줌

### 트랜스파일러

- 코드를 재작성해줌
- 웹팩(webpack)과 같은 모던 프로젝트 빌드 시스템은 자동으로 트랜스파일러를 동작 시킴

### 폴리필

- 명세서에 추가된 새로운 문법이나 내장 함수를 사용한

# 심볼형

- 같은 입력으로 다른 값을 가지게 되며 이를 이용하여 고유한 식별자를 만들 수 있음
- 프로퍼티의 키로 문자형 외에 심볼형을 사용할 수 있음
- 프로퍼티의 키로 사용하여 서드파티 라이브러리와 충돌 없이 객체에 식별자 주입이 가능함
- 예외적으로 자동 형 변환 되지 않아 출력을 하게 될 경우 오류 발생
- 심볼로 구성된 프로퍼티는 iteration에서 배제됨
- 같은 입력에 같은 값을 반환하는 **전역 심볼**도 존재함

# [객체형에서 원시형으로 형변환](https://ko.javascript.info/object-toprimitive)

- 객체의 형변환의 경우 hint 값을 기준으로 구분됨
- hint
    - string : 함수 혹은 연산자가 문자열을 기대하는 경우 (e.g. 프로퍼티의 키, alert 함수)
    - number : 함수 혹은 연산자가 숫자형을 기대하는 경우 (e.g. - 연산자, > < 연산자)
    - default : 함수 혹은 연산자가 기대하는 자료형이 확실치 않을 때 (e.g. + 연산자, == 연산자)
- 또한, 실제로 Date 객체를 제외한 모든 내장 객체는 default인 경우, number로 처리함
- 즉, 실질적으로는 object to string, object to number 두 경우 밖에 없다고 생각해도 무방
- 모든 객체는 true로 평가되기 떄문에 hint에 boolean은 없음

# [원시값을 객체처럼 다루기](https://ko.javascript.info/primitives-methods#ref-78)

- 기본 동작 컨셉
    1.  원시값은 원시값 그대로 남겨둬 단일 값 형태를 유지 (객체로 다루지 않음)
    2.  문자열, 숫자, 불린, 심볼의 메서드와 프로퍼티에 접근할 수 있도록 언어 차원에서 허용
    3.  이를 가능하게 하기 위해, 원시값이 메서드나 프로퍼티에 접근하려 하면 추가 기능을 제공해주는 특수한 객체, "원시 래퍼 객체(object wrapper)"를 만듦. 해당 객체는 곧 삭제됨
- 원시 래퍼 객체는 원시 데이터 타입별로 상이함
- null과 undefined는 래퍼 객체가 존재 하지 않음
