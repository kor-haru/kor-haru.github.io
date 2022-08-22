---
date: 2020-01-31 02:08
lastmod: 2020-01-31 02:08
tags:
- DEV
- Web
- Angular
- Framework
- JavaScript
- TypeScript
---

최근에 Angular 공부를 시작해서 우선하여 공식 튜토리얼을 진행하면서 기본적이어서 기억해야 될 것 같은 내용과 좀 더 학습을 해야 될것 같은 내용을 진행 순서에 따라 정리한 글... 인데 단순히 번역을 목적으로 하지 않고 이해한 바를 개인적인 표현으로 정리를 하고자 했기에 읽기 어색한 부분이 있을 수도 있다. ~~(불편하다면 메일을 보내면 된다.)~~

아 참고로 대상 튜토리얼은 [`Tour of Heroes App and Tutorial`](https://angular.io/tutorial) 이다.

---

## Component

```typescript
// src/app/foo/foo.component.ts
...
@Component({
  selector: 'app-foo',                 // 컴포넌트의 CSS element selector
  templateUrl: './foo.component.html', // 컴포넌트의 템플릿 경로
  styleUrls: ['./foo.component.css']   // 컴포넌트의 CSS 스타일 경로
})
export class FooComponent {
  bar = 'test';
  ...
}
```

---

## 단방향 데이터 바인딩 (One-way data binding)

```html
<!-- src/app/foo/foo.component.html -->
...
<app-bar [bar-property]="foo-property"></app-bar>
...
```

```typescript
// src/app/bar/bar.component.ts
...
@Component({
  selector: 'app-bar',
  ...
})
export class Bar {
  ...
  // foo-property가 바인딩 될 프로퍼티
  @Input() bar-property;
  ...
}
```

[`프로퍼티 바인딩(Property binding)`](https://angular.io/guide/template-syntax#property-binding) 이라고 하는데 한 템플릿에서 하위 컴포넌트로 데이터(프로퍼티)를 바인딩 하는 방법으로 템플릿에서는 `[하위컴포넌트의 프로퍼티]="바인딩 할 데이터(프로퍼티)"` 형식으로 작성하고 해당 컴포넌트 클래스의 프로퍼티는 [`@Input`](https://angular.io/guide/template-syntax#inputs-outputs) 프로퍼티로 선언이 되어야한다.

---

## 양방향 데이터 바인딩 (Two-way data binding)

>  `상자속의 바나나(banana in box)` 만 생각하기 =>  `[()]`

```html
<!-- src/app/foo/foo.component.html -->
...
<input [(ngModel)]="bar"/>
...
```

위 템플릿에서 보이는 `[(ngModel)]` 은 앵귤러의 양방향 바인딩 문법으로 컴포넌트의 프로퍼티인 `bar` 와 HTML의 입력창을 양방향 바인딩을 하여 값을 수정할 경우 변경된 데이터가 화면에 반영된다.

> `ngModel` 모듈의 경우 `FormsModule` 이 필요하기 때문에 `app.module.ts` 에 추가를 해주어야 한다.

---

## 템플릿의 *ng 문법

```html
<!-- src/app/foo/foo.component.html -->
...
<ul>
  <li *ngFor="let element of iterable">
</ul>
...
```

> 템플릿에서 사용할 수 있는 `ngFor`, `ngIf` 와 같은 ng문법을 사용할때 꼭꼭 앞에 `*` 을 빼먹지 말라고 강조한다...

---

## [이벤트 바인딩](https://angular.io/guide/template-syntax#event-binding)

```html
<!-- src/app/heroes/heroes.component.html -->
...
<ul>
  <li *ngFor="let hero of heroes" (click)="onSelect(hero)">
</ul>
...
```

위 템플릿에서 `(click)` 은 앵귤러의 이벤트 바인딩 문법의 한 종류이다.

~~이벤트는 DOM에만 바인딩 할 수 있다?~~

~~템플릿의 `()` 는 수신 지시자이다?~~

---

## [Style `class` 바인딩](https://angular.io/guide/template-syntax#class-binding)

```html
<!-- src/app/heroes/heroes.component.html -->
...
<ul>
  <li *ngFor="let hero of heroes"

    [class.selected]="hero === selectedHero"

    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
...
```

템플릿상에서 `[class.some-css-class]="some-condition"` 으로 Element에 스타일의 클래스를 바인딩 할 수 있다.

---

## 왜 Service인가... ?

컴포넌트는 데이터를 출력하는데에 중점을 두고 직접적인 데이터 핸들링은 서비스에게 위임하는 것이 좋은데, 일반적으로 데이터를 핸들링 하는것은 비즈니스 로직으로 컴포넌트에서 해당 부분을 분리하면 이후 비즈니스 로직이 변경되더라도 컴포넌트를 수정할 필요가 없어진다. (~~물론 데이터 바인딩까지 변경되는 경우엔 아니겠지만...~~)

더하여 서비스는 독립적인 클래스 간 정보를 공유하기 좋은 방법으로 아무런 관련이 없는 컴포넌트 간에도 동일한 데이터를 흐르게 만들 수 있다.

---

## [Dependency Injection](https://angular.io/guide/dependency-injection)

서비스를 생성하면 서비스 클래스에 [`@Injectable()`](https://angular.io/api/core/Injectable) 데코레이터가 적용되어 있는데, 이는 앵귤러 의존성 주입 시스템에 해당 클래스를 등록시키는 의미이다.  여기서 서비스를 생성하고 딜리버리하는 주체를 [`provider`](https://angular.io/guide/providers) 라고한다.

```typescript
// src/app/foo.service.ts
...
@Injectable({
  providedIn: 'root'
})
export class FooService {
  ...
}
```

위 같이 서비스를 생성하게 되면 [`@Injectable()`](https://angular.io/api/core/Injectable) 데코레이터 안에 `provideIn` 이라는 메타데이터를 설정할 수 있는데, Angular CLI를 통해서 생성된 서비스는 기본적으로 [`root injector`](](https://angular.io/guide/dependency-injection)) 에 등록하도록 설정되는 것이다.

메타데이터에 [`provider`](https://angular.io/guide/providers) 를 등록하는 것으로 서비스는 관리가 되는데 앵귤러는 서비스들을 싱글톤으로 관리하고 필요로 하는 클래스에 주입까지(root 레벨인 경우는 어떠한 클래스에도 주입이 가능함) 다 해주는데... 심지어 사용되지 않는 서비스는 제거하는 최적화까지 해준다. ~~(너흰 신경쓰지마 내가 다 해줄게)~~

```typescript
// src/app/foo/foo.component.ts
import { FooService } from '../foo.service';
...
@Component({...})
export class Foo {
  ...
  constructor(private fooService: FooService) {}
  ...
}
```

서비스를 주입받고 싶은 컴포넌트에서는 위와 같이 해당 서비스를 import한 후 클래스 생성자 함수의 파라미터로 전달해주면 객체 생성시점에 파라미터들을 프로퍼티로 정의하고 의존성 주입을 받을 수 있다. 추가적으로 혹시라도 템플릿에 바인딩 될 프로퍼티를 주입받는 경우 `public` 만 바인딩이 가능함을 주의하자.

---

## [Lifecycle Hooks](https://angular.io/guide/lifecycle-hooks) - [ngOnInit](https://angular.io/guide/lifecycle-hooks#oninit)

클래스의 생성자 함수에서 인스턴스 생성 및 초기에 **필요한 작업**을 수행할 수 있으나, 이는 바람직한 동작은 아니다. 생성자 함수는 매개변수를 바탕으로 프로퍼티를 정의하거나 인스턴스 생성에 필요한 설정 외에는 어떤 동작도 하지않는 간단한 생성자 함수로 만드는것을 권고한다.

실제 인스턴스 생성 직후에 동작수행이 필요한 경우 `ngOnInit()`이라는 ~~생명주기..~~ [`Lifecycle Hook`](https://angular.io/guide/lifecycle-hooks) 메소드 사용을 권장한다.

```typescript
// src/app/foo/foo.component.ts
import { OnInit } from '@angular/core';
...
@Component({...})
export class Foo implements OnInit {
  ...
  ngOnInit() {
    doSomething();
  }
  ...
}
```

위의 코드처럼 `OnInit ` 을 import하고 클래스에 구현을 하면 된다.

---

## Observable Data

현실의 서비스는 원격지의 데이터를 활용하거나 다른 서비스 제공자와 통신이 필요한 경우가 절대적으로 많고, 이는 본질적으로 비동기 처리다. 이를 위해 Angular에서는 `Rx JS` 의  `Obserable`을 주로 활용한다. (`Obserable` 에 관한 자세한 내용은 `Rx JS` 의 학습이 필요하다....ㅠ)

간단하게 정리해서 서비스는 `Observable` 을 반환하고 컴포넌트에서는 `Subscribe` 해서 비동기 처리의 결과를 콜백에서 사용한다.

```typescript
// src/app/foo.service.ts
...
@Injectable({
  providedIn: 'root'
})
export class FooService {
  url = 'domain/which/you/need';

  getBar(): Observable<Bar> {
    return this.http.get<Bar>(this.url);
  }
  ...
}
```

```typescript
// src/app/foo/foo.component.ts
import { FooService } from '../foo.service'
...
@Component({...})
export class Foo implements OnInit {
  ...
  constructor(private fooService: FooService) {}

  ngOnInit() {
    this.fooService.getBar()
      .subscribe((bar) => {
        // Do Something with bar
      });
  }
  ...
}
```

서비스의 `getBar()` 함수는 `Observable` 을 반환하고 해당 함수를 사용하는 컴포넌트는 비동기 처리에 따른 결과를 콜백 함수로 `emit` 하는 `subscribe()` 를 통해서 후속 처리를 할 수 있다.

> `Obserable` 을 `subscribe` 하는 것 이외에도 `pipe()` 나 `tap()` 과 같은 `Rx JS` 를 잘 활용하면 에러 처리나 콜백 이외의 함수로 값을 전달하는 것과 같은 유용한 작업을 수행할 수 있다.... `Rx JS` 를 공부하자...

---

## Routing

```typescript
// src/app/app-routing.module.ts
...
const routes: Routes = [
  { path: 'foo/:bar', component: FooComponent },
  ...
];
...
```

위와 같이 라우팅의 `path` 에  `:` 을 사용하여 파라미터를 넘길 수 있다. (쿼리스트링이라고 해야되나...)

```html
<!-- src/app/foo/foo.component.html -->
...
<a *ngIf="bar" routerLink="/foo/{{bar}}">/foo로 bar전달</a>
...
```

템플릿에서 라우터를 통해 다른 url로 이동 시 파라미터를 넘길 필요가 있을 경우 위와 같이 바인딩 하면 `foo` 클래스의 `bar` 프로퍼티가 `/foo` url의 파라미터로 전달이 된다.

> [!info] 위처럼 템플릿에서 클래스의 프로퍼티를 `{{..}}` 를 이용하여 바인딩하는 방식을 [interpolation 바인딩](https://angular.io/guide/template-syntax#interpolation)이라 한다.

```typescript
// src/app/bar/bar.component.ts
import { ActiveRoute } from '@angular/router';
...
export class BarComponent implements OnInit {
  constructot(
    private route: ActiveRoute,
    ...
  ) {}
  
  ngOninit() {
    this.doSomething();
  }
  
  doSomething() {
    barFromQueryString = this.route.snapshot.paramMap.get('bar');
    ...
  }
  ...
}
```

위처럼 라우터 모듈을 통해 동적인 라우팅이 필요한 컴포넌트는 라우팅에 필요한 정보를 쥐고있는 `ActiveRoute` 를 활용하면 된다.

`route.snapshot` 은 라우팅을 통해 컴포넌트가 생성된 직후의 route 정보에 대한 snapshot으로 `paramMap` 은 URL에 포함된 파라미터들을 보관하는 딕셔너리이다.

> [!info] 쿼리스트링으로 넘어오기 때문에 값은 당연히 String 임을 주의하자. 혹시라도 숫자로 받아야하는 경우엔 `bar = +this.route.snapshot.paramMap.get('some-number');` 와 같이 JS의 + 연산자를 활용하면 숫자로 변환이 된다.

---

## HTTP

```typescript
// src/app/foo.service.ts
...
@Injectable({...})
export class FooService {
  url = 'domain/which/you/need';

  getBar(): Observable<Bar> {
    return this.http.get<Bar>(this.url);
  }
  ...
}
```

모든  `HttpClient` 는 `Rx JS` 의 `Observable` 을 반환하고 해당 `Observable` 은 항상 단일 값(single value)을 한 번만 리턴한다.

`HttpClient.get()` 을 통해 받은 응답은 일반 JSON 객체로 반환되기 때문에 제네릭(혹은 캐스팅...?)을 통해 응답 객체를 형식화해서 사용할 수 있다.
