---
date: 2022-08-22 10:08
lastmod: 2022-08-22 10:08
tags:
- JavaScript
- Angular
- knowledge
- dump
---

- nodejs 버전 관리
    - npm cache clean -f
    - npm install -g n
    - n stable
- SET DEBUG=learn-express:* & npm start
- Angular cli
    - 신규 프로젝트 ng new ${project_name}
    - 신규 컴포넌트 ng g component ${component_name}
    - 신규 서비스 ng g service ${service_name}
- module.ts
    - NgModule
        - 모듈에서 사용할 컴포넌트 등록
    - imports
        - import 될 모듈 및 컴포넌트?
    - providers
        - 자주 사용되고 공통으로 공유될 데이터, 객체?
        - `{provide: $key, useValue: $data}`
        - 각 컴포넌트의 생성자를 통해 주입 받을 수 있음
        - 생성자로 주입받은 객체는 싱글톤을 보장 받음?
        - ng가 아닌 일반 ts로 생성한 클래스를 주입없이 생성하여 사용할 수 있지만 new를 통한 객체화가 필요함
    - bootstrap
        - 기본 동작 컴포넌트
- es6 lambda 식? // ?는 옵셔널을 의미
```jsx
var method = (arg?): void => { console.log(arg); }
method('print');
method(); // undefined
```
- construct()에는 @Inject 인자를 통해 컴포넌트 초기 설정에 필요한 값을 주입 받을 수 있음. module에 providers에 정의된 provider의 useValue를 주입 받음
- 컴포넌트와 컴포넌트를 연결하는 데 html 파일이 필요함
- 이벤트를 전달하는 녀석은 소괄호에 이름을 명시하고, 받는 녀석은 ='받는메소드($event)' 로 표현
- 컴포넌트간에 데이터 전달
    - `@Input`, `@Output` 데코레이터가 필요
    - 보내는 방법
        - Component : `@Output() 보내는변수명 = new EventEmitter<any>();`
        - HTML : `(보내는변수) = '받는 메소드'`
    - 받는 방법
        - Component : `@Input 받는변수명;`
        - HTML : `[받는변수명]='보내는 변수명'`
- The ngOnInit() is a lifecycle hook. Angular calls ngOnInit() shortly after creating a component. It's a good place to put initialization logic.
- Data Binding
    - one way data binding
        - [consuming_name] = "provide_name"
- service injecterable
    - https://angular.io/tutorial/toh-pt4#provide-the-heroservice
- Observable data
    - https://angular.io/tutorial/toh-pt4#observable-data
- add AppRouting
    - https://angular.io/tutorial/toh-pt5#add-the-approutingmodule