## 뷰 적용 및 API 결과 처리

1. XML parser 설치
  - 네이버 API가 xml 형태로만 리턴하므로 XML 처리 라이브러리 설치
  - `npm install --save xml2js`
  <br>

2. 뷰 수정
  - **public/stylesheets/style.css** (기존 css 제거)
  ```css
  body {
    padding: 50px 0;
  }
  ```
  
  - **layout.jade** (bootstrap 사용을 위해 div.container 추가)
  ```jade
  doctype html
  html
    head
      title= title
      link(rel='stylesheet', href='/bootstrap/dist/css/bootstrap.min.css')
      link(rel='stylesheet', href='/stylesheets/style.css')
      script(src='/jquery/dist/jquery.min.js')
      meta(name='viewport', content='width=device-width, initial-scale=1')
    body
      div.container
        block content
  ```
  <br>
  
  - **dictionary.jade** (검색어 입력 부분과 ajax 호출 부분 추가)
  ```jade
  extends layout
  
  block content
  h2 사전 API 활용
  div.form-inline
    input#source.form-control(name='q') 
    button#sumbitBtn.btn.btn-primary 검색
  hr
  div#result(style={background: '#f7f7f9', padding : '20px 50px'})
  script.
  $('#source').on('keypress', function(e){
    if (e.which == 13) {
      fnAjaxDictionary();
    }
  });
  $('#sumbitBtn').click(function(){
    fnAjaxDictionary();
  });
  
  function fnAjaxDictionary(){
    var searchText = $('#source').val().trim();
    if (searchText == '') {
      alert('검색어를 입력하세요');
      return false;
    }
    
    $.ajax({
      url: '/dictionary?q=' + $('#source').val()
    })
    .done(function(data){
      $('#result').text('');
      if (data.length == 0) {
        $('#result').append('<h4>검색 결과가 없습니다.</h4>');
      } else {
        $(data).each(function(i,row){
          var dl = '<dl class="dl-horizontal">' + 
                      '<dt><a href="'+ row.link +'" target="_blank">' + row.title + '</a></dt>' +
                      '<dd>' + row.description + '</dd>' +
                    '</dl>';
          $('#result').append(dl);
        });        
      }
  
    })
    .fail(function(jqXHR, status, e){
      $('#result').text('');
      alert(jqXHR.responseText);
    });
  }
  ```
  <br>
  
3. **dictionary.js** 수정
  ```node
  var express = require('express');
  var router = express.Router();
  
  var https = require('https');
  
  var CLIENT_ID = '발급받은 ID';
  var CLIENT_SECRET = '발급받은 SECRET';
  var API_URI = '/v1/search/encyc.xml?display=20&query=';
  
  var options = {
    host: 'openapi.naver.com',
    port: 443,
    path: API_URI,
    method: 'GET',
    headers: {'X-Naver-Client-Id': CLIENT_ID, 'X-Naver-Client-Secret': CLIENT_SECRET}
  };
  
  router.get('/', function(req, res, next) {
    if (typeof req.query.q === 'undefined') {
      res.render('dictionary', {title: 'dic'});
    } else {
      var searchText = encodeURIComponent(req.query.q);
      options.path = API_URI + searchText;
      var apiReq = https.request(options, function(apiRes) {
        var statusCode = apiRes.statusCode;
        console.log('STATUS: ' + statusCode);
        if (statusCode === 200) {
          apiRes.setEncoding('utf8');
          apiRes.on('data', function(chunk) {
            var parseString = require('xml2js').parseString;
            parseString(chunk, function(err, result) {
              res.send(result.rss.channel[0].item);
            });
          });
        } else {
          res.status(statusCode).send('Naver API error return');
        }
      });
      apiReq.end();
    }
  });
  
  module.exports = router;
  ```
  <br>
  

현재까지의 코드 :  
https://github.com/hangaebal/dictionary-node-express/tree/blog3
