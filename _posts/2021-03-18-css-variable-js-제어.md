# CSS Variable을 Javascript로 제어

- CSS 변수를 사용하기 위한 `var()`
- `var()`를 통해 사용하는 CSS변수의 값은 자바스크립트를 통해 생성/변경 가능하다.
- 아래 코드는 `--vh` 라는 CSS 변수를 2번 째 인자로 전달한 값으로 설정하는 코드이다.

```js
document.documentElement.style.setProperty("--vh", `${get1vh()}px`);
```

- javascript를 통해 변경된 값은 CSS가 리엑티브하게 변경된 값으로 화면에 반영된다.

## 응용

- 위 특성을 이용 한다면 아래와 같은 문제의 해결방법으로 사용 할 수 있다.
- 100vh를 CSS에서 사용할 때 주소 입력창의 높이 때문에 100vh가 정상동작하지 않는 문제의 해결방법으로 사용할 수 있다.

### 예제 코드

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      .layer {
        position: fixed;
        top: 0;
        width: 20vw;
        box-sizing: border-box;
        border: 5px solid lightblue;
      }
      .layer1 {
        left: 0vh;
        height: 100vh;
        background: red;
      }
      .layer2 {
        left: 25vw;
        // :root에 정의된 변수 --vh를 가져온다.
        // var(--vh, 1vh)는 fallback기능으로 --vh가 없다면 1vh를 사용 하겠다는 의미
        height: calc(var(--vh, 1vh) * 100);
        background: orange;
      }
      .layer3 {
        left: 50vw;
        height: 100%;
        background: sandybrown;
      }
      .bg {
        height: 500vh;
        background: silver;
      }
      .size {
        position: fixed;
        left: 0;
        top: 45%;
        z-index: 1;
        font-size: 40px;
      }
    </style>
  </head>
  <body>
    <div class="size"></div>
    <div class="bg"></div>
    <div class="layer layer1">layer1</div>
    <div class="layer layer2">layer2</div>
    <div class="layer layer3">layer3</div>
    <script>
      function get1vh() {
        // 1vh는 뷰포트 화면의 height 값의 1%를 나타내기 때문에 innerHeight * 0.01을 통해 1vh값을 계산한다.
        return window.innerHeight * 0.01;
      }

      function setCss1vh() {
        document.documentElement.style.setProperty("--vh", `${get1vh()}px`);
      }

      function renderWinInnerHeightSize() {
        document.querySelector(".size").innerHTML = `${window.innerHeight}px`;
      }
      setCss1vh();
      renderWinInnerHeightSize();

      // ios safari를 예로 스크롤 시 주소창이 숨겨지거나 노출되는경우 resize이벤트가 발생한다.
      // resize 이벤트를 감지하여 CSS --vh 변수를 재계산 한다.
      window.addEventListener("resize", function () {
        renderWinInnerHeightSize();
        setCss1vh();
      });
    </script>
  </body>
</html>
```

### 참조

- https://css-tricks.com/the-trick-to-viewport-units-on-mobile/
