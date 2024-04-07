## 자바스크립트 모듈

### 자바스크립트 모듈의 주요 이점

| 이점 | 설명 |
|------|------|
| 네임스페이스 관리 | 글로벌 스코프 오염 방지 및 네임스페이스 충돌 위험 감소 |
| 재사용성 | 다양한 애플리케이션에서의 코드 재사용으로 중복 감소 및 효율성 증가 |
| 의존성 관리 | 다른 모듈에 대한 명시적 선언을 통한 쉬운 의존성 관리 |
| 유지보수 | 모듈별 코드 구분으로 간편한 유지보수 |

<br/>

***

<br/>

### 자바스크립트 모듈 시스템 종류

| 종류 | 설명 |
|-------------|------|
| ES6 모듈    | 최신 자바스크립트 표준인 ECMAScript 2015 (ES6)에 도입된 모듈 시스템입니다. `import`와 `export` 키워드를 사용하여 모듈을 불러오고 내보낼 수 있습니다. |
| CommonJS 모듈 | 주로 Node.js 환경에서 사용되는 모듈 시스템으로, `require` 함수를 사용해 모듈을 불러오고, `module.exports`를 통해 모듈을 내보냅니다. |

<br/>

***

<br/>

### 자바스크립트 모듈 시스템 비교 및 실행

| 구분 | ES6 모듈 | CommonJS 모듈 |
|------|----------|---------------|
| 환경 | 최신 웹 브라우저 또는 Node.js (버전 13 이상) | 주로 Node.js |
| 특징 | `import`와 `export` 키워드 사용 | `require`와 `module.exports` 사용 |
| 웹 브라우저에서 실행 | `<script type="module" src="app.js"></script>`로 로드 | - |
| Node.js에서 실행 | `.mjs` 확장자 사용 또는 `package.json`에 `"type": "module"` 추가, `node app.js` 또는 `node app.mjs` 실행 | `node app.js` 명령어 사용 |

<br/>

***

<br/>

### ES6의 모듈 시스템 Export와 Import 기능 비교

| 기능   | 설명 |
|--------|------|
| Export | 모듈에서 특정 기능(함수, 클래스, 객체 등)을 외부에 제공하기 위해 사용합니다. 예를 들어, `export function myFunction() { ... }` 형태로 함수를 내보낼 수 있습니다. |
| Import | 다른 모듈에서 export된 기능을 사용하기 위해 `import` 문을 사용합니다. 예를 들어, `import { myFunction } from 'myModule'` 형태로 특정 모듈에서 함수를 가져옵니다. |

<br/>

***

<br/>

### ES6 모듈

```js
// 1. 모듈 생성 및 내보내기 (Export)
export function Bread(type) {                        // 빵 종류 모듈
  const descriptions = {
    mochaBread: "모카빵",
    saltBread: "소금빵",
    chocoBread: "초코빵"
  };
  return descriptions[type] || "기타";
}
export function recommend(bread) {                   // 빵 추천 모듈
  return bread === "breakfast" ? "chocoBread" : 
    bread === "lunch" ? "saltBread" : 
    "mochaBread";
}

// 2. 모듈 가져오기 (Import)
import { Bread, recommend } from './breadModule.js'; // breadModule.js에서 함수를 가져옵니다.

console.log(Bread("saltBread"));       // "소금빵" 출력
console.log(recommend("breakfast"));   // "chocoBread" 출력
```

<br/>

***

<br/>

### CommonJS 모듈

```js
// 모듈 내보내기
function Bread(type) {
  const descriptions = {
    mochaBread: "모카빵",
    saltBread: "소금빵",
    chocoBread: "초코빵"
  };
  return descriptions[type] || "기타";
}

function recommend(bread) {
  return bread === "breakfast" ? "chocoBread" : 
    bread === "lunch" ? "saltBread" : 
    "mochaBread";
}

module.exports = {
  Bread,
  recommend
};


// 다른 파일에서 모듈 가져오기
const { Bread, recommend } = require('./breadModule.js'); // breadModule.js에서 함수를 가져옵니다.

console.log(Bread("saltBread"));       // "소금빵" 출력
console.log(recommend("breakfast"));   // "chocoBread" 출력
```