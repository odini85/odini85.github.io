> 참고
> [https://developers.google.com/web/tools/chrome-devtools/javascript/breakpoints?hl=ko](https://developers.google.com/web/tools/chrome-devtools/javascript/breakpoints?hl=ko)

일반적으로 코드 줄을 중단점으로 이용하지만 코드 줄은 어느 부분이 문제가 되는지 모르거나, 큰 규모의 코드 베이스에서 비효율적입니다.

## 중단점 종류

| 유형          | 일시 중지하고 싶은 지점                             |
| ------------- | --------------------------------------------------- |
| 코드 줄       | 정확한 코드 영역.                                   |
| 조건부 코드줄 | 정확한 코드 영역(몇 가지 다른 조건이 참일 때에만).  |
| DOM           | 정확한 코드 영역(몇 가지 다른 조건이 참일 때에만).  |
| XHR           | XHR URL이 문자열 패턴을 보유할 때                   |
| 이벤트 리스너 | 이벤트 후에 실행되는 click과 같은 코드가 실행될 때. |
| 예외          | 포착 또는 미포착 예외를 생성하는 코드 줄.           |
| 함수          | 특정 함수가 호출될 때마다.                          |

## 코드 줄 중단점

<img src="/assets/images/chrome-devtools-js-pause-breakpoints/loc-breakpoint.png" alt="코드 줄 중단점 예시" style="max-width:800px;">

조사해야 하는 정확한 코드 영역을 알고 있을 때 코드 줄 중단점을 사용합니다.<br />
DevTools은 항상 이 코드 줄이 실행되기 전에 일시 중지합니다.

설정방법

1. Sources 탭을 클릭합니다.
1. 중단하고 싶은 코드 줄이 담긴 파일을 엽니다.
1. 해당 코드 줄로 이동합니다.
1. 코드 줄의 왼쪽에 줄 번호 열이 있습니다. 이것을 클릭합니다. 파란 아이콘이 줄 번호 열 위에 나타납니다.

## 코드 내 코드 줄 중단점

일시 중지할 줄에서 debugger를 호출합니다.<Br />
이 방법은 코드 줄 중단점과 동일하게 동작합니다.

```js
console.log("a");
console.log("b");
debugger; // 줄 중단점 설정
console.log("c");
```

## 조건부 코드 줄 중단점

<img src="/assets/images/chrome-devtools-js-pause-breakpoints/conditional-loc-breakpoint.png" alt="조건부 코드 줄 중단점 예시" style="max-width:800px;" />

조사할 정확한 코드 영역을 알고 있고, 특정 조건인 경우에만 줄 중단점을 설정하기 위해 사용합니다.

설정방법

1. Sources 탭을 클릭합니다.
1. 중단하고 싶은 코드 줄이 담긴 파일을 엽니다.
1. 해당 코드 줄로 이동합니다.
1. 코드 줄의 왼쪽에 줄 번호 열이 있습니다. 오른쪽 클릭합니다.
1. Add conditional breakpoint를 선택합니다.
1. 조건을 대화상자에 입력합니다.
   ```js
   // 아래와 같은 조건을 추가하면 sum이 1000보다 큰경우 줄 중단점이 활성화됩니다.
   sum > 1000;
   ```
1. Enter를 눌러 중단점을 활성화합니다. 주황색 아이콘이 줄 번호 열 위에 나타납니다.

## 코드 줄 중단점 관리하기

<img src="/assets/images/chrome-devtools-js-pause-breakpoints/breakpoints-pane.png" alt="코드 줄 중단점 관리하기 예시" style="max-width:800px;" />

`Breakpoints` 창을 이용하여 한 위치에서 코드 줄 중단점을 비활성화하거나 삭제할 수 있습니다.

- 체크박스를 통해 비활성화/활성화할 수 있습니다.
- 오른쪽 버튼을 클릭하여 삭제 등을 처리할 수 있습니다.
- 비활성화된 중단점은 코드 줄 표시에서 흐리게 표현됩니다.

## DOM 변경 중단점

<img src="/assets/images/chrome-devtools-js-pause-breakpoints/dom-change-breakpoint.png" alt="DOM 변경 중단점 예시" style="max-width:800px;" />

DOM 노드 또는 그 하위 요소를 변경하는 코드에서 일시 중지하고 싶을 때 DOM 변경 중단점을 사용합니다.

설정방법

1. `Elements` 탭을 클릭합니다.
1. 중단점을 설정하고 싶은 요소로 이동합니다.
1. 요소를 오른쪽 클릭합니다.
1. `Break on`으로 마우스를 가져간 다음 `Subtree modifications, Attribute modifications, Node removal`을 선택합니다.

### DOM 변경 중단점의 유형

- `Subtree modifications.`
  - 현재 선택한 노드의 하위 요소가 삭제 또는 추가되거나 하위 요소의 콘텐츠가 변경될 때 트리거 됩니다.
  - 하위 요소 노드의 속성이 변경되거나 현재 선택한 노드가 변경될 때는 트리거 되지 않습니다.
- `Attributes modifications`
  - 현재 선택한 노드에 속성을 추가 또는 삭제하거나 속성 값이 변경될 때 트리거 됩니다.
- `Node Removal`
  - 현재 선택된 노드가 삭제될 때 트리거 됩니다

## XHR/Fetchbreakpoints

<img src="/assets/images/chrome-devtools-js-pause-breakpoints/xhr-breakpoint.png" alt="XHR/Fetchbreakpoints 예시" style="max-width:800px;" />

(위 예제는 `org`문자열을 포함하는 URL에 대한 XHR 중단점을 생성합니다.)

XHR의 요청 URL에 특정 문자열이 포함된 경우 중단점을 적용하고 싶을때 사용합니다.<br />
DevTools은 XHR이 send()를 호출하는 코드 줄에서 일시 중지합니다.

> 참고: 이 기능은 Fetch 요청에도 작동합니다.

이 방법은 올바르지 않은 요청을 일으키는 AJAX나 Fetch 소스 코드를 찾고 싶을때 유용합니다.

설정방법

1. Sources 탭을 클릭합니다.
1. XHR Breakpoints 창을 펼칩니다.
1. Add breakpoint를 클릭합니다.
1. 중단하고 싶은 문자열을 입력합니다. DevTools는 이 문자열이 XHR의 요청 URL 내에 존재하면 일시 중지합니다.
1. Enter를 눌러 확인합니다.

## 이벤트 리스너 중단점

<img src="/assets/images/chrome-devtools-js-pause-breakpoints/event-listener-breakpoint.png" alt="이벤트 리스너 중단점 예시" style="max-width:800px;" /><br />

이벤트가 발생했을때 실행되는 리스너 코드에 중단점을 추가하고 싶을때 사용합니다.<br />
click과 같은 특정 이벤트 또는, 모든 마우스 이벤트(포괄적인 범주의 이벤트)를 선택할 수 있습니다.

설정방법

1. Sources 탭을 클릭합니다.
1. Event Listener Breakpoints 창을 펼칩니다. DevTools이 Animation과 같은 이벤트 카테고리 목록을 표시합니다.
1. 해당 카테고리의 이벤트가 실행될 때 일시 중지하고 싶은 카테고리를 하나 선택하거나, 카테고리를 펼쳐 특정 이벤트를 선택합니다.

## 예외 중단점

<img src="/assets/images/chrome-devtools-js-pause-breakpoints/uncaught-exception.png" alt="예외 중단점 예시" style="max-width:800px;" /><br />

예외 발생시 중단점을 적용하기위해 사용합니다.

설정방법

1. Sources 탭을 클릭합니다.
1. Pause on exceptions 예외에서 `Pause on exceptions`을 클릭합니다{
   - 활성화되면 파란색으로 변합니다.
1. (옵션) 미포착 예외와 더불어 포착 예외에도 일시 중지하고 싶다면 `Pause On Caught Exceptions` 체크박스를 선택합니다.

## 함수 중단점

특정 함수가 호출될 때마다 중단점을 적용할때 사용합니다.<br />
`debug(functionName)`로 설정합니다.<br />
`functionName`은 디버그하고 싶은 함수입니다.

```js
// debug() 사용 예
function sum(a, b) {
  let result = a + b; // DevTools pauses on this line.
  return result;
}
debug(sum); // Pass the function object, not a string.
sum();
```

> DevTools은 디버그하려는 함수가 범위 내에 없으면 `ReferenceError`를 생성합니다.

```js
(function () {
  function hey() {
    console.log("hey");
  }
  function yo() {
    console.log("yo");
  }
  debug(yo); // This works.
  yo();
})();
debug(hey); // This doesn't work. hey() is out of scope.
```

DevTools Console에서 debug()를 사용하고 싶은경우 대상 함수가 범위 내에 있는지 확인하는 것이 어려울 수 있습니다.<br />
이러한 경우 다음과 같이할 수 있습니다.

1. 함수가 범위인 영역 어딘가에 코드 줄 중단점을 설정합니다.
1. 중단점을 트리거합니다.
1. 코드가 코드 줄 중단점에서 아직 일시 중지된 동안 DevTools 콘솔에서 debug()를 호출합니다.
