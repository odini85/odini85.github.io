# prevent scroll chain

- 스크롤이 중첩된 UI에서 자식 스크롤이 끝(시작/종료)에 도달 한 경우
- 스크롤 이벤트 전파가 바닥에 전달되는 동작을 막기위한 목적
  - 보통 body태그에 overflow:hidden을 처리하거나
  - [overscroll-behavior](https://developer.mozilla.org/en-US/docs/Web/CSS/overscroll-behavior)을 이용 할 수 있음
    - 이 속성은 ios 에서 정상동작을 보장하기 어렵다.
  - 스크롤 체인의 이벤트 전파을 막기 위해 스크이 시작/종료에 도달한 경우 scrollTop값을 재 보정하여 이벤트 전파를 막는다.

```js
function scrollChainLock(scrollElement) {
  if (!(scrollElement instanceof Element)) {
    throw Error("scrollElement는 엘리먼트만 가능합니다.");
  }

  var scrollHandler = function () {
    if (scrollElement.scrollTop === 0) {
      scrollElement.scrollTop = 1;
    }

    if (
      scrollElement.scrollHeight - scrollElement.scrollTop ===
      scrollElement.clientHeight
    ) {
      scrollElement.scrollTop -= 1;
    }
  };

  scrollElement.addEventListener("scroll", scrollHandler);

  return {
    destroy: function () {
      scrollElement.removeEventListener("scroll", scrollHandler);
    },
  };
}

scrollChainLock(document.querySelector("#layer1"));
```

#### 참고 링크

- [https://developers.google.com/web/updates/2017/11/overscroll-behavior](https://developers.google.com/web/updates/2017/11/overscroll-behavior)
- [예제](https://codepen.io/uicoder/pen/abBqObQ)
