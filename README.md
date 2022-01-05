# Progressive Web App 설정하는 법

<br>

### 📝 목차
- [PWA 란](#PWA-란)
- [특징](#특징)
- [Manifest란?](#Manifest란?)
- [유용한 사이트 또는 툴](#유용한-사이트-또는-툴)

<br>

## PWA 란
모바일 앱고 웹 기술의 장점을 결합한 것

  - 브라우저를 통해 처음 방문한 사용자에게 유용하며, 설치가 필요하지 않다.<br>
  - 느린 네트워크에서 빠르게 로드되며, 관련된 푸시 알림을 전송한다.<br>
  - __모바일 앱처럼 주소창이 없는 전체 화면이 로드되고, 홈 화면에 아이콘이 표시된다.__<br>

이처럼 모두 각자의 네이티브 앱과 비슷한 사용자경험을 제공한다.

- 웹 브라우저를 통해 설치 없이 페이지 접속 후 바탕화면에 앱 아이콘을 추가할 수 있다.
- 언제든지 푸시알림을 통해 재참여가 가능하다.
- 오프라인에서도 웹 또는 앱에 접근 가능하다.
<br>

PWA로 만들어진 대표적인 웹사이트로는 네이버(Naver), 스타벅스(Starbucks), 핀터레스트(Pinterest), 우버(Uber), 트위터(Twitter)가 있다.

<br>

## 특징
- 반응형(Responsive) 웹 디자인
- App-like & Discoverable
  - 설치 배너가 생성됨
  - 앱 아이콘이 생성됨
- Engageable
  - Push 알람
  - PC와 단말기 커버가 가능
- Connectivity
  - Online = Offline 경험을 제공함
- Safe
  - 제약사항 기보 기술 -Https 프로토콜이 있어야 가능

<br>

## Manifest란?
   매니페스트는 웹을 데스크톱 및 모바일장칭 설치할 때 __아이콘, 이름, 시작시 시작해야하는 경로__ 등의 내용을 브라우저에 알려주는 json파일입니다.
   <br>manifest를 설정하게 되면 해당 앱을 설치할 때의 아이콘을 설정할 수 있고, 처음 앱을 활성화 시킬 때 스플래시 이미지를 보여주어
   <br>좀 더 앱과 가깝게 느낄 수 있도록 해줍니다.
   
   <br>
   
   ###  manifest 속성
   
   #### name, short_name
   __name__ : 앱을 설치하고나면 icon에 표시되는 이름입니다.
   __short_name__ : 사용자의 홈 화면이나 name을 보여주기에는 제한적인 장소에서 표시되는 이름입니다.
    
   #### display
   __display__ : 설치하 앱으 실행할때 브라우저 처럼 보일지 앱처럼 보일지 아예 전체화면으로 보일지 등에 대한 설정을 할 수 있습니다.
     <br>속성 옵션에는 `fullscreen`, `minimul-ui`, `standalone`, `browser`가 있습니다.
     <br>browser : 일반 브라우저와 동일하게 보입니다.
     <br>standalone : 다른 앱처럼 최상단에 상태표시줄을 제외한 전체화면을 보입니다..
     <br>fullscreen : 상태표시줄도 제외한 전체화면으로 보여줍니다.(예, 게임)
     <br>miniul-ui : fullscreen고 비슷하지만 뒤로가기, 새로고침 등 최소한의 영역만 제공합니다.(모바일 크롬 전용)
   
   #### orientation
   __orientation__ : 앱이 실행될 때 `가로`, `세로`의 방향을 선택할 수 있습니다.
   이 옵션은 선택사항이므로 고정해야하는 상황이 아니라면 사용하지 않아도 됩니다.

   #### start_url
   __start_url__ : 홈 화면에 설치한 앱을 시작할 때 처음에 시작할 위치를 정합니다.

   #### theme_color
   __theme_color__ : 상단부의 테마 부분의 색상을 지정할 수 있습니다. 단, hex코드로 지정해야합니다.

   #### background_color
   __background_color__ : 앱이 처음 시작될 때 `splashscreen`에서 사용하기 위해 사용됩니다. 단, hex코드로 지정해야합니다.

   #### icons
   __icons__ : 홈화면에 추가하면 화면에 보여지기위해 사용하는 아이콘을 설정하는 옵션입니다.
     <br>앱실행, 작업 전환, 스플래시 화면 등의 장소에 사용하게 됩니다.
     <br>safari 브라우저에서는 이를 지원하지 않아 head에 다음과 같은 태그를 추가하여 브라우징 이슈를 해결해야합니다.
     ```
     <link rel="apple-touch-icon" sizes="192x192" href="/images/icons/icon-192x192.png">
     <link rel="apple-touch-icon" sizes="512x512" href="/images/icons/icon-512x512.png">
     ```
   
   <br>
   
   ### manifest.json
   ```
   {
    "name": "Paint Board",
    "short_name": "Paint Board",
    "display": "standalone",
    "orientation": "portrait",
    "dir": "ltr",
    "scope": "/",
    "start_url": "/index.html",
    "theme_color": "#ffffff",
    "background_color": "#ffffff",
    "purpose": "any maskable",
    "icons": [
      {
        "src": "icons/icon-48x48.png",
        "sizes": "48x48",
        "type": "image/png"
      },
      {
        "src": "icons/icon-72x72.png",
        "sizes": "72x72",
        "type": "image/png"
      },
      {
        "src": "icons/icon-96x96.png",
        "sizes": "96x96",
        "type": "image/png"
      },
      {
        "src": "icons/icon-128x128.png",
        "sizes": "128x128",
        "type": "image/png"
      },
      {
        "src": "icons/icon-192x192.png",
        "sizes": "192x192",
        "type": "image/png"
      },
      {
        "src": "icons/icon-384x384.png",
        "sizes": "384x384",
        "type": "image/png"
      },
      {
        "src": "icons/icon-512x512.png",
        "sizes": "512x512",
        "type": "image/png"
      }
     ]
   }
   ```
   
   이렇게 만든 manifest파일을 index.html 파일에 link 태그를 사용하여 추가해주세요.
   ```
   <link rel="manifest" href="/manifest.json">
   ```

<br>

## Service Worker란?
서비스워커는 브라우저가 백그라운드에서 실행하는 스크립트로 웹페이지와는 별개로 동작하며 브라우저와 웹서버 간의 미들웨어 역할을 수행합니다.
<br>서비스 워커를 사용하게 되면 대표적으로 다음과 같은 기능들을 사용할 수 있습니다.
  - web-push service
  - 백그라운드 동기화 기능
  - 네트워크 요청을 가로채 캐쉬 상호작용
이때 서비스워커는 `https` 환경 혹은 `localhost` 환경에서만 동작하기 때문에 local환경에서 특정 도메인을 반드시 사용해야 하는 경우
<br>이에 대해서도 반드시 `https` 설정을 해야합니다. 그리고 서비스워커(serviceWorker)에서는 기존의 자바스크립트와 다른 쓰레드에서
<br>사용되기 때문에 `self`를 통해 `this`를 접근할 수 있습니다.

   ### pwabuilder-sw.js
   ```
   // This is the "Offline page" service worker

      importScripts(
       'https://storage.googleapis.com/workbox-cdn/releases/5.1.2/workbox-sw.js'
      );

      const CACHE = 'pwabuilder-page';

      const offlineFallbackPage = 'offline.html';

      self.addEventListener('message', (event) => {
        if (event.data && event.data.type === 'SKIP_WAITING') {
          self.skipWaiting();
        }
      });

      self.addEventListener('install', async (event) => {
        event.waitUntil(
          caches.open(CACHE).then((cache) => cache.add(offlineFallbackPage))
        );
      });

      if (workbox.navigationPreload.isSupported()) {
        workbox.navigationPreload.enable();
      }

      self.addEventListener('fetch', (event) => {
        if (event.request.mode === 'navigate') {
          event.respondWith(
            (async () => {
              try {
                const preloadResp = await event.preloadResponse;

                if (preloadResp) {
                  return preloadResp;
                }

                const networkResp = await fetch(event.request);
                return networkResp;
              } catch (error) {
                const cache = await caches.open(CACHE);
                const cachedResp = await cache.match(offlineFallbackPage);
                return cachedResp;
              }
            })()
          );
        }
      });   
   ```
   
   service worker 파일을 만들었다면 offline.html이란 이름으로 오프라인일 때 보여줄 화면을 만들어줍니다.
   <br>그리고 오프라인 파일은 `head`영역에 추가하지 않아도 됩니다.

<br>

## index.html
PWA 적용을 위한 html파일 작성은 대략 아래와 같습니다.
```
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <link rel="stylesheet" href="style.css">
    <link rel="manifest" href="manifest.json">
    <link rel="apple-touch-icon" sizes="192x192" href="/images/icons/icon-192x192.png">
    <link rel="apple-touch-icon" sizes="512x512" href="/images/icons/icon-512x512.png">
    <link rel="icon" type="image/x-icon" href="icons/icon-72x72.png">

    <script src="main.js"></script>
    <script type="module">
      import 'https://cdn.jsdelivr.net/npm/@pwabuilder/pwaupdate';
      const el = document.createElement('pwa-update');
      document.body.appendChild(el);
    </script>

    <title>Title area</title>
  </head>
```

<br>

## 유용한 사이트 또는 툴
 - [PWA builder](https://www.pwabuilder.com) : 웹앱이 PWA화 되기에 적합한지 검사할 수 있고,
 부족한 파일들을 생성시켜줘서 간편하게 스토어에 런칭할 수 있게 해줍니다.
 - lighthouse : 더욱 디테일한 체크를 하고싶다면 확장프로그램을 설치하면 확인할 수 있습니다.
 - [Maskable.app](https://maskable.app/editor) : PWA를 위한 아이콘을 만들 수 있는 사이트입니다.
 - [깃헙 페이지를 위한 serviceWorker](https://gist.github.com/kosamari/7c5d1e8449b2fbc97d372675f16b566e) :
 깃헙 페이지로 웹앱을 만들 때 참고할만한 service worker 템플릿입니다.
 - [Next steps for building your Progressive Web App (PWA)](https://github.com/pwa-builder/pwabuilder-web/blob/V2/src/assets/next-steps.md)
  
<br>  
  
 ## 그 외에 참고한 영상 링크
  [드림코딩by엘리](https://www.youtube.com/watch?v=FEBkne7Nyu4&t=621s)<br>
  [얄팍한 코딩사전](https://www.youtube.com/watch?v=NMdnzvPsGu8&t=559s)
  
  
