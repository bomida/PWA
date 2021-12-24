## Progressive Web App 설정하는 법


### 📝 목차
- [PWA 란](#PWA-란)
- [특징](#특징)
- [제작에 필요한 것](#제작에-필요한-것)
- [유용한 사이트 또는 툴](#유용한-사이트-또는-툴)

<br>

### PWA 란
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

### 특징
- 반응형(Responsive) 웹 디자인
- App-like & Discoverable
  - 설치 배너가 생성된다.
  - 앱 아이콘이 생성된다.
- Engageable
  - Push 알람
  - PC와 단말기 커버가 가능하다.
- Connectivity
  - Online = Offline 경험을 제공한다.
- Safe
  - 제약사항 기보 기술 -Https 프로토콜이 있어야 가능하다.

<br>

### 제작에 필요한 것
 - __보안 연결(HTTPS)__ : PWA는 신뢰할 수 있는 연결 상태에서만 동작학 때문에, 보안 연결을 통해서 서비스를 제공해야한다.<br>
  단지 보안 상의 이유 뿐만이 아니라, 사용자들의 신뢰를 얻기 위해섣 아주 중요한 부분이다.
 - __오프라인(offline.html)__ : 오프라인 시 보여 줄 마크업 파일이 있어야한다.
 - __인덱스 파일 추가 링크__ : 
``` html
  <!-- charset 코드 바로 아래에 넣는다. -->
  <link rel="manifest" href="manifest.json" />
  <link rel="apple-touch-icon" href="/images/192x192.png" />

  <!-- header 영역의 제일 하단에 추가한다. -->
  <script type="module">
    import 'https://cdn.jsdelivr.net/npm/@pwabuilder/pwaupdate';
    const el = document.createElement('pwa-update');
    document.body.appendChild(el);
  </script>
```
 
 - __서비스 작업자(service worker)__ : 서비스 작업자는 백그라운드에서 실행되는 스크립트이다.<br>
  서비스 작업자는 네트워크와 관련된 요청의 처리를 도와주기 때문에, 그 점에 대해서는 걱정하지 않고 더욱 복잡한 작업을 수행할 수 있다.
``` Javascript
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

 - __매니페스트 파일(manifest file, 설정 파일)__ : 이것은 제이슨(JSON, 용량이 적은 데이터를 교환하 위한 형식) 파일이며,<br>
  PWA가 표시되고 기능하는 방식에 대한 정보들이 포함되어있는 것이다. 여기에서 __PWA의 이름, 설명, 아이콘, 색상 등을 지정할 수 있다.__  
    - start_url: 앱이 시작되어야하는 위치를 지정해야한다.<br>
      사용자들이 PWA에서 개발자가 원하는 특정한 페이지에서 시작하도록 설정하는 것이 좋다.
    - display: 여러분이 보여주고 싶은 브라우저 UI의 타입을 지정할 수 있다.<br>
      설정할 수 있는 옵션으로는 fullscreen(전체화면), standalne(스텐드얼론, 네트워크 연결되지 않은 상태에서도 스스로 동작할 수 있는 것), minimal-ui(최소화된 UI), the standard browser interface(브라우저 표준 인터페이스)가 있다.
  
``` JavaSript
  //  manifest.json 파일 구성
  {
    "short_name": "React App",           // 홈 화면에 표시할 약식 이름
    "name": "Create React App Sample",   // 웹 앱의 이름
    "start_url": ".",                    // 앱이 시작할 때 실행할 초기 문서이다. 보통 index.html로 설정한다.
    "display": "standalone",             // 앱을 표시하는 방식입니다(전체 화면, 독립형(standalone), 최소 UI, 또는 브라우저).
    "theme_color": "#000000",            // 운영 체제에 의해 사용될 UI를 위한 주요 색상
    "background_color": "#ffffff",       // 스플래시 화면과 설치하는 동안 사용될 배경 색상
    "lang": "en",                        // 세팅 언어
    "icons": [                           // 아이콘들의 정보(URL, 타입, 사이즈). 
      {                                  // 사용자의 기기에 적합한 것을 선택할 수 있도록 여러 개를 추가하는 것이 좋다.
        "src": "favicon.ico",
        "sizes": "64x64 32x32 24x24 16x16",
        "type": "image/x-icon"
      },
      {
        "src": "logo192.png",
        "type": "image/png",
        "sizes": "192x192"
      },
      {
        "src": "logo512.png",
        "type": "image/png",
        "sizes": "512x512"
      }
    ]
  }

  // 웹 manifest를 위한 최소 요구 사항은 name과 적어도 하나(src, size, type을 포함)의 아이콘이다. 
  // description, short_name, start_url은 권장사항입니다.
```

<br>

### 유용한 사이트 또는 툴
 - lighthouse : 크롬에 내장된 PWA 설정을 하기에 적합한지를 체크해준다.
 - gitpages : HTTPS 링크를 생성하기 위해서는 다양한 방법이 있지만 간단하게 만들 수 있는 깃헙 페이지를 사용한다.
 - [Maskable.app](https://maskable.app/editor) : PWA를 위한 아이콘을 만들 수 있는 사이트이다.
  
 - 그 외에 참고한 영상 링크
   [드림코딩by엘리](https://www.youtube.com/watch?v=FEBkne7Nyu4&t=621s)
   [얄팍한 코딩사전](https://www.youtube.com/watch?v=NMdnzvPsGu8&t=559s)
  
  
