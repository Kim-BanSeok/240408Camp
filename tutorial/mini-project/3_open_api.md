## Open API 를 활용한 미니 프로젝트

- 사용할 API

한국에너지공단_에너지사용 및 온실가스배출량 통계-마이크로데이터

에너지 사용 및 온실가스 배출량 마이크로데이터 조회 서비스(조사대상 업체현황 마이크로 데이터 서비스 Rest 조회, 업종별 에너지 소비량 마이크로 데이터 서비스 Rest 조회, 업종별 CO2 배출량 현황 마이크로 데이터 서비스 Rest 조회, 온실가스 배출량 환산계수 Rest 조회, 분석보고서 결과분석통계 현황 Rest 조회)

https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15075918#tab_layer_detail_function


OpenAPI 데이터 테스트 순서

1. OpenAPI 에서 API 발급받기

선택한 API  
> 제목 : 한국에너지공단_에너지사용 및 온실가스배출량 통계-마이크로데이터  
> 설명 : 에너지 사용 및 온실가스 배출량 마이크로데이터 조회 서비스(조사대상 업체현황 마이크로 데이터 서비스 Rest 조회, 업종별 에너지 소비량 마이크로 데이터 서비스 Rest 조회, 업종별 CO2 배출량 현황 마이크로 데이터 서비스 Rest 조회, 온실가스 배출량 환산계수 Rest 조회, 분석보고서 결과분석통계 현황 Rest 조회)
> 링크 : https://www.data.go.kr/data/15075918/openapi.do#/

활용신청 버튼 클릭  
![alt text](image-2.png)

신청 승인 확인  
![alt text](image-3.png)
![alt text](image-4.png)

> 데이터포맷 확인	JSON+XML  
> End Point	주소 확인 http://apis.data.go.kr/B553530/GHG_LIST_040
> 일반 인증키 (Encoding)와 일반 인증키 (Decoding) 확인

웹에서 정상 데이터 확인해보기  
![alt text](image-5.png)

인증 키에 키 넣고 미리보기 버튼 클릭
![alt text](image-6.png)

정상 데이터 확인  
![alt text](image-7.png)

일반 인증키 (Encoding)와 일반 인증키 (Decoding) 복사  
![alt text](image-8.png)

API 종류 확인하기  
![alt text](image-9.png)

```
/GHG_LIST_04_03_20220831_VIEW01
업종별 CO2 배출량 현황 마이크로 데이터 서비스 Rest 조회

/GHG_LIST_04_01_20220831_VIEW01
에너지-온실가스 조사대상 업체현황 마이크로 데이터 서비스 Rest 조회

/GHG_LIST_04_05_20220831_VIEW01
분석보고서 결과분석통계 현황 서비스

/GHG_LIST_04_04_20220831_VIEW01
온실가스 배출량 환산계수 Rest 조회

/GHG_LIST_04_02_20220831_VIEW01
업종별 에너지 소비량 마이크로 데이터 서비스 Rest 조회
```

이제 코드 작성


2. API 연결 후 터미널에서 정상 데이터 확인하기  
`Nodejs 환경`에서 `node app.js` 로 스크립트 실행하면서 fetch 로 API 연결 후 데이터 GET 테스트까지 완료해보기  
`package.json`에 `{"type" : "module"}` 추가해서 모듈로 터미널에서 테스트해야 import 라이브러리를 사용할 수 있음

```js
// NodeJS 환경에서 API 데이터 받아오기 테스트
import fetch from 'node-fetch';
import { parseStringPromise } from 'xml2js';

const openApiUrl = 'http://apis.data.go.kr/B553530/GHG_LIST_040/GHG_LIST_04_03_20220831_VIEW01';
const serviceKeys = [
  'E%2FU6HXW2N%2BAzFh2F985tamUeDOwfJE0X9%2FbxNr9GwALvC7jV5bsKYOVgCEjUzbWqMx1Y51vwHrNU52xrW8niqQ%3D%3D',
  'E1VjwrWDlS7hDDnd8diNNaIkEbroWWTHqgR7vL2KIrXjup%2B1ZdlodbLQGMuMGDFx%2B1rxxS3vV4NJlQJ3JCzQuA%3D%3D'
];

function simplifyJSON(data) {
  if (Array.isArray(data)) {
    return data.length === 1 ? simplifyJSON(data[0]) : data.map(simplifyJSON);
  } else if (typeof data === 'object') {
    return Object.fromEntries(Object.entries(data).map(([k, v]) => [k, simplifyJSON(v)]));
  } else {
    return data;
  }
}

function fetchAPIWithKey(serviceKey) {
  let queryParams = '?' + encodeURIComponent('serviceKey') + '=' + serviceKey;
  // queryParams += '&' + encodeURIComponent('pageNo') + '=' + encodeURIComponent('1');
  // queryParams += '&' + encodeURIComponent('numOfRows') + '=' + encodeURIComponent('3');
  // queryParams += '&' + encodeURIComponent('apiType') + '=' + encodeURIComponent('XML');
  // queryParams += '&' + encodeURIComponent('q1') + '=' + encodeURIComponent('2019');
  // queryParams += '&' + encodeURIComponent('q2') + '=' + encodeURIComponent('5인 ~ 9인');
  // queryParams += '&' + encodeURIComponent('q3') + '=' + encodeURIComponent('대전');
  // queryParams += '&' + encodeURIComponent('q4') + '=' + encodeURIComponent('27213');
  // queryParams += '&' + encodeURIComponent('q5') + '=' + encodeURIComponent('전력');
  // queryParams += '&' + encodeURIComponent('q6') + '=' + encodeURIComponent('전력');

  return fetch(openApiUrl + queryParams, {
    method: 'GET',
    headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json',
    }
  });
}

async function fetchWithRetry() {
  for (const key of serviceKeys) {
    try {
      const response = await fetchAPIWithKey(key);
      if (response.ok) {
        const xmlData = await response.text();
        const jsonData = await parseStringPromise(xmlData);
        const simplifiedData = simplifyJSON(jsonData);
        return simplifiedData;
      }
    } catch (error) {
      console.log(`Error with key ${key}:`, error);
    }
  }
  throw new Error('All attempts failed');
}

fetchWithRetry()
  .then(data => {
    console.log(JSON.stringify(data, null, 2)); // JSON 형태로 출력
  })
  .catch(error => {
    console.log('Error:', error);
  });
```

2. 브라우저 환경에서 서버 실행해보기  
- `브라우저 환경`에서 `http-server` 로 실행했을 때 Nodejs 환경에서 사용했던 라이브러리들을 사용하지 못할 수 있으니, 브라우저 환경에 맞는 스크립트 코드로 수정해서 실행해보기
- 에러를 전부 고치고 마지막에 CORS 에러가 나오면서 더이상 실행이 안된다면, API 제공하는 서버에서 Access-Control-Allow-Origin 를 허용하지 않은 것이기 때문에 CORS(Cross-Origin Resource Sharing) 에러가 계속 나옵니다.
- 개발 중인 프론트엔드 애플리케이션과 외부 API 간의 요청을 중계할 프록시 서버를 설정해야함, 가장 간단한 방법 중 하나는 Node.js를 사용하여 프록시 서버를 구축하는 것


3. 프록시 서버 구축
Express와 http-proxy-middleware 설치 `npm install express http-proxy-middleware`
서버 파일 생성 `server.js`
여전히 cors 문제가 생긴다면 cors 미들웨어 추가

<br/><br/>

## 백엔드 서버와 프론트엔드

1. NodeJS 환경에서 실행되는 express 프록시 서버 (백엔드)
`server.js`에 express 코드 작성하고
실행은 터미널에서 `node server.js` 로 실행
> 프록시 서버 설정 전에 터미널에서 테스트할 떄에 `package.json`에 type module 을 설정해두었는데 프록시 서버를 실행하면서 모듈은 지워야함

```js
const express = require('express');
const cors = require('cors');
const { createProxyMiddleware } = require('http-proxy-middleware');

const app = express();
app.use(cors()); // CORS 미들웨어 추가

const port = 3000; // 원하는 포트 번호로 변경할 수 있습니다.

// 프록시 설정
app.use('/api', createProxyMiddleware({
  target: 'http://apis.data.go.kr', // 타겟 API의 기본 URL
  changeOrigin: true,
  pathRewrite: {'^/api' : ''} // URL 경로 재작성 (예: /api/xyz -> /xyz)
}));

app.listen(port, () => {
  console.log(`Proxy server listening on port ${port}`);
});
```

2. 브라우저 환경에서 실행 (프론트엔드)
`script.js` 에 fetch 코드 작성하고
실행은 터미널에서 `http-server` 로 실행

```js
const APIURL = 'localhost:3000/api'; // 'http://apis.data.go.kr' 대신 'http://localhost:3000/api' 사용
const openApiUrl = `http://${APIURL}/B553530/GHG_LIST_040/GHG_LIST_04_03_20220831_VIEW01`;
const serviceKeys = [
  'E%2FU6HXW2N%2BAzFh2F985tamUeDOwfJE0X9%2FbxNr9GwALvC7jV5bsKYOVgCEjUzbWqMx1Y51vwHrNU52xrW8niqQ%3D%3D',
  'E1VjwrWDlS7hDDnd8diNNaIkEbroWWTHqgR7vL2KIrXjup%2B1ZdlodbLQGMuMGDFx%2B1rxxS3vV4NJlQJ3JCzQuA%3D%3D'
];

function parseXMLtoJSON(xmlString) {
  const parser = new DOMParser();
  const xmlDoc = parser.parseFromString(xmlString, "application/xml");
  const items = xmlDoc.getElementsByTagName('item'); // XML의 'item' 태그들을 JSON 객체로 변환
  const jsonResult = {}; // jsonResult 초기화
  jsonResult['items'] = [];

  for (let item of items) {
      let itemObj = {};
      for (let node of item.children) {
          itemObj[node.tagName] = node.textContent;
      }
      jsonResult['items'].push(itemObj);
  }

  return jsonResult;
}

function fetchAPIWithKey(serviceKey) {
  let queryParams = '?' + encodeURIComponent('serviceKey') + '=' + serviceKey;
  // queryParams += '&' + encodeURIComponent('pageNo') + '=' + encodeURIComponent('1');
  // queryParams += '&' + encodeURIComponent('numOfRows') + '=' + encodeURIComponent('3');
  // queryParams += '&' + encodeURIComponent('apiType') + '=' + encodeURIComponent('XML');
  // queryParams += '&' + encodeURIComponent('q1') + '=' + encodeURIComponent('2019');
  // queryParams += '&' + encodeURIComponent('q2') + '=' + encodeURIComponent('5인 ~ 9인');
  // queryParams += '&' + encodeURIComponent('q3') + '=' + encodeURIComponent('대전');
  // queryParams += '&' + encodeURIComponent('q4') + '=' + encodeURIComponent('27213');
  // queryParams += '&' + encodeURIComponent('q5') + '=' + encodeURIComponent('전력');
  // queryParams += '&' + encodeURIComponent('q6') + '=' + encodeURIComponent('전력');

  return fetch(openApiUrl + queryParams, {
    method: 'GET',
    headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json',
    }
  });
}

async function fetchWithRetry() {
  for (const key of serviceKeys) {
    try {
      const response = await fetchAPIWithKey(key);
      if (response.ok) {
        const xmlData = await response.text();
        const jsonData = parseXMLtoJSON(xmlData);
        return jsonData;
      }
    } catch (error) {
      console.log(`Error with key ${key}:`, error);
    }
  }
  throw new Error('All attempts failed');
}

fetchWithRetry()
  .then(data => {
    console.log(JSON.stringify(data, null, 2)); // JSON 형태로 출력
  })
  .catch(error => {
    console.log('Error:', error);
  });
```

결과
![alt text](image-1.png)

3. 브라우저에 데이터 출력

콘솔에 데이터가 정상적으로 출력 된다면 이제 브라우저에 데이터 출력되도록 테이블 만들기
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>API Data Display</title>
  <style>
    table {
      width: 100%;
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid black;
      padding: 4px;
      text-align: left;
    }
  </style>
</head>
<body>
  <h1>API Data</h1>
  <table id="dataTable">
    <thead>
      <tr>
        <th>TRGT_YEAR</th>
        <th>ENGSRC_NM</th>
        <th>ENGSRC_DVSN_NM</th>
        <th>WRKPLC_FANM</th>
        <th>WIDM_LOCL_NM</th>
        <th>WRKPLC_WRKR_VOL_NM</th>
        <th>KSIC_CD</th>
        <th>KSIC_NM</th>
        <th>GHG_EMSN_QNTY_NIDVAL</th>
        <th>DATA_REG_DT</th>
      </tr>
    </thead>
    <tbody>
        <!-- Data will be inserted here -->
    </tbody>
  </table>
  <script src="script.js"></script>
</body>
</html>
```

`script.js` 에서 데이터 받아오는 부분에 json 키를 가져와서 테이블에 넣도록 수정하기 
```js
fetchWithRetry()
  .then(data => {
    const tableBody = document.getElementById('dataTable').getElementsByTagName('tbody')[0];
    
    data.items.forEach(item => {
      let row = tableBody.insertRow();
      row.insertCell(0).textContent = item.TRGT_YEAR;
      row.insertCell(1).textContent = item.ENGSRC_NM;
      row.insertCell(2).textContent = item.ENGSRC_DVSN_NM;
      row.insertCell(3).textContent = item.WRKPLC_FANM;
      row.insertCell(4).textContent = item.WIDM_LOCL_NM;
      row.insertCell(5).textContent = item.WRKPLC_WRKR_VOL_NM;
      row.insertCell(6).textContent = item.KSIC_CD;
      row.insertCell(7).textContent = item.KSIC_NM;
      row.insertCell(8).textContent = item.GHG_EMSN_QNTY_NIDVAL;
      row.insertCell(9).textContent = item.DATA_REG_DT;
      console.log(JSON.stringify(data, null, 2)); // JSON 형태로 출력
    });
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

결과
![alt text](image.png)


# 전체 완성 코드

`index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>API Data Display</title>
  <style>
    table {
      width: 100%;
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid black;
      padding: 4px;
      text-align: left;
    }
  </style>
</head>
<body>
  <h1>API Data</h1>
  <table id="dataTable">
    <thead>
      <tr>
        <th>TRGT_YEAR</th>
        <th>ENGSRC_NM</th>
        <th>ENGSRC_DVSN_NM</th>
        <th>WRKPLC_FANM</th>
        <th>WIDM_LOCL_NM</th>
        <th>WRKPLC_WRKR_VOL_NM</th>
        <th>KSIC_CD</th>
        <th>KSIC_NM</th>
        <th>GHG_EMSN_QNTY_NIDVAL</th>
        <th>DATA_REG_DT</th>
      </tr>
    </thead>
    <tbody>
        <!-- Data will be inserted here -->
    </tbody>
  </table>
  <script src="script.js"></script>
</body>
</html>

```
`script.js`
```js
const APIURL = 'localhost:3000/api'; // 'http://apis.data.go.kr' 대신 'http://localhost:3000/api' 사용
const openApiUrl = `http://${APIURL}/B553530/GHG_LIST_040/GHG_LIST_04_03_20220831_VIEW01`;
const serviceKeys = [
  'E%2FU6HXW2N%2BAzFh2F985tamUeDOwfJE0X9%2FbxNr9GwALvC7jV5bsKYOVgCEjUzbWqMx1Y51vwHrNU52xrW8niqQ%3D%3D',
  'E1VjwrWDlS7hDDnd8diNNaIkEbroWWTHqgR7vL2KIrXjup%2B1ZdlodbLQGMuMGDFx%2B1rxxS3vV4NJlQJ3JCzQuA%3D%3D'
];

function parseXMLtoJSON(xmlString) {
  const parser = new DOMParser();
  const xmlDoc = parser.parseFromString(xmlString, "application/xml");
  // 예를 들어, XML의 'item' 태그들을 JSON 객체로 변환
  const items = xmlDoc.getElementsByTagName('item');
  const jsonResult = {}; // jsonResult 초기화
  jsonResult['items'] = [];

  for (let item of items) {
      let itemObj = {};
      for (let node of item.children) {
          itemObj[node.tagName] = node.textContent;
      }
      jsonResult['items'].push(itemObj);
  }

  return jsonResult;
}

function fetchAPIWithKey(serviceKey) {
  let queryParams = '?' + encodeURIComponent('serviceKey') + '=' + serviceKey;
  // queryParams += '&' + encodeURIComponent('pageNo') + '=' + encodeURIComponent('1');
  // queryParams += '&' + encodeURIComponent('numOfRows') + '=' + encodeURIComponent('3');
  // queryParams += '&' + encodeURIComponent('apiType') + '=' + encodeURIComponent('XML');
  // queryParams += '&' + encodeURIComponent('q1') + '=' + encodeURIComponent('2019');
  // queryParams += '&' + encodeURIComponent('q2') + '=' + encodeURIComponent('5인 ~ 9인');
  // queryParams += '&' + encodeURIComponent('q3') + '=' + encodeURIComponent('대전');
  // queryParams += '&' + encodeURIComponent('q4') + '=' + encodeURIComponent('27213');
  // queryParams += '&' + encodeURIComponent('q5') + '=' + encodeURIComponent('전력');
  // queryParams += '&' + encodeURIComponent('q6') + '=' + encodeURIComponent('전력');

  return fetch(openApiUrl + queryParams, {
    method: 'GET',
    headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json',
    }
  });
}

async function fetchWithRetry() {
  for (const key of serviceKeys) {
    try {
      const response = await fetchAPIWithKey(key);
      if (response.ok) {
        const xmlData = await response.text();
        const jsonData = parseXMLtoJSON(xmlData);
        return jsonData;
      }
    } catch (error) {
      console.log(`Error with key ${key}:`, error);
    }
  }
  throw new Error('All attempts failed');
}

fetchWithRetry()
  .then(data => {
    const tableBody = document.getElementById('dataTable').getElementsByTagName('tbody')[0];
    
    data.items.forEach(item => {
      let row = tableBody.insertRow();
      row.insertCell(0).textContent = item.TRGT_YEAR;
      row.insertCell(1).textContent = item.ENGSRC_NM;
      row.insertCell(2).textContent = item.ENGSRC_DVSN_NM;
      row.insertCell(3).textContent = item.WRKPLC_FANM;
      row.insertCell(4).textContent = item.WIDM_LOCL_NM;
      row.insertCell(5).textContent = item.WRKPLC_WRKR_VOL_NM;
      row.insertCell(6).textContent = item.KSIC_CD;
      row.insertCell(7).textContent = item.KSIC_NM;
      row.insertCell(8).textContent = item.GHG_EMSN_QNTY_NIDVAL;
      row.insertCell(9).textContent = item.DATA_REG_DT;
      console.log(JSON.stringify(data, null, 2)); // JSON 형태로 출력
    });
  })
  .catch(error => {
    console.error('Error:', error);
  });
```
`server.js`
```js
const express = require('express');
const cors = require('cors');
const { createProxyMiddleware } = require('http-proxy-middleware');

const app = express();
app.use(cors()); // CORS 미들웨어 추가

const port = 3000; // 원하는 포트 번호로 변경할 수 있습니다.

// 프록시 설정
app.use('/api', createProxyMiddleware({
  target: 'http://apis.data.go.kr', // 타겟 API의 기본 URL
  changeOrigin: true,
  pathRewrite: {'^/api' : ''} // URL 경로 재작성 (예: /api/xyz -> /xyz)
}));

app.listen(port, () => {
  console.log(`Proxy server listening on port ${port}`);
});
```


## 여러 api 사용하기

미들웨어 대신 axios 로 변경해서 사용

`npm i axios`

```js
const express = require('express');
const cors = require('cors');
const axios = require('axios');

const app = express();
app.use(cors()); // CORS 미들웨어 추가

const port = 3000; // 원하는 포트 번호로 변경할 수 있습니다.

// 백엔드에서 API를 호출하는 함수
async function fetchDataFromAPI(apiPath, queryParams) {
  const apiUrl = `http://apis.data.go.kr/${apiPath}${queryParams}`;
  try {
    const response = await axios.get(apiUrl);
    return response.data;
  } catch (error) {
    throw new Error(`Error fetching data from API: ${error.message}`);
  }
}

// 예시: 업종별 CO2 배출량 현황 API 호출
app.get('/api/GHG_LIST_04_03_20220831_VIEW01', async (req, res) => {
  const apiPath = 'GHG_LIST_04_03_20220831_VIEW01';
  const queryParams = `?serviceKey=${req.query.serviceKey}`;
  try {
    const data = await fetchDataFromAPI(apiPath, queryParams);
    res.json(data);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// 예시: 에너지-온실가스 조사대상 업체현황 API 호출
app.get('/api/GHG_LIST_04_01_20220831_VIEW01', async (req, res) => {
  const apiPath = 'GHG_LIST_04_01_20220831_VIEW01';
  const queryParams = `?serviceKey=${req.query.serviceKey}`;
  try {
    const data = await fetchDataFromAPI(apiPath, queryParams);
    res.json(data);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// 나머지 API에 대해서도 유사한 방식으로 엔드포인트를 생성하여 데이터를 전달할 수 있습니다.

app.listen(port, () => {
  console.log(`Server listening on port ${port}`);
});
```


