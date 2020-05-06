> 참고
>
> - [https://developers.google.com/web/tools/chrome-devtools/javascript?hl=ko](https://developers.google.com/web/tools/chrome-devtools/javascript?hl=ko)
> - [https://ko.javascript.info/debugging-chrome](https://ko.javascript.info/debugging-chrome)

## 1단계 : 버그 재현

버그를 생성하는 부분을 찾는것은 디버깅의 첫단계입니다.

## 2단계 : 소스 패널 UI 익히기

DevTools은 CSS 변경, 페이지 로드 성능 프로파일링, 네트워크 요청 모니터링과 같은 다양한 작업을 위한 다양한 도구를 제공합니다.<br />
Source 패널은 자바스크립트를 디버그하는 곳입니다.

1. `Command+Option+I(Mac)` 또는 `Control+Shift+I(Windows, Linux)`를 눌러서 DevTools를 엽니다. 이 단축키는 Console 패널을 엽니다.
1. `Sources` 탭을 클릭합니다.
   - Sources 패널 UI는 3개 부분으로 나뉩니다.<br />
     <img src="https://developers.google.com/web/tools/chrome-devtools/javascript/imgs/sources-annotated.png?hl=ko" style="max-width: 500px">
   - `File Navigator`
     - 해당 페이지가 요청하는 모든 파일이 여기에 나열됩니다.
   - `Code Editor`
     - File Navigator 창에서 파일을 선택한 후, 해당 파일의 콘텐츠가 여기에 표시됩니다.
   - `JavaScript Debugging`
     - 페이지의 자바스크립트를 검사하는 다양한 도구입니다. DevTools 창이 넓다면 이 창이 Code Editor 창의 오른쪽에 표시됩니다.

## 3단계 : 중단점(break point)으로 코드 일시 중지

console.log() 메서드로도 작업을 완료할 수는 있지만,<br />
중단점을 이용하면 실행 도중에 코드를 일시 중지하고 해당 시점의 모든 변수 값을 검사할 수 있습니다.

console.log보다 나은 점

- console.log()를 코드에 삽입하고, 새로고침으로 확인해야 합니다.
  - 중단점을 이용하면 코드의 구조를 모르더라도 원하는 부분에서 일시 중지할 수 있습니다.
- console.log() 문에서 검사하고 싶은 각 값을 명시적으로 작성해야 합니다.
  - 중단점을 이용하여 DevTools는 해당 시점의 모든 변수의 값을 확인가능합니다.

[예제](https://googlechrome.github.io/devtools-samples/debug-js/get-started)의 `click` 리스너가 실행되는 시점에 코드를 일시 중지하고 싶을 경우 아래의 순서로 `Event Listener Breakpoints`를 이용할 수 있습니다.

<img src="/assets/images/get-started-click-breakpoint.png?hl=ko" style="max-width:800px;" />

1. JavaScript Debugging 창에서 Event Listener Breakpoints를 클릭하여 섹션을 펼칩니다.
1. Mouse 이벤트 카테고리를 펼쳐서 click항목을 체크합니다.
   - Mouse click항목이 체크 되었으므로 모든 click이벤트가 실행될때 자동 중단점이 잡힙니다.
1. 다른 코드 줄에서 일시 중지된다면 원하는 줄에서 중단될때까지 `Resume Script Execution` <img src="https://developers.google.com/web/tools/chrome-devtools/images/resume-script-execution.png?hl=ko" width="26" height="20" /> 누릅니다.

## 4단계 : 단계별 코드 실행

일반적으로 버그는 스크립트를 잘못된 순서로 실행했을 때 발생합니다.<br />
코드를 단계별로 실행하면 코드 실행을 한 번에 한 줄씩 따라가면서 기대한 것과 다른 순서로 실행되는 곳이 어디인지 알아낼 수 있습니다.

- `Step`
  - 다음 문을 실행합니다.
- `Step over next function call`
  - 다음 명령어를 실행하되, 함수 안으로 들어가지 않습니다
  - 함수 호출 시 내부에서 어떤 일이 일어나는지 궁금하지 않을 때 유용합니다.
- `Step into next function call`
  - Step과 유사하지만 비동기 함수 호출시 다르게 동작합니다.
  - Step은 비동기 동작을 무시합니다.
  - Step into는 비동기 동작을 담당하는 코드로 진입하고, 필요하다면 비동기 동작이 완료될 때까지 대기합니다.
- `Step out of current function`
  - 실행 중인 함수의 실행이 끝날 때 까지 실행을 계속합니다.
  - 실수로 Step 을 눌러 내부 동작을 알고 싶지 않은 중첩 함수로 진입했거나 가능한 한 빨리 함수 실행을 끝내고 싶은 경우 유용합니다.
- Deactivate breakpoints
  - 모든 중단점을 일시적으로 활성화/비활성화합니다(실행에는 영향이 없습니다).
- `Pause in exceptions`
  - 예외 발생 시 코드를 자동 중지시켜주는 기능을 활성화/비활성화 합니다.
  - 활성화되어 있고, 개발자 도구가 열려있는 상태에서 스크립트 실행 중에 에러가 발생하면 실행이 자동으로 멈춥니다.
  - 개발하다가 에러와 함께 스크립트가 죽었다면 디버거를 열고 이 옵션을 활성화한 후, 페이지를 새로 고침하면 에러가 발생한 곳과 에러 발생 시점의 컨텍스트를 확인할 수 있습니다.

## 5단계 : 코드 줄 중단점 설정

<img src="https://developers.google.com/web/tools/chrome-devtools/javascript/imgs/line-of-code-breakpoint.png?hl=ko" style="max-width: 800px;">

코드 줄 중단점(Line-of-code breakpoints)은 가장 일반적인 유형의 중단점입니다.<br />
일시 중지하고 싶은 코드 줄이 있다면 코드 줄 중단점을 사용하세요.

## 6단계 : 변수 값 확인

### 방법 1. Scope 창

코드 줄에서 일시 중지하면 `Scope` 창에 현재 정의된 로컬 및 전역 변수가 무엇인지 각 변수의 값과 함께 표시합니다.<br />
가까운 변수가 있는 경우에는 이것도 표시합니다. 변수 값을 편집하려면 더블클릭합니다.

### 방법 2: Watch Expressions

`Watch` 창의 `+`버튼으로 표현식을 추가하여 결과를 확인할 수 있습니다.

### 방법 3: Console

console.log()를 이용해 원하는 구문을 평가할 수 있습니다.<br />
Console 창이 안보인다면, `Esc`를 눌러 열 수 있습니다.<br />

중단점에서 접근이 가능한 변수라면 console.log()를 이용하여 확인가능합니다.

## 7단계 : 수정 내용 적용

수정내용 반영!
