---
date: 2022-08-18 13:08
lastmod: 2022-08-18 13:08
tags:
- programming
- language
- javascript
- 동작방식
---

# 호이스팅 (Hoisting)

- JS에서 코드는 실행되기 전에 Parser가 해당 코드를 한 번 훑고 필요한 변수값들을 모두 모아서 유효 범위의 최상단에 선언하는 행위
- 즉, 함수 내에서 아래쪽에 존재하는 내용 중 필요한 값들을 끌어올리는 것. 실제로 코드가 끌어올려지는 건 아니며, JS Parser 내부적으로 끌어올려서 처리하는 것으로 실제 메모리에서는 변화가 없음

## 대상

- var 변수 및 함수 선언
- 선언만 호이스팅되며 할당은 대상이 아님
- let, const, 함수표현식

###  `var` vs. `let`, `const`
```Javascript
/*--- 원본 코드 ---*/
console.log("hello");
var foo = 10;  // var 변수
let bar = 20;  // let 변수

/*--- 호이스팅 동작 시 ---*/
var foo;  // 선언부 호이스팅
console.log("hello");
foo = 10;  // 할당
let bar = 20;
```

### 함수 선언 vs. 함수 표현
```Javascript
/*--- 원본 코드 ---*/
foo();
bar();

// 함수 선언문 (Function Declaration)
function foo() {
  console.log("hello");
}

// 함수 표현식 (Function Expression)
var bar = function() {
  console.log("hello2");
}

/*--- 호이스팅 동작 시 ---*/
var bar; // 선언부 호이스팅
function foo() { // 선언부 호이스팅
  console.log("hello");
}

foo();
bar(); // ERROR!!

bar = function() {
  console.log("hello2");
}
```