# 스트릭트 모드

기존과 다른 방식으로 자바스크립트를 파싱하고 실행하도록 지시하는 것이다.
strict mode 와 non-strict mode는 동시에 공존할 수 없다.<br />

## 브라우저별 스트릭트 모드를 지원 여부 동작 차이

스트릭트 모드를 지원하지 않는 브라우저에서는 스트릭트 모드의 코드가 다른 방식으로 동작한다.<br />

`"use strict";` 는 스트릭트 모드를 지원하는 브라우저에서 자바스크립트 엔진이 인식하여 스트릭트 모드로 전환한다.

스트릭트 모드를 지원하지 않는 브라우저는 단순한 문자열로 인식하여 무시된다.

## 스트릭트 모드 정의

### 스트릭트 모드 정의시 주의 사항

`"use strict";`는 스크립트 최상단에 존재해야 한다.<br />
그렇지 않다면 스트릭트 모드가 활성화되지 않을 수 있다.<br />

> `"use strict";` 이전에는 주석만 작성할 수 있다.

#### 활성화 되지 않는 예

```sh
# "use strict"; 보다 다른 코드가 먼저 작성되었으므로 스트릭트 모드가 활성화되지 않는다.

alert("some code");

"use strict";
```

### 전체 스크립트 스트릭트 모드 적용

```js
// 전체 스크립트 스트릭트 모드 구문
"use strict";
var v = "Hi!  I'm a strict mode script!";
```

#### 관련 사건 사고

[Amazon 'Lightning Deals' are not visible on 4.0 betas](https://bugzilla.mozilla.org/show_bug.cgi?id=579119)

### 함수 단위 스트릭트 모드 적용

함수 본문 처음에 `"use strict";`를 작성한다.

```js
function strict() {
  // 함수-레벨 strict mode 문법
  "use strict";
  function nested() {
    return "And so am I!";
  }
  return "Hi!  I'm a strict mode function!  " + nested();
}
function notStrict() {
  return "I'm not strict.";
}
```

### 모듈에서의 스트릭트 모드

JavaScript 모듈의 전체 컨텐츠는 `"use strict";` 지시자를 작성하지 않아도 자동으로 스트릭트 모드가 적용된다.

```js
function strict() {
  // 모듈이기때문에 기본적으로 엄격합니다
}
export default strict;
```

## 스트릭트 모드 취소

ㅈ바스크립트 엔진을 이전 방식으로 되돌리는 `"no use strict";`와 같은 지시자는 존재하지 않는다.<br />
엄격모드가 활성화되면 취소가 불가능 하다.

## 스트릭트 모드의 특징

### 실수로 글로벌 객체에 맴버를 추가하는것을 방지한다.

```js
"use strict";
// 전역 변수 mistypedVariable 이 존재한다고 가정

mistypedVaraible = 17; // 변수의 오타때문에 이 라인에서 ReferenceError 를 발생시킴
```

### 논 스트릭트 모드에서 조용히 무시되었던 오류를 알려준다.

쓸 수 없는 전역 또는 프로퍼티에 할당, getter-only 프로퍼티에 할당, 확장 불가 객체에 새 프로퍼티 할당등을 오류로 알려준다.

```js
"use strict";

// 쓸 수 없는 프로퍼티에 할당
var undefined = 5; // TypeError 발생
var Infinity = 5; // TypeError 발생

// 쓸 수 없는 프로퍼티에 할당
var obj1 = {};
Object.defineProperty(obj1, "x", { value: 42, writable: false });
obj1.x = 9; // TypeError 발생

// getter-only 프로퍼티에 할당
var obj2 = {
  get x() {
    return 17;
  },
};
obj2.x = 5; // TypeError 발생

// 확장 불가 객체에 새 프로퍼티 할당
var fixed = {};
Object.preventExtensions(fixed);
fixed.newProp = "ohai"; // TypeError 발생
```

### 삭제할 수 없는 프로퍼티를 삭제하려할 때 예외를 발생시킵니다.

```js
"use strict";
delete Object.prototype; // TypeError 발생
```

### 중복된 파라미터 이름 사용

```js
function sum(a, a, c) {
  "use strict";
  return a + b + c; // Uncaught SyntaxError: Duplicate parameter name not allowed in this context
}
```

### 8진법의 값을 정의 할 때 0 prefix로 붙는다면 예외가 발생합니다.

```js
function sum(a, c) {
  "use strict";
  var a = 010; // Uncaught SyntaxError: Octal literals are not allowed in strict mode.
}
```

### 원시값(primitive)에 프로퍼티 할당을 금지한다.

```js
(function () {
  "use strict";

  false.true = ""; // TypeError
  (14).sailing = "home"; // TypeError
  "with".you = "far away"; // TypeError
})();
```

### with를 사용하지 못하게합니다.

- `with` 문제
  - `런타임중`에 블록안의 모든 이름이 전달된 객체의 프로퍼티나 인근 또는 전역 스코프의 변수로 매핑될 수 있기 때문에 사전에 알 수가 없다.매핑될 수도 있다는 것입니다.

```js
"use strict";
var x = 17;
with (obj) {
  // !!! 구문 에러
  // 엄격모드가 아니라면, 이는 var x 가 되어야 하나요,
  // obj.x 가 되어야 하나요?
  // 일반적으로는 코드를 실행하지 않고 이를 말하는 것은 불가하므로,
  // 이름을 최적화 할 수 없습니다.
  x;
}
```

### 스트릭트 모드에서 eval은 새로운 변수를 주위 스코프에 추가하지 않는다.

```js
"use strict";
eval("var aaa = 1");
console.log(aaa); // Uncaught ReferenceError: aaa is not defined
```

### arguments.callee 는 더이상 지원되지 않는다.

```js
function sum(a, b, c) {
  // Uncaught TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them
  "use strict";
  console.log(arguments.callee);
}

sum();
```

#### `arguments.callee`가 스트릭트 모드에서 제거된 이유

초기 버전 JavaScript는 기명(`named`) 함수 식을 허용하지 않습니다. <br />
그리고 이 때문에 재귀(`recursive`) 함수 식을 만들 수 없습니다.

[더 많은 참고 자료](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/arguments/callee)
