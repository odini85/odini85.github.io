> [원문 - JavaScript’s Memory Model](https://medium.com/@ethannam/javascripts-memory-model-7c972cd2c239)

# 자바스크립트 메모리 모델

```js
// 변수를 선언하고 초기화 한다.
var a = 5;
let b = "xy";
const c = true;

// 새로운 값을 할당한다.
a = 6;
b = b + "z";
c = false; // TypeError: Assignment to constant variable
```

코드를 위와 같이 작성했을 때 내부적으로 어떤일이 일어나는지 알아 보도록 하겠습니다.

이 아티클은 다음 내용을 다룹니다.

1. JS 원시타입(`primitive`)에 대한 변수 선언 및 할당
1. JavaScript의 메모리 모델 : 콜 스택 및 힙
1. JS 비 원시타입(`non-primitive`) 타입 대한 변수 선언 및 할당
1. `let` vs `const`

## JS 원시값(`primitive`)에 대한 변수 선언 및 할당

아래에서 `myNumber`라는 변수를 선언하고 `23`의 값으로 초기화합니다.

```js
let myNumber = 23;
```

위 코드가 실행되면 자바스크립트는 다음과 같이 처리합니다.

1. 변수에 대한 고유 식별자(`identifier`) `myNumber`를 만듭니다.
1. 메모리에 주소를 할당합니다.(런타임에 할당)
1. 할당된 주소(`23`)에 값을 저장합니다.

<img src="/assets/images/javascript-memory-model/1.jpeg" width="500" alt="" />

`myNumber`는 `23`과 같다고 할 수 있지만,<br />
좀 더 기술적으로는 말한다면 `myNumber`는 값 `23`을 보유하는 메모리 주소와 같습니다.

비슷한 말처럼 보이지만 중요한 차이점 입니다.

`newVar`라는 새 변수를 만들고 `myNumber`를 할당해보겠습니다.

```js
let newVar = myNumber;
```

`myNumber`는 `0012CCGWH80` 주소를 갖고 있으므로 `newVar`는 `23` 값을 보유하는 메모리 주소 인 `0012CCGWH80`갖게 되므로 `newVar`는 `23`과 같습니다.

<img src="/assets/images/javascript-memory-model/2.jpeg" width="500" alt="" />

이 상태에서 아래 코드를 추가하게되면

```js
myNumber = myNumber + 1;
```

`myNumber`는 `24`의 값을 갖게 됩니다.<br />
하지만 `newVar`는 동일한 메모리 주소를 가리 키기 때문에 `24`의 값을 가질까요?

이 질문에 대한 답은 `no`입니다.<br />

JS의 원시타입은 `immutable`이므로,<br />
`myNumber + 1`는 `24`로 해석하게 되어 `JS`는 새로운 메모리 주소를 할당받고 값으로 `24`를 저장하며, `myNumber`는 새 주소를 가르키게 됩니다.

<img src="/assets/images/javascript-memory-model/3.jpeg" width="500" alt="" />

또 다른 예시는 다음과 같습니다.

```js
let myString = "abc";
myString = myString + "d";
```

초보 JS 프로그래머는 문자 `d`가 메모리에있는 문자열 `abc` 에 단순히 추가된다고 말할 수 있지만 내부적으로는 다릅니다.<br />

`abc`가 `d`와 연결되면 문자열도 JS의 원시타입이므로 새로운 메모리 주소가 할당되고 `abcd`가 여기에 저장되며 `myString`은 새로운 메모리 주소를 가르키게 됩니다.

<img src="/assets/images/javascript-memory-model/4.jpeg" width="500" alt="" />

## JavaScript의 메모리 모델 : 콜 스택 및 힙

JS 메모리 모델은 콜 스택, 힙이라는 2 가지 영역으로 설명을 하겠습니다.

<img src="/assets/images/javascript-memory-model/5.jpeg" width="500" alt="" />

콜 스택은 원시타입이 저장되는 곳입니다.(함수 호출 제외). 이전 섹션의 내용을 콜 스택으로 대략적으로 표현하면 다음과 같습니다.

<img src="/assets/images/javascript-memory-model/6.jpeg" width="500" alt="" />

위 그림에는 각 변수의 값을 표시하기 위해 메모리 주소를 추상화했습니다.<br />
하지만 실제로 변수가 값을 보유하는 메모리 주소를 가리킨다는 것을 잊지 말아야 합니다.<br />
이것은 `let` vs `const` 섹션을 이해하는 데 중요합니다.

이제 힙을 설명하겠습니다.

`힙은 원시타입이 아닌 항목이 저장되는 곳입니다.`<br />
`주요 차이점은 힙이 동적으로 증가 할 수 있는 정렬되지 않은 데이터를 저장할 수 있다는 것입니다.`<br />`(배열과 개체에 적합합니다.)`

## JS 비 원시타입(`non-primitive`) 타입 대한 변수 선언 및 할당

비 원시타입은 원시타입과 다르게 작동합니다.

간단한 예를 들어보겠습니다.<br />
아래처럼 `myArray`라는 변수를 선언하고 빈 배열로 초기화합니다.

```js
let myArray = [];
```

`myArray` 변수를 선언하고 `[]`와 같은 비 원시타입 유형을 할당하면 메모리에서 다음과 같은 일이 발생합니다.

1. 변수에 대한 고유 식별자(`identifier`) `myArray` 를 만듭니다.
1. 메모리에 주소를 할당합니다.(런타임에 할당).
1. 힙에 할당 된 메모리 주소 값을 저장합니다.(런타임에 할당).
1. 힙의 메모리 주소는 할당 된 값 (빈 배열 [])을 저장합니다.

<img src="/assets/images/javascript-memory-model/7.jpeg" width="500" alt="" />

<img src="/assets/images/javascript-memory-model/8.jpeg" width="500" alt="" />

우리는 배열에 아래와 같이 `push()`, `pop()`을 실행할 수 있습니다.

```js
myArray.push("first");
myArray.push("second");
myArray.push("third");
myArray.push("fourth");
myArray.pop();
```

<img src="/assets/images/javascript-memory-model/9.jpeg" width="500" alt="" />

## `let` vs `const`

일반적으로 `const`를 사용하고 변수가 변경 가능하다고 판단되면 `let`을 사용해야합니다.

`변경`이 무엇을 의미하는지 명확히해야 합니다.

`변화`를 값의 변화로 해석하면 안됩니다. 값의 변화로 해석한다면 아래와 같이 코드를 작성하게 됩니다.

```js
let sum = 0;
sum = 1 + 2 + 3 + 4 + 5;
let numbers = [];
numbers.push(1);
numbers.push(2);
numbers.push(3);
numbers.push(4);
numbers.push(5);
```

이 프로그래머는 값이 변경된다고 판단했기 때문에 `let`을 사용하여 `sum`을 올바르게 선언했습니다.<br />
하지만 `let`을 사용하여 `numbers`를 잘못 선언했습니다.<br />
그 이유는 배열에 `push()`하는 것을 값이 변경되었다고 생각했기 때문입니다.

올바른 `변경`에 대한 판단은 `메모리 주소를 변경`하는것 입니다.<br />
`let`은 메모리 주소를 변경할 수 있지만, 아래 코드 처럼 `const`는 메모리 주소를 변경할 수 없습니다.

```js
const importantID = 489;
importantID = 100; // TypeError: Assignment to constant variable
```

위 코드를 시각화 한다면 다음과 같습니다.

`importantID`를 선언하면 메모리 주소가 할당되고 값 `489`가 저장됩니다.<br />
`importantID` 변수는 메모리 주소와 동일하다고 생각해야합니다.

<img src="/assets/images/javascript-memory-model/10.jpeg" width="500" alt="" />

`importantID`에 `100`을 할당하면 `100`이 `원시타입`이므로 새 메모리 주소가 할당되고 `100` 값이 저장됩니다.<br />
JS는 새 메모리 주소를 `importantID`에 할당하려고 시도하고 여기에서 오류가 발생합니다.<br />
`importantID`를 변경하고 싶지 않기 때문에 코드는 원하는 방식으로 동작합니다.

<img src="/assets/images/javascript-memory-model/11.jpeg" width="500" alt="" />

위에서 언급했듯이 가상의 초보 JS 프로그래머는 `let`을 사용하여 배열을 잘못 선언했습니다.<br />
`let` 대신 `const`로 선언해야합니다.<br />
처음에는 혼란스러워 보일 수 있지만, `변경`은 메모리 주소를 기준으로 생각해야합니다.

```js
const myArray = [];
```

위와 같이 `myArray`가 선언되면 메모리 주소가 콜 스택에 할당되고 값은 힙에 할당 된 메모리 주소입니다.(힙에 저장된 값은 실제 빈 배열입니다)<br />

<img src="/assets/images/javascript-memory-model/12.jpeg" width="500" alt="" />

<img src="/assets/images/javascript-memory-model/13.jpeg" width="500" alt="" />

```js
myArray.push(1);
myArray.push(2);
myArray.push(3);
myArray.push(4);
myArray.push(5);
```

위와 같은 코드가 추가된다면.

<img src="/assets/images/javascript-memory-model/14.jpeg" width="500" alt="" />

힙에있는 배열에 데이터를 추가합니다.<br />
`myArray`가 `const`로 선언되었지만 오류가 발생하지 않는 이유는 `myArray`의 메모리 주소는 변경되지 않았기 때문입니다.<br />
`myArray`는 여전히 `0458AFCZX91`과 같으며, 힙에있는 배열의 메모리 주소는 변경없이 `22VVCX011`로 유지하고 있습니다.

다음과 같이하면 오류가 발생합니다.

```js
myArray = 3;
```

`3`은 원시타입이므로 콜 스택의 메모리 주소가 할당되고 값 `3`이 저장되고 새로운 메모리 주소를 `myArray`에 할당하려고 하지만, `myArray`는 `const`로 선언되었으므로 변경이 불가능합니다.

<img src="/assets/images/javascript-memory-model/15.jpeg" width="500" alt="" />

오류를 발생시키는 또 다른 예 :

```js
myArray = ["a"];
```

`["a"]`는 새로운 비 원시타입의 배열이므로 호출 스택의 새 메모리 주소가 할당되고 힙의 메모리 주소 값이 저장되므로 힙 메모리 주소에 저장된 값은 다음과 같습니다. `['a']`.<br />
그런 다음 호출 스택 메모리 주소를 `myArray`에 할당하려고하면 오류가 발생합니다.

<img src="/assets/images/javascript-memory-model/16.jpeg" width="500" alt="" />

배열과 같이 `const`로 선언 된 `object`의 경우 객체는 원시타입이 아니므로 키를 추가하고 값을 업데이트하는 등의 작업을 할 수 있습니다.

```js
const myObj = {};
myObj["newKey"] = "someValue"; // this will not throw an error
```
