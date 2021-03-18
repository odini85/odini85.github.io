# 토글 속도 비교 - Not Boolean vs XOR Bitwise

## React Funtional Component forceUpdate를 구현

- 함수 컴포넌트를 사용할 때 강제 forceUpdate를 실행하기 위해 흔히 사용되는 코드는 다음과 같다.

```js
// forceUpdate.ts
import { useState } from "react";

const useForceUpdate = () => {
  const [value, setValue] = useState(false);

  return () => setValue(!value);
};

export default useForceUpdate;
```

```ts
// 사용 컴포넌트
import useForceUpdate from './forceUpdate';

const TestComponent: React.FC<Props> = () => {
  ....

  // 원하는 시점에 forceUpdate()를 실행한다.
  const forceUpdate = useForceUpdate();

  ....
};
```

- 위 코드는 forceUpdate()가 실행될 때 마다 state의 값을 not boolean으로 처리하여 컴포넌트를 재 랜더링 시킨다.
- 비트 연산의 XOR을 이용한다면 이와 비슷하게 처리할 수 있다.

## XOR 어떻게 동작하나?

```js
const a = 5; // 00000000000000000000000000000101
const b = 3; // 00000000000000000000000000000011

console.log(a ^ b); // 00000000000000000000000000000110
```

- XOR 로 a, b값을 연산한다면 a와 b의 동일한 비트 자리가 서로 다른 경우에만 1로 변경된다.

## XOR을 이용한 값 토글.

- 위 동작 방식을 통해 `서로 값이 다른 경우에 1이 된다`는 것을 알 수 있었다.
- 이런 특성을 이용한다면 XOR을 이용한 값 토글이 가능하다.

```js
0 ^ 1; // 1
1 ^ 1; // 0
0 ^ 1; // 1
```

- 위 코드는 이전 결과를 첫번째 비트 연산 값으로 사용하는 경우에 대한 예시 코드이다.

- 아래 코드는 위 코드를 기반으로 작성해본 테스트 코드
- 10번 토글이 반복되는 테스트 코드 이다.

```js
// 토글 확인용 테스트 코드
var i = 10;
var flag = 0;
while(i){
 flag = flag ^ 1;
 console.log(flag);
 --i;
}
/* 위 코드의 결과는 아래와 같다 */
VM2726:6 1
VM2726:6 0
VM2726:6 1
VM2726:6 0
VM2726:6 1
VM2726:6 0
VM2726:6 1
VM2726:6 0
VM2726:6 1
VM2726:6 0
```

### 이걸 forceUpdate에 적용 해보자.

```ts
// forceUpdate.ts
import { useState } from "react";

const useForceUpdate = () => {
  const [value, setValue] = useState(0);

  return () => setValue(value ^ 1); // update the state to force render
};

export default useForceUpdate;
```

## 속도 차이는 얼마나 날까?

- 아래 코드는 `not boolean, xor` 각 각 1천만 번 토글을 시키고
- 1천만번을 세트로 1000번만큼 반복했을 때 걸리는 값에 대한 결과를 비교해 보았다.

## 테스트 코드

```js
var limit = 10000000;
function testBoolean() {
  var flag = true;
  var start = Date.now();
  var i = limit;
  while (i) {
    // not
    flag = !flag;
    --i;
  }
  var end = Date.now();
  var result = end - start;
  return result;
}

function testBit() {
  var flag = 0;
  var start = Date.now();
  var i = limit;
  while (i) {
    // XOR
    flag = flag ^ 1;
    --i;
  }
  var end = Date.now();
  var result = end - start;
  return result;
}

var i = 0;
var r = { boolean: [], bit: [] };
while (i < 1000) {
  var rboolean = testBoolean();
  var rbit = testBit();
  r.boolean.push(rboolean);
  r.bit.push(rbit);
  ++i;
}

r.bit.total = r.bit.reduce((a, c) => a + c, 0);
r.boolean.total = r.boolean.reduce((a, c) => a + c, 0);
r["bit-boolean"] = r.bit.total - r.boolean.total;
console.log("end!!", r);

/* 결과 
*
{
    bit: (1000) [10, 10, 6, …]
    bit-boolean: -2632
    boolean: (1000) [11, 9, 8, …]
}
*/
```

- 보는 바와 같이 약 1천만번을 1천번 세트로 확인한 결과 bit연산을 이용한 결과가 `2632 ms`만큼 빠르게 확인이 된다.
- `XOR`가 더 빨라 보이기는 하는데.. 엄청난 차이로는 느껴지지가 않는다.(사실 토글하는 횟수가 많은것도 아니고)
- 체감하는 속도 차가 크지 않지만 뭔가 있어 보이기는(?)하다 ㅋㅋ
