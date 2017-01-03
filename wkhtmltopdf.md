# wkhtmltopdf

> wkhtmltopdf, wkhtmltoimage, html to pdf, html to image

- html 페이지를 이미지화 하는 모듈을 여러가지 찾던 중 css가 가장 브라우저에 가깝게 출력되는 것으로 확인되어 선택했다.


1. 설치

- http://wkhtmltopdf.org/downloads.html 페이지에서 OS에 맞는 항목을 다운로드하여 설치한다.

2. 활용

- 기본 명령어는 `wkhtmltopdf http://google.com google.pdf` 형태로 간단하게 사용할 수 있다.

- 추가 옵션은 `wkhtmltoimage -H` 명령어로 전체 옵션을 확인 할 수 있다.

  1) 실제 적용시 활용했던 주요 옵션
  - `-q` 기본 출력되는 진행 메시지를 출력하지 않도록 한다. 개발 완료 후 적용했다.
  - `-width <int>` 이미지의 가로 너비를 설정한다.

