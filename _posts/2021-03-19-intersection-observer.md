# Intersection Observer

## 개요

타겟 요소, 상위 요소, document 의 viewport 사이의 intersection 내의 변화를 비동기적으로 관찰하는 방법

### 어디에 사용?

- 이미지 레이지 로딩
- 무한 스크롤
- 광고 노출 분석

### 과거에는 이런방법으로

- 이벤트 발생시 `Element.getBoundingClintRect()`를 반복 호출하여 감지

### Intersection Observer는 정확한 픽셀 겹침 정보를 알려주지 않음.

- 정확히 몇 픽셀이 쳤는지 알려주지 Intersection Observer는 알려줄 수 없다.
- 단 n % 정도 교차하는 경우 사용 가능하다.

## Intersection Observer 사용

### 인스턴스 생성

```js
const options = {
  root: document.querySelector("#scrollArea"),
  rootMargin: "0px",
  threshold: 1.0,
};

const observer = new IntersectionObserver(callback, options);
```

### 콜백 함수 호출 시점

- 대상 엘리먼트가 특정 요소 또는 기기의 뷰포트에 교차 할 때
- observer가 최초로 타겟을 관측하도록 요청받을 때

### 옵션

- root
  - 대상 객체의 가시성을 확인할 때 사용되는 뷰포트 요소
  - target의 조상 요소여야 함.
  - document를 루트로 지정하고 싶다면 null 또는 해당 속성을 지정하지 않음
- rootMain
  - root가 가진 여백 margin 속성과 유사함
    - e.g) `rootMarin: "10px 20px 30px 40px"`
- threshold
  - observer의 콜백이 실행될 대상(target) 요소의 가시성 퍼센트
  - 단일 숫자 혹은 배열로 설정 가능
  - 값
    - 0(기본 값) : 대상 요소가 1픽셀이라도 노출되면 콜백 실행
    - 1 : 요소의 모든 픽셀이 화면에 노출되기 전까지 실행시키지 않음
    - 0.5 : 50%만큼 대상 요소가 노출되면 콜백 실행
    - 여러 값 도 설정 가능
      - `threshold: [0, 0.25, 0.5, 0.75, 1]`

## 관찰 요소 타겟팅

```js
// 옵션
const options = {
  root: document.querySelector("#scrollArea"),
  rootMargin: "0px",
  threshold: 1.0,
};

// 옵저버 생성
const observer = new IntersectionObserver(() => {
  // 실행될 콜백
}), options);

// 타겟 설정
const target = document.querySelector("#listItem");
observer.observe(target);
```

### 콜백

- 대상 요소가 threshold의 조건에 맞을 때 호출
- 인자로 entries, observer를 받음
- 콜백은 메인 스레드에서 실행되므로 무거운 작업이 들어가면 안됨
- 시간이 많이 걸리는 작업을 수행하려면 [window.requestIdleCallback](https://developer.mozilla.org/ko/docs/Web/API/Window/requestIdleCallback)을 사용

```js
// window.requestIdleCallback 테스트 예제

var start = Date.now();
function calc() {
  return Date.now() - start;
}

console.log("전체 코드 시작", calc());
requestIdleCallback(() => {
  console.log("requestIdleCallback 시작", calc());
  var i = 0;
  while (i < 2000000000) {
    ++i;
  }
  console.log("requestIdleCallback 끝", calc());
});

requestAnimationFrame(() => {
  console.log("requestAnimationFrame");
});

setTimeout(() => {
  console.log("timeout");
}, 0);

queueMicrotask(() => {
  console.log("queueMicrotask");
});

requestAnimationFrame(() => {
  console.log("loop requestAnimationFrame 시작", calc());
  var i = 0;
  while (i < 2000000000) {
    ++i;
  }
  console.log("loop requestAnimationFrame 끝", calc());
});

queueMicrotask(() => {
  console.log("loop queueMicrotask 시작", calc());
  var i = 0;
  while (i < 2000000000) {
    ++i;
  }
  console.log("loop queueMicrotask 끝", calc());
});

setTimeout(() => {
  console.log("loop setTimeout 시작", calc());
  var i = 0;
  while (i < 2000000000) {
    ++i;
  }
  console.log("loop setTimeout 끝", calc());
});

console.log("전체코드 끝", calc());
```

#### 콜백 인자 : `entries`(읽기 전용 속성)

- entries
  - IntersectionObserver entry 인스턴스 배열
- boudingClientRect
  - 관찰 대상의 사격형 정보
  - Element.getBoundingClientRect()와 동일
- intersectionRect
  - 관찰 대상의 교차한 영역 정보
- intersectionRatio
  - 관찰 대상이 루트 요소와 겹치는 수치(비율)
  - 0.0 ~ 1.0 사이의 값
- isIntersecting
  - 관찰 대상의 교차 상태 여부
  - true: 교차중, false: 교차중 아님
- rootBounds
  - 지정한 루트 요소의 사각형 정보
- target
  - 관찰 대상 요소
- time
  - 변경이 발생한 시간 정보

## Polyfill

[https://github.com/w3c/IntersectionObserver/tree/main/polyfill](https://github.com/w3c/IntersectionObserver/tree/main/polyfill)

## 예

- [무한스크롤](https://codepen.io/uicoder/pen/OJbBMRb)
- [영상 재생](https://codepen.io/uicoder/pen/jOVeWBY)

## 참고

- [https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API](https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API)
- [https://heropy.blog/2019/10/27/intersection-observer/](https://heropy.blog/2019/10/27/intersection-observer/)
