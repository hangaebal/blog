
## 개발 환경 준비

1. 먼저 기본 앱 구조를 편하게 잡기 위해 Express generator를 사용한다.
  - [이전 글 참조](http://hangaebal.blogspot.kr/2015/12/expressjs-express-application-generator.html)
<br><br>

2. 개발 중 소스 수정시마다 서버 재시작하려면 불편하니 [nodemon](http://nodemon.io/)을 이용
  - 개발 dependency로 설정하기 위해 --save-dev 옵션 사용
  - package.json파일에 devDependencies로 따로 관리된다.

  ```
  $ npm install nodemon --save-dev
  ```
  이후 서버 구동은
  ```
  $ nodemon
  ```
<br>

3. Code Style 확인을 위해 Linter를 사용
  - 편집기로 SublimeText3를 사용하고 있어서 SublimeLinter를 선택했다.
  - 설치 순서 : https://github.com/roadhump/SublimeLinter-eslint
  - 설치 후 설정에서 Lint Mode를 Save only로 해두면 저장할 때마다 표시를 해준다.
<br><br>


4. 의존성 관리를 위해 [bower](https://bower.io/)를 설치
	```
	$ npm install bower --save
	```
	- bower 패키지 관리를 위해 bower init 커맨드로 bower.json 파일 생성
	```
	$ bower init
	```
	
	
<br>

5. bower를 이용해서 bootstrap(+jquery) 설치
  - bootstrap 의존성에 jquery 2.2.4 버전이 포함되어있어서 같이 설치 된다.

  ```
  $ bower install bootstrap
  ```

<br>
_____

  
## 코드 작성

1. app에서 bower_components 디렉토리에 접근 가능하도록 app.js에 아래 내용을 추가

  ```
  app.use(express.static(path.join(__dirname, 'bower_components')));
  ```
<br>

2. 레이아웃에 bootstrap.css 와 jquery를 추가
  - views/layout.jade 파일 수정
  ```jade
doctype html
html
	head
		title= title
		link(rel='stylesheet', href='/bootstrap/dist/css/bootstrap.min.css')
		link(rel='stylesheet', href='/stylesheets/style.css')
		script(src='/jquery/dist/jquery.min.js')
	body
		block content
  ```
<br>

3. /dictionary 경로로 접근 가능하도록 routes와 view를 추가
  1. app.js 파일에 아래 내용 추가

		```javascript
  app.use('/dictionary', require('./routes/dictionary'));
    ```
  2. routes/dictionary.js 파일 추가 (index.js를 복사해서 약간 수정한다.)
    ```javascript
  var express = require('express');
  var router = express.Router();
  
  router.get('/', function(req, res, next) {
		res.render('dictionary', {title: 'dic'});
    
  });
  
  module.exports = router;  
    ```
  3. view/dictionary.jade 파일 추가 (마찬가지로 index.jade를 복사해서 수정한다.)
    ```jade
    extends layout
    
    block content
      h1 사전
      p Welcome to #{title}
    ```
<br>

4. 접속 확인
  - [http://localhost:3000/dictionary](http://localhost:3000/dictionary)

<br><br>



~~아직 사전 관련 기능은 하나도 없지만...~~ 일단 앱 기본 토대가 완료 되었다.

현재까지의 소스 https://github.com/hangaebal/dictionary-node-express/tree/blog1
