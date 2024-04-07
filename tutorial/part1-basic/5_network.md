## 자바스크립트 네트워크 요청

### 환경에 따라 다른 네트워크 요청 처리

| 환경 | 예시 |
| ---- | ---- |
| 브라우저 환경 | `fetch('/some/url')` |
| 런타임 환경 | `http.get('/some/url')` |

<details>
<summary>네트워크 설명</summary>

> 브라우저 환경에서의 네트워크 요청  
> - `fetch('/some/url')`를 사용해 서버로부터 데이터를 요청하고 받아옵니다.  
> - 웹 페이지의 동적 콘텐츠 로딩이나 사용자 데이터 전송에 주로 사용됩니다.

> JS 런타임 환경에서의 네트워크 요청  
> - 서버 측 환경(예: Node.js)에서 `http.get('/some/url')`를 이용해 다른 서버에 데이터를 요청합니다.  
> - 백엔드에서 데이터 처리, API 통신, 외부 서버 간 상호작용에 사용됩니다.

</details>

### 네트워크 요청을 다루는 방법

| 기술 | 설명 | 특징 |
| ---- | --- | --- |
| Fetch API | 자바스크립트의 내장 함수로, `Promise` 기반으로 서버와 네트워크 통신을 합니다. | 현대적이고 간결한 구문, `async/await`와 잘 호환되어 사용하기 쉽습니다. |
| Axios | HTTP 클라이언트 라이브러리로, `Promise` 기반으로 작동하며 브라우저와 Node.js에서 사용 가능합니다. | 자동 JSON 변환, 넓은 브라우저 호환성 및 Node.js 지원. |
| XMLHttpRequest | 자바스크립트에서 서버와 상호작용하기 위한 오래된 방법입니다. 비동기 요청을 보내는 데 사용됩니다. | 복잡한 구문이지만, 거의 모든 브라우저에서 지원됩니다. |
| AJAX | `XMLHttpRequest`를 포함한 여러 기술을 사용하여 데이터를 비동기적으로 교환하는 방식입니다. | 페이지 새로 고침 없이 서버와 통신 가능, 다양한 라이브러리(`jQuery` 등)와 함께 사용됩니다. |

***

> FetchAPI : https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch  
Axios: https://axios-http.com/kr/docs/intro  
XMLHttpRequest : https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest  
AJAX : https://namu.wiki/w/AJAX  
