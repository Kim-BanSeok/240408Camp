## 브라우저 Web API

### WebAPI 소개  

브라우저의 Web API는 웹 애플리케이션 개발에 필수적인 다양한 프로그래밍 인터페이스를 제공하는 모음입니다. 이 API들을 통해 자바스크립트를 사용하여 브라우저의 다양한 기능에 접근하고 조작할 수 있으며, 오직 웹 브라우저 환경에서만 작동합니다.

Web API는 브라우저가 제공하는 인터페이스 및 객체 집합을 활용하여 웹 애플리케이션의 범위를 넓힐 수 있게 도와줍니다. 예를 들어, fetch API를 이용해 네트워크 요청을 처리하거나, localStorage를 활용해 데이터를 저장하고, Geolocation API로 사용자의 위치 정보에 접근하는 것이 가능합니다.

<br/>

***

<br/>

### WebAPI 종류

| [Web API](https://developer.mozilla.org/ko/docs/Web/API) | 설명 |
|---------|-----|
| DOM (Document Object Model) API | 웹 페이지의 HTML 및 XML 문서를 조작할 수 있게 해주는 API입니다. DOM API를 사용하여 문서의 구조, 스타일, 내용 등을 동적으로 변경할 수 있습니다. |
| Fetch API | 네트워크 요청을 수행하고 데이터를 비동기적으로 불러오는 방법을 제공합니다. `fetch()` 함수를 사용하여 서버로부터 리소스를 요청하고 받아올 수 있습니다. |
| Canvas 및 WebGL API | 그래픽을 그리기 위한 API입니다. Canvas API는 2D 그래픽스를, WebGL API는 3D 그래픽스를 브라우저에서 렌더링하는 데 사용됩니다. |
| Geolocation API | 사용자의 위치 정보를 얻을 수 있게 해주는 API입니다. 사용자의 현재 위치를 얻어 지도 서비스나 위치 기반 서비스에 활용할 수 있습니다. |
| Web Storage API | 브라우저에 데이터를 저장할 수 있는 API입니다. `localStorage`와 `sessionStorage`를 통해 간단한 키-값 쌍을 저장하고, 필요할 때 이를 다시 읽어올 수 있습니다. |
| Audio 및 Video API | 오디오와 비디오를 웹 페이지에 삽입하고 제어할 수 있는 API입니다. HTML의 `<audio>`와 `<video>` 요소를 통해 멀티미디어 컨텐츠를 조작합니다. |

