# Jekyll 적용
> https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/ 를 요약 번역



## 요구 사항 설치
> OS X 10.11 El Capitan 기준

1. ruby 설치
	- `ruby --version` 으로 확인해보니 `2.0.0p648` 버전이 설치가 되어 있음
	- Jekyll 관련 dependency 중 activesupport가 ruby 2.2.2 이상을 요구해서 설치 함  
	(`Gem::InstallError: activesupport requires Ruby version >= 2.2.2.`)
	- `brew install ruby` 후 터미널을 새로 열고 `ruby --version` 으로 확인
2. Bundler 설치
	- 해당 페이지에서는 Jekyll를 설치하고 실행하는데 Bundler를 이용하는 것을 추천한다.  
	Bundler는 gem dependency를 관리하고, Jekyll 빌드 에러를 감소시키고, 환경 관련 버그를 막아준다고 함
	- `sudo gem install bundler`로 Bundler 설치. Library 폴더 접근 권한이 필요해서 sudo 로 실행
3. git init
	- `git init my-jekyll-site-project-name` 으로 대상이 될 폴더 생성 및 git 초기화


## Bundler 이용 Jekyll 설치

1. Gemfile 생성
	- 프로젝트 root 디렉토리에서 `Gemfile` 이라는 이름의 파일을 생성하고, 아래 내용을 기입
	```
	source 'https://rubygems.org'
	gem 'github-pages', group: :jekyll_plugins
	```

2. Jekyll 와 관련 dependency를 설치한다.
	- bundle install

## Jekyll 실행

1. Jekyll 관련 파일 생성
	- `bundle exec jekyll new . --force`

2. Jekyll 실행
	- `bundle exec jekyll serve`
	- http://localhost:4000 에 접속해서 결과 확인





