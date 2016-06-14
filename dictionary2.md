## 사전 API 적용
다음에 검색 API는 있지만 사전 검색은 없어서 사용 불가

네이버에는 사전 검색 API를 지원하고 있다. 처리한도가 25,000 / 일 이지만 개발 용도로 쓰기에 무리가 없어서 선택

-----
<br>

##### [API 개발 가이드](https://developers.naver.com/docs/search/encyclopedia) 내용에 따라 진행해보자



1. 개인 애플리케이션을 등록한다. (ID, SECRET 발급에 필요)
	- 페이스북 계정 개발자 등록시 처럼 네이버 계정에 휴대폰 인증이 최초 1회 필요하다고 한다. 인증을 진행한다.
	- 대부분 기본적인 정보를 입력하고 사용할 API 권한관리에서 검색 API를 잊지말고 체크한다.
	- 이후 내 애플리케이션> 해당 앱 메뉴에서 Client ID / Client Secret 을 확인할 수 있다.

  <br>

2. API 호출 코드 작성
	- dictionary.js 파일에 호출 코드를 추가한다.
	- 일단 API 정상 동작을 확인하기 위해 뷰 코드 수정 없이 서버 자체에서 임의의 텍스트(개발)로 검색한 결과를 리턴하게 해보자
	
  ```node
    var express = require('express');
    var router = express.Router();
    
    var https = require('https');
    
    var CLIENT_ID = '발급받은 ID';
    var CLIENT_SECRET = '발급받은 SECRET';
    var API_URI = '/v1/search/encyc.xml?query=';
    
    var options = {
      host: 'openapi.naver.com',
      port: 443,
      path: API_URI,
      method: 'GET',
      headers: {'X-Naver-Client-Id':CLIENT_ID, 'X-Naver-Client-Secret': CLIENT_SECRET}
    };
    
    router.get('/', function(req, res, next) {
      var searchText = encodeURIComponent('개발');
      options.path = API_URI + searchText;
      var apiReq = https.request(options, function(apiRes) {
        console.log('STATUS: ' + apiRes.statusCode);
        apiRes.setEncoding('utf8');
        apiRes.on('data', function (chunk) {
          res.setHeader('Content-Type', 'application/xml');
          res.send(chunk);
        });
      });
      apiReq.end();
    });
    
    module.exports = router;
  ```


	- nodemon을 설치했다면 파일 수정시 서버가 자동으로 재시작 된다.

  <br>

3. 접속 확인
	- [http://localhost:3000/dictionary](http://localhost:3000/dictionary)
	- 정상적으로 API가 호출 되었다면 xml 형태의 결과가 화면에 출력된다.
