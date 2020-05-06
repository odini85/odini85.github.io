1. 프로젝트 추가
1. 프로젝트 이름 입력
1. Google 애널리스틱 설정(안해도됨)
1. 확인
1. [계속] 버튼 클릭
1. 사이드 메뉴 Hosting 메뉴 선택
1. [시작] 버튼 클릭
1. Firebase CLI 설치
   - `npm install -g firebase-tools`
1. 프로젝트 초기화

   1. Google 로그인
      - `firebase login`
   1. 프로젝트 시작(앱의 루트 디렉터리에서 다음 명령어를 실행하세요.)
      - `firebase init`
        - Project Setup
          - Which Firebase CLI features do you want to set up for this folder? Press Space to select features, then Enter to confirm your choice
            - `Hosting: Configure and deploy Firebase Hosting sites`
          - First, let's associate this project directory with a Firebase project.<br />
            You can create multiple project aliases by running firebase use --add,<br />
            but for now we'll just set up a default project. - `❯ Use an existing project`
        - Hosting Setup
          - Your public directory is the folder (relative to your project directory) that will contain Hosting assets to be uploaded with firebase deploy. If you have a build process for your assets, use your build's output directory.
            - What do you want to use as your public directory?
              - `dist`
            - Configure as a single-page app (rewrite all urls to /index.html)?
              - `n`

1. Firebase 호스팅에 배포
   - `firebase deploy`
   - 팁
     - 환경에 따라(stage, product..) 다른 프로젝트에 배포를 원하다면 아래와 같은 npm script를 미리 등록
       - `npm run build && firebase use <Project ID> && firebase deploy`
