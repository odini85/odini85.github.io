> 참고
> [https://developers.google.com/web/tools/chrome-devtools/network?hl=ko](https://developers.google.com/web/tools/chrome-devtools/network?hl=ko)

## 네트워크 패널을 사용하는 경우

일반적으로 네트워크 패널을 사용하는 경우는 아래와 같습니다.

- 리소스가 업로드/다운로드되고 있는지 확인이 필요한 경우
- HTTP header, content, size 등과 같은 개별 리소스검사가 필요한 경우

## 네트워크 패널 열기

[데모](https://devtools.glitch.me/network/getstarted.html)를 이용하여 네트워크 패널 기능을 확인합니다.

1. [데모](https://devtools.glitch.me/network/getstarted.html) 페이지를 엽니다.
1. `Control + Shift + J` 또는 `Command + Option + J (Mac)`를 눌러 DevTools를 엽니다.
1. 네트워크 탭을 클릭하여 네트워크 패널을 엽니다.

## 네트워크 로그 확인

페이지에서 발생하는 네트워크 로그 확인방법

<img src="/assets/images/chrome-devtools-network/log.png" alt="네트워크 활동 로그 기록" style="max-width:800px" />

1. 페이지를 새로고침합니다.(네트워크 패널에 모든 네트워크 활동 로그가 기록됩니다)

   - 네트워크 로그의 각 행은 리소스를 나타냅니다.
   - 리소스는 시간순으로 나열됩니다.
   - 최상위 리소스는 보통 기본 HTML 문서입니다.
   - 최하위 자원은 마지막으로 요청 된 것입니다.

   - 각 열은 자원에 대한 정보를 나타냅니다.

   - Status
     - HTTP response 코드입니다.
   - Type
     - 리소스 타입입니다.
   - Initiator
     - 자원을 요청한 원인
     - Initiator 열에서 링크를 클릭하면 요청을 발생시킨 소스 코드로 이동합니다.
   - Time
     - 요청 소요 시간
   - Waterfall
     - 여러 요청단계를 UI로 표현한 것입니다.
     - waterfall에 마우스를 올리면 break down이 표시됩니다.

1. DevTools가 열려있는 동안 네트워크 로그가 기록됩니다.
1. 데모링크에서 [Get Data]버튼을 클릭하세요.
1. 네트워크 로그에 getstarted.json이라는 새로운 리소스가 추가됩니다.

## 더 많은 정보표시

<img src="/assets/images/chrome-devtools-network/domain.png" alt="도메인 항목 추가 예시" style="max-width: 800px;" />

네트워크 로그의 열을 구성 할 수 있습니다.<br />
사용하지 않는 열을 숨길 수 있으며 숨겨져있는 유용한 열이 많이 있습니다.

네트워크 로그 테이블의 헤더를 마우스 오른쪽 버튼을 클릭하고 도메인을 선택하면 각 자원의 도메인이 표시됩니다.

## 느린 네트워크 연결 시뮬레이션

<img src="/assets/images/chrome-devtools-network/slow3g.png" alt="느린 네트워크 연결 시뮬레이션 예시" style="max-width: 800px;" />

비교적 느린 모바일 장치의 네트워크 장치에서 페이지를 로드하는 것처럼 시뮬레이션할 수 있습니다.

설정방법

1. Online으로 표시된 드랍다운을 클릭합니다.
1. Slow 3G를 선택합니다.
1. 캐시 비우기 및 강력 새로고침을합니다.
   - 반복 방문시 브라우저는 일반적으로 캐시에서 일부 파일을 제공하므로 페이지로드 속도가 빨라집니다.
   - 캐시 비우기 및 강력 새로고침은 브라우저가 모든 리소스에 대해 네트워크를통해 가져오도록 합니다.
   - 처음 방문자가 페이지를 로드하는것과 동일한 경험을 제공합니다.

## 스크린샷 캡쳐

스크린 샷을 통해 페이지가로드되는 동안 시간이 지남에 따라 어떻게 보이는지 확인할 수 있습니다.

<img src="/assets/images/chrome-devtools-network/allscreenshots.png" alt="스크린샷 캡쳐 예시" style="max-width:800px;" />

방법

1. `Capture Screenshots`을 클릭합니다.
1. 캐시 비우기 및 강력 새로고침을 통해 페이지를 다시 로드합니다.
1. 첫 번째 썸네일을 클릭하면 DevTools는 해당 시점에 어떤 네트워크 활동이 발생했는지 보여줍니다.
1. `Capture Screenshots`을 클릭하여 스크린샷 창을 닫습니다.
1. 페이지를 다시 로드핣니다.

## 상세 리소스 검사

<img src="/assets/images/chrome-devtools-network/headers.png" alt="상세 리소스 검사 예시" style="max-width:800px;" />

리소스의 자세한 내용을 보고싶다면 리소스를 클릭합니다.

검사방법

1. getstarted.html을 클릭하면 `Headers` 탭이 표시됩니다. 이 탭에서 HTTP 헤더를 검사할수 있습니다.
1. `Preview` 탭을 클릭하면 HTML의 기본 렌더링이 표시됩니다.

   - 이 탭은 API오류를 확인하는것보다 랜더링된 HTML으로 확인하는것이 더 편할때 유용합니다.

1. `Response` 탭을 클릭하면 HTML 소스 코드가 표시됩니다.
1. `Timing` 탭을 클릭하면 리소스에 대한 네트워크 로그가 표시됩니다.
1. `X`버튼을 클릭하여 다른 네트워크 로그를 확인할 수 있습니다.

## 네트워크 헤더 및 응답 검색

<img src="/assets/images/chrome-devtools-network/cache.png" alt="네트워크 헤더 및 응답 검색 예시" style="max-width:800px;" />

모든 리소스의 HTTP 헤더 및 response를 검색해야하는 경우 `Search` 패널을 이용합니다.

예를 들어 리소스가 적절한 캐시 정책을 사용하고 있는지 확인하고 싶은경우

1. 검색을 클릭하면 네트워크 로그 왼쪽에 검색 창이 열립니다.
1. `Cache-Control`을 입력하고 Enter키를 누르면 검색 창에는 헤더 또는 내용에서 `Cache-Control`를 사용한 모든 리소스 목록을 보여줍니다.
1. 목록을 클릭하여 확인하십시오.
1. 확인이 끝났다면 `X`버튼을 클릭하여 되돌아 갑니다.

## 리소스 필터

<img src="/assets/images/chrome-devtools-network/filters.png" alt="리소스 필터 예시" style="max-width:800px;" />

DevTools는 현재 작업과 관련없는 리소스를 필터링하기위한 수많은 워크 플로를 제공합니다.

필터는 활성화가 기본값이지만 활성화가 안된경우 Filter 버튼을 클릭하여 활성화할 수 있습니다.

## 문자열, 정규식 또는 속성별로 필터링

<img src="/assets/images/chrome-devtools-network/png.png" alt="문자열, 정규식 또는 속성별로 필터링 예시" style="max-width:800px;" />

필터 텍스트 상자는 다양한 유형의 필터링을 지원합니다.

1. 필터 텍스트 상자에 `png`를 입력하면 텍스트 png가 포함 된 파일 만 표시됩니다.
1. 필터 텍스트 상자에 정규식을 입력하여 필터할 수 있습니다.
1. `-main.css`를 입력하면 DevTools는 main.css를 필터링합니다
1. `domain:raw.githubusercontent.com`을 입력하면 DevTools는 이 도메인과 일치하지 않는 URL로 리소스를 필터링합니다.

## 리소스 타입 필터

<img src="/assets/images/chrome-devtools-network/css.png" alt="리소스 타입 필터 예시" style="max-width:800px;" />

스타일시트와 같은 리소스를 필터 하려면 CSS를 클릭하여 필터할 수 있습니다.

## 요청 차단

<img src="/assets/images/chrome-devtools-network/blockedstyles.png" alt="요청 차단 예시" style="max-width:800px;" />

일부 리소스를 차단한경우 페이지가 어떻게 동작하는지 알고 싶은 경우 사용합니다.

사용방법

1. `Control + Shift + P` 또는 `Command + Shift + P` (Mac)를 눌러 명령 메뉴를 엽니 다.
1. 블록을 입력하고 요청 차단 표시를 선택한 후 Enter를 누르십시오.
1. `+`버튼을 눌러 패턴을 추가합니다.
1. `main.css`을 입력하고 `Add`버튼을 눌러 추가하고, 페이지를 새로고침합니다.
1. `main.css` 리소스가 제외된 것을 확인할 수 있습니다.
1. 하단의 `main.css` 체크박스를 언체크하여 다시 활성화 할 수 있습니다.
