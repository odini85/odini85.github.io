# CSS만으로 이미지 비율 적용

- 가로 width에 대한 세로 높이 가변 비율 적용

- 5:2 비율(width: 328px, height: 132px;)의 요구 조건에서

> 가로 폭은 가변으로 설정하되 높이는 5:2비율에 맞춰 늘어나야 한다

## 높이의 비율 적용은 padding-top으로

- `padding-top`을 이용하면 높이값이 가로 비율에 맞춰 증가한다.
- 비율의 값이기 때문에 `%`값이어야 하는 조건을 만족해야 한다.

## 비율 계산 공식

- 제공된 디자인 파일 기준의 가로/세로 사이즈로 비율을 가져오는 공식은 다음과 같다.

```
(132 / 328) * 100; // 40.243902439024396
```

- 위 결과로 계산된 값을 `padding-top`로 설정한다.

## calc를 이용한 비율 설정

- `padding-top`에 계산된 값을 추가하는 부분은 동일하다.
- calc에 대한 부분은 [MDN Calc()](<https://developer.mozilla.org/ko/docs/Web/CSS/calc()>) 참고

```css
calc(100% * (132 / 328)
```

## 이미지는 부유 요소가 되어야 한다.

- `padding-top` 속성을 갖는 것은 이미지가 `padding-top` 값을 갖고 있어야 한다는 조건과 동일하다.
- 이미지가 `padding-top`을 갖고 있다면 사이즈를 적용 했을 때 문제가 발생할 수 있다.(padding-top을 감안하여 css가 적용 되어야 한다.)
- 이런 문제를 위해 이미지를 감싸는 컨테이너가 필요하다.
- 이미지는 컨테이너 기준(relative)으로 절대 좌표(absolute)로 설정한다.
- 이미지의 적절한 위치 표현 방법은 여러 방법이 있을 수 있다.

### object-fit: cover | container 등을 이용한 방법 [MDN object-fit](https://developer.mozilla.org/ko/docs/Web/CSS/object-fit)

```css
position: aboslute;
top: 0;
left: 0;
object-fit: cover | container;
```

### transform를 이용한 방법 [MDN transform](https://developer.mozilla.org/ko/docs/Web/CSS/transform)

```css
position: aboslute;
top: 50%;
left: 50%;
transform: translate(-50%, -50%);
```

## HTM, CSS

### HTML

```html
<div class="image">
  <img src="http://web.pdx.edu/~allstott/hypebeasthistory/images/OWair1.jpg" />
</div>
```

### CSS

```css
/* padding-top에 calc()로 계산된 값 입력 */
.image {
  position: relative;
  padding-top: calc((132 / 328) * 100);
}

.image img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

```css
/* padding-top에 퍼센트를 직접 입력 */
.image {
  position: relative;
  padding-top: 40.2439%; /* 132/328 * 100 */
}
.image img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

## calc 크로스 브라우징

- [caniuse Calc](https://caniuse.com/?search=calc)
- IE 11에서 일부 오동작 하는 것으로 생각되기 때문에 크로스 브라우징 범위를 감안하여 적용여부를 결정한다.

<img src="/assets/images/image-ratio-height/caniuse-calc.png" alt="caniuse Calc" style="max-width:800px;" /><br />

## 참조

- [MDN Calc()](<https://developer.mozilla.org/ko/docs/Web/CSS/calc()>)
- [MDN object-fit](https://developer.mozilla.org/ko/docs/Web/CSS/object-fit)
- [MDN transform](https://developer.mozilla.org/ko/docs/Web/CSS/transform)
