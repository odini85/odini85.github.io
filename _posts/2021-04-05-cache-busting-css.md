# Cache CSS busting

## 조건

- 완성된 `html` 파일에서 특정 CSS의 브라우저 캐시를 제거하고 갱신한다.
- `html`이 완성된 이후의 처리 이므로 빌드시 버저닝 정보 주입은 불가능하다.
- 바벨 사용 불가로 es5문법을 사용한 구현

## 처리 방법

### 리소스 갱신

- link 태그의 리소스 경로에 버전 `querystring`을 추가하여 새로운 리소스를 다운로드 받도록 한다.

#### 단점

- link 태그의 리소스의 경로를 새로운 경로로 변경하게 되면 리소스가 다운로드되기 전 CSS 리소스가 존재하지 않아
- 잠시동안 CSS가 풀린 화면이 노출된다.

### 새로운 link태그 생성

- 리소스 갱신을 처리하기 위한 목적
- 전달받은 CSS 리소스와 매칭되는 link를 헬퍼함수(직렬화, 객체화)를 통해 새로운 CSS리소스 경로를 생성한다.
- link엘리먼트를 동적으로 생성하여 새로운 CSS리소스를 로드(append)하고
- 리소스 로드가 완료된 시점에 이전 link태그를 삭제 한다.

```js
// CSS 캐시 버스팅 - 기존 생성된 CSS파일을 리로드한다.
var purgeLinkCSS = (function () {
  var methods = {
    refresh: function (href, linkElement, path) {
      if (href.indexOf(path) > -1) {
        var querystringObj = methods.serializeToObject(href);
        var plainUrl = methods.getOmitQuerystring(href);
        if (plainUrl) {
          methods.addVersionField(querystringObj);
          var newHref = plainUrl + methods.objectToSerialize(querystringObj);
          // @FIX - 아래 깜빡임 이슈로 리소스 경로 변경은 임시 주석처리
          // linkElement.setAttribute('href', newHref)
          // var newLinkElement = document.createElement('link');

          // 깜빡임 이슈(리소스가 로드되기전 리소스 경로가 변경되어 리소스 다운로드 시간동안 스타일이 벗겨진 모습 노출) 로 인해 새로운 <link>를 생성하고 리소스 완료 시점에 이전 <link>를 제거
          var newLinkElement = document.createElement("link");
          newLinkElement.rel = "stylesheet";
          newLinkElement.href = newHref;

          document.body.appendChild(newLinkElement);
          newLinkElement.onload = function () {
            linkElement.parentElement.removeChild(linkElement);
          };
        }
      }
    },

    // 버전 추가
    addVersionField: function (querystringObj) {
      // 캐시 간격은 1분
      querystringObj.purgeversion = Math.floor(
        new Date().getTime() / 1000 / 60
      );
    },

    // 배열 검증
    isArray: function (target) {
      return Object.prototype.toString.call(target) === "[object Array]";
    },

    // 반복문
    forEach: function (arr, callback) {
      if (!arr || typeof callback !== "function") {
        return;
      }

      for (var i = 0, endi = arr.length; i < endi; ++i) {
        callback(arr[i], i, arr);
      }
    },

    // url 정보 직렬화 --> 객체
    serializeToObject: function (url) {
      var result = {};

      if (!url || typeof url !== "string") {
        return;
      }

      var querystring = url.split("?")[1];
      if (querystring) {
        var split = querystring.split("&");
        methods.forEach(split, function (item) {
          var splitItem = item.split("=");
          var key = splitItem[0];
          var value = splitItem[1];
          result[key] = value;
        });
      }

      return result;
    },

    // url 정보 객체 --> 직렬화
    objectToSerialize: function (obj) {
      var result = "?";
      if (!obj || Object.prototype.toString.call(obj) !== "[object Object]") {
        return "";
      }
      Object.keys(obj).forEach(function (key, index) {
        if (typeof obj[key] !== "undefined" && typeof obj[key] !== "function") {
          if (index > 0) {
            result += "&";
          }
          result += key + "=" + encodeURIComponent(obj[key]);
        }
      });

      return result;
    },

    // querystring을 제외한 리소스 경로 반환
    getOmitQuerystring: function (url) {
      if (!url || typeof url !== "string") {
        return;
      }

      return url.split("?")[0];
    },
  };

  // purge
  function purge(paths) {
    if (!methods.isArray(paths)) {
      return;
    }
    var links = document.querySelectorAll("link");
    methods.forEach(links, function (linkElement) {
      var href = linkElement.getAttribute("href");
      methods.forEach(paths, function (path) {
        methods.refresh(href, linkElement, path);
      });
    });
  }

  return purge;
})();

// usage e.g
purgeLinkCSS(["test/main.css"]);
```

## 추가 고려사항(리소스 갱신, 새로운 link태그 생성)

- CSS querystring이 이미 존재 한다면 기존 보유하고 있던 querystring을 유지하고 새로운 버저닝 파라미터가 추가되어야 한다.
- querystring을 유지하면서 추가하기 위한 목적으로 querystring에 대한 직렬화, 객체화 기능 필요

## Node.js를 이용한 replace

- Node.js를 이용하여
- html파일을 읽어 오고, 읽어온 파일의 내용 중 약속된 문자열을 replace 처리 후
- html파일을 덮어 씌운다.

```js
var fs = require("fs");

fs.readFile("읽어올파일", "utf-8", function (err, data) {
  if (err) {
    console.error("read failed: ", err);
  }

  // <%= version %>으로 삽입된 내용을 타임스템프로 변환한다.
  const result = data.replace(/<%= version %>/gi, new Date().getTime());

  // 변환 후 생성될 파일
  fs.writeFile("생성할 파일", result, "utf8", function (err) {
    if (err) {
      console.error("write failed: ", err);
    }
  });
});
```
