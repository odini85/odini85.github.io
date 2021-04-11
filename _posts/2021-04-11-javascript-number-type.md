# Number

IEEE-754 형식을 통해 정수와 부동소수점 숫자를 나타냅니다.

## IEEE-754

> IEEE 754는 IEEE에서 개발한 컴퓨터에서 부동소수점을 표현하는 가장 널리 쓰이는 표준이다. ... - [위키](https://ko.wikipedia.org/wiki/IEEE_754) -

IEEE 754에는 `32`비트 단정밀도(`single-precision`), `64`비트 배정밀도(`double-precision`), `43`비트 이상의 확장 단정밀도(거의 쓰이지 않음), `79`비트 이상의 확장 배정밀도(일반적으로 `80`비트로 구현됨)에 대한 형식을 정의하고 있다.

### 구조

![구조 예시](https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/General_floating_point_ko.svg/500px-General_floating_point_ko.svg.png)

`-118.625` 10 진법값을 `IEE 754` `32bit` 단정밀도 표현
![예시](https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Float_point_example_frac.svg/600px-Float_point_example_frac.svg.png)

## 부동소수점 표현의 문제

`0.1 + 0.2` 은 제대로 표현이 안된다.
비트로 표현되어야 하기 때문에 제대로 표현이 안w된다.

```js
(0.3).toString(2); // "0.010011001100110011001100110011001100110011001100110011"
(0.1 + 0.2).toString(2); // "0.0100110011001100110011001100110011001100110011001101"

(0.1).toString(2); // "0.0001100110011001100110011001100110011001100110011001101"
(0.2).toString(2); // "0.001100110011001100110011001100110011001100110011001101"

"0.0001100110011001100110011001100110011001100110011001101" &
  "0.001100110011001100110011001100110011001100110011001101"; // "0.0100110011001100110011001100110011001100110011001100111"

binaryToFloat("0.0100110011001100110011001100110011001100110011001100111"); // 0.30000000000000004
```

```js
function getDecimalPointBinarys(size) {
  let value = 1;
  const result = [];
  while (size > 0) {
    value /= 2;
    console.log(value);
    result.push(value);
    size--;
  }

  return result;
}
```

```js
[0.5, 0.25, 0.125, 0.0625, 0.03125, 0.015625, 0.0078125, 0.00390625, 0.001953125, 0.0009765625, 0.00048828125, ….. ]
```

```js
function binaryToFloat(binarystr) {
  const [decimal, decimalPoint] = binarystr.split(".");
  let result = parseInt(decimal, 2);
  result += decimalPoint.split("").reduce(
    (acc, current) => {
      acc.start = acc.start / 2;
      if (current === "1") {
        acc.value += acc.start;
        console.log("add", acc.start);
      }
      return acc;
    },
    { start: 1, value: 0 }
  ).value;

  return result;
}

binaryToFloat("1.111111");
```

```js
0.1 + 0.2 === 0.30000000000000004;
```

```js
((0.1 + 0.2).toString(2) === "0.0100110011001100110011001100110011001100110011001101"
```

```js
binaryToFloat((0.1 + 0.2).toString(2));
```

————

```js
(0.2 + 0.4 === 0.6000000000000001(0.2 + 0.4).toString(2)) ===
  "0.100110011001100110011001100110011001100110011001101";

binaryToFloat((0.2 + 0.4).toString(2));
```

## NaN

숫자를 반환할 것으로 의도한 조작이 실패했을 때 반환되는 값이다.

### NaN을 반환하는 5가지 연산 종류

- 숫자로 읽을 수 없음 : `parseInt("blah"), Number(undefined)`
- 결과가 허수인 수학 계산식 : `Math.sqrt(-1)`
- 피연산자가 `NaN` : `7 ** NaN`
  - `**`는 ES7 [지수 연산자](https://github.com/tc39/proposal-exponentiation-operator)로 제곱처리를 한다.
  - `7 ** 4 === Math.pow(7,4); // true`
- 문자열을 포함하면서 덧셈이 아닌 계산식 : `"가" / 3`

### NaN 판별

NaN은 비교 연산자(`==, !=, ===, !==`)로 판별이 불가능하다.<br />
다른 NaN과도 판별이 불가능하다.<br />
(`"가" / 3 === NaN; // false`)<br />

`isNaN()`, 또는 `Number.isNaN()`을 통해서 판별할 수 있다.

```js
NaN === NaN; // false
Number.NaN === NaN; // false
isNaN(NaN); // true
isNaN(Number.NaN); // true

function valueIsNaN(v) {
  return v !== v;
}
valueIsNaN(1); // false
valueIsNaN(NaN); // true
valueIsNaN(Number.NaN); // true
```

`isNaN()`과 `Number.isNaN()`의 차이를 주의해야 합니다.

#### `isNaN()`에는 치명적인 결함

- `isNaN()`은 `NaN(숫자가 아님)`을 나타낸다.
- 인자 값이 숫자인자 여부를 평가하는 기능이 전부이다.(정말 `NaN`인지 구분이 불가능하다)

```js
var a = 2 / "foo";
var b = "foo";

a; // NaN
b; // 'foo';

window.isNaN(a); // true;
window.isNaN(b); // true;
```

위 코드에서 `b`는 숫자가 아니지만 그렇다고 `NaN`은 아니므로 명백한 오류이다.

이러한 문제를 `Number.isNaN()`이 해결한다.
기존 로직에 숫자 타입을 체크하는것으로 폴리필 코드를 작성할 수 있다.

```js
Number.isNaN =
  Number.isNaN ||
  function (value) {
    return typeof n === "number" && window.isNaN(value);
  };
```

다른 폴리필 코드는 `NaN`이 `자신과 동등하지 않는 특성을 이용`하여 간단하게 처리할 수도 있다.

```js
Number.isNaN =
  Number.isNaN ||
  function (value) {
    return value !== value;
  };
```

## Number vs parseInt vs parseFloat

### 목적

- Number
  - 값을 숫자로 형변환하기 위한 목적
- parseInt, parseFloat
  - 문자열의 구문을 분석하기 위한 목적

### 입력이 숫자로 시작하고 문자가 합쳐진 경우

- Number
  - NaN
- parseInt, parseFloat
  - 숫자를 반환

### 빈문자열

- Number
  - 0
- parseInt, parseFloat
  - NaN
