## GitHub API 활용 (2)
> ajax 방식에서 페이지 갱신 방식으로 변경

<br>

**routes/github.js 수정**
  - 데이터를 직접 리턴하던 형태에서, 뷰와 데이터를 같이 리턴하도록

```node
//...
-   res.send(data);
+   res.render('github', {data: data, q: req.query.q});
//...
```

<br>

**views/github.jade 수정**
  - jade를 거의 처음 사용하지만 [레퍼런스](http://jade-lang.com/reference/attributes/) 참고하니 크게 어려울 것은 없었다.
  - 뷰 코드 작성 시 태그를 열고 닫는 등의 단순 반복 타이핑이 확 줄었다.


```jade
extends layout

block content
  h2 GitHub API 활용
    form.form-inline
      div.form-group
        input#source.form-control(name='q', value=q) 
        button#sumbitBtn.btn.btn-primary(type='submit') 검색
  hr
  if data
    if data.total_count < 1
      h4 검색 결과가 없습니다.
    else 
      table.table.table-bordered.table-hover
        thead
          tr
            th name
            th description
            th(style={width:'10%'}) language
            th(style={width:'8%'}) star
            th(style={width:'8%'}) watch
        tbody
          each item in data.items
            tr
              td
                a(target='_blank', href=item.html_url)= item.full_name
              td= item.description
              td= item.language
              td= item.stargazers_count
              td= item.watchers_count
  script.
    $(function(){
      $('#source').focus();
    });
    
```

<br>

현재까지의 코드 :  
https://github.com/hangaebal/dictionary-node-express/tree/github-api2
