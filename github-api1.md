## 만들던 앱을 활용해서 GitHub API 활용

네이버 백과사전 검색 API 결과가 기대했던 것과 달라서 계속 개선해나가기가 아쉬워졌다.  
(검색 결과가 특정 길이 이상 출력되지 않고 링크로 직접 가서 확인해야 한다든지..)  

다른 API 중에 괜찮은 게 없을까 찾다가 GitHub API를 선택했다.  

<br>

#### 리파지토리 검색 API 적용
  - https://developer.github.com/ Overview 페이지에 예시가 잘 나와 있다.
  - [헤더에 User-Agent 값이 필수](https://developer.github.com/v3/#user-agent-required)라는 것 이외에는 별다른 특이사항은 없다. 
  - document에서는 curl을 사용하고 있으나 curl node.js 라이브러리 중 맘에 드는 것을 찾지 못해서 기본 모듈인 https를 사용했다.

<br>

app.js 에 routes 추가
```node
...
app.use('/github', require('./routes/github'));
...
```

<br>

routes/github.js 파일 생성
```node
var express = require('express');
var router = express.Router();

var https = require('https');

var API_URI = '/search/repositories?page=1&per_page=100&q=';

var options = {
  host: 'api.github.com',
  port: 443,
  path: API_URI,
  method: 'GET',
  headers: {
    'User-Agent': 'hangaebal-Exam-App'
  }
};

router.get('/', function(req, res, next) {
  if (typeof req.query.q === 'undefined') {
    res.render('github');
  } else {
    var searchText = encodeURIComponent(req.query.q);
    options.path = API_URI + searchText;
    var apiReq = https.request(options, function(apiRes) {
      var statusCode = apiRes.statusCode;
      console.log(statusCode);
      console.log(apiRes.headers);
      apiRes.setEncoding('utf8');
      var body = '';
      apiRes.on('data', function(chunk) {
        body += chunk;
      });
      apiRes.on('end', function() {
        var data = JSON.parse(body);
        res.send(data);
      });
    });
    apiReq.on('error', (e) => {
      console.log(e);
    });

    apiReq.end();
  }
});

module.exports = router;
```

<br>

views/github.jade 파일 생성
```jade
extends layout

block content
  h2 GitHub API 활용
    div.form-inline
      input#source.form-control(name='q', value='nonono') 
      button#sumbitBtn.btn.btn-primary 검색
  hr
  table.table.table-bordered.table-hover
    thead
      tr
        th name
        th description
        th(style={width:'10%'}) language
        th(style={width:'8%'}) star
        th(style={width:'8%'}) watch
    tbody#target

  div#result(style={background: '#f7f7f9', padding : '20px 50px'})
  script.
    $('#source').on('keypress', function(e){
      if (e.which == 13) {
        fnAjaxQuery();
      }
    });
    $('#sumbitBtn').click(function(){
      fnAjaxQuery();
    });

    function fnAjaxQuery(){
      var searchText = $('#source').val().trim();
      if (searchText == '') {
        alert('검색어를 입력하세요');
        return false;
      }

      $('#target').text('');
      $.ajax({
        url: '/github?q=' + $('#source').val()
        ,success: function(data) {
          console.log(data);
          if (data.total_count < 1) {
            $('#target').append('<tr><th rowspan="5">검색 결과가 없습니다.</td></tr>');
            return false;
          }
          $(data.items).each(function(i, item) {
            var tr = 
              '<tr>' +
                '<td><a target="_blank" href="'+ item.html_url +'">' + item.full_name + '</a></td>' +
                '<td>' + item.description + '</td>' +
                '<td>' + item.language + '</td>' +
                '<td>' + item.stargazers_count + '</td>' +
                '<td>' + item.watchers_count + '</td>' +
              '</tr>';
            $('#target').append(tr);
          });
        }
        ,error: function(jqXHR, status, e) {
          console.log(jqXHR);
          console.log(e);
        }
      })
}
```

<br>

현재까지의 소스:  
https://github.com/hangaebal/dictionary-node-express/tree/github-api1
