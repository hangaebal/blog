*https://github.com/Eyepea/API-Hour/blob/master/README.rst 를 요약 번역*

-----

# API Hour

API Hour는 강력한 애플리케이션을 만들 수 있게 하는 경량 데몬 프레임워크이다.

매우 효과적인 데몬을 쉽게 만들기 위한 간단하고 튼튼하고 정말 빠른 서버사이드 환경에 대한 요구에 대응하여 만들어졌다.

기본적으로, API-Hour Starter Kit (Cookiecutter)는 당신을 위해 웹 서비스를 개발하기 위한 HTTP 데몬을 만든다.

API-Hour와 함께 당신은 어떤 AsyncIO 서버 라이브러리라도 실 서비스를 위해 준비된 멀티 프로세싱 데몬으로 빠르게 전환할 수 있다.



## 빠르고 적당히 만든 HTTP 벤치마크

![이미지](https://github.com/hangaebal/blog/blob/master/img/stats.png?raw=true)

규모: 400개의 동시 연결 상태에서 30초 동안 쿼리 수

벤치마크는 Dell Precision M6800 API-Hour 와 Gunicorn 16 워커로 만들었다.

상세 정보는 [링크](https://github.com/Eyepea/API-Hour/tree/master/benchmarks)



## 이런 성능을 가지게 하는 마법은 어떤것인가?

아키텍처가 툴보다 훨씬 더 중요하다.

HTTP 요청을 가능한 많이 핸들하기 위해 비동기와 멀티프로세스 패턴을 함께 결합하여 사용했다.

이상적으로는, 한계는 CPU나 메모리가 아닌 네트워크 카드여야 한다.

게다가, 당신의 코드와 비동기 소켓간의 레이어를 가능한 많이 줄이기 위해 노력했다.

각 레이어 별로 성능과 단순함을 최고의 용어로 사용했다.

  1. [AsyncIO](https://docs.python.org/3/library/asyncio.html): 쉬운 비동기 프레임워크, Python 3.4 이상에 직접 통합되었다.
  
  2. [aiohttp.web](http://aiohttp.readthedocs.org/en/latest/web.html): AsyncIO를 위한 HTTP protocol 구현채 + 웹 프레임워크
  
  3. (ujson)[https://github.com/esnme/ultrajson#ultrajson]: 가장 빠른 JSON serialization


## 예제
  
  1. API-Hour Starter Kit (Cookiecutter)
  
  2. API-Hour TechEmpower Web Framework 구현체 Benchmarks
  
  3. HTTP+SSH Daemon
  
  4. 빠르고 적당히 만든 HTTP 벤치마크
  

## API-Hour 프로젝트를 어떻게 시작하나요?

[튜토리얼](http://pythonhosted.org/api_hour/tutorials/index.html)

## 지원

  - [문서](http://pythonhosted.org/api_hour/)
  
  - [메일링 리스트](https://groups.google.com/d/forum/api-hour)

## 요구사항

  - Python 3.3+

## 설치

[공식 문서 참조](http://pythonhosted.org/api_hour/installation.html)

## 라이센스

API-Hour 는 Apache 2 license를 따른다.

## 아키텍쳐

API-Hour 는 각각의 프로세스 안에서 당신의 코드를 시작하기 위한 당신의 코드와 Gunicorn 사이의 접착제이다.

## 기원

API-Hour는 aiorest의 fork 였지만, 현재는 멀티 프로세싱을 위해 Gunicorn 에만 기반을 두고 있다.

## Thanks

Thanks to Gunicorn, aiorest, aiohttp and AsyncIO community, they made 99,9999% of the job for API-Hour.

Special thanks to Andrew Svetlov, the creator of aiorest.

## API-Hour의 목표

  1. Fast(빠른): API-Hour는 극도로 빠르기 위해 bottom-up 으로 디자인 되었고, 거대한 로드를 핸들링 가능하다. Python 3와 새롭고 강력한 AsyncIO 패키지를 사용했다.
  
  2. Scalable(확장 가능한): API-Hour는 탄력적이게 만들어졌고, 쉽게 확장 가능하다.
  
  3. Lightweight(경량)
  
    1. small codebase(작은 코드): 적게 하는 것은 더 빨라지는 것을 의미한다. 요청을 수행할 코드베이스는 가능한 작게 유지시킨다. 이런 기본 풋프린트 외에도, 당연히 더 많은 플러그인이나 패키지를 활성화, 사전 설치, 초기화 할 수 있지만, 선택은 당신 몫이다.
    
    2. flexible setup (유연한 설정): 어떤 사람은 많은 디펜던시를 사용해도 문제가 없는 반면 다른 어떤 사람들은 아무것도 가지지 않길 원한다 (파이썬 제외).
    어떤 사람들은 코딩의 용이성(그리고 속도)을 위해 약간의 성능을 잃는 것을 괜찮아 하는 반면, 다른 어떤 사람들은 기성 기능을 위해 밀리초를 희생하길 원치 않는다.
    이러한 선택은 당신 몫이고, 그래서 필수적인 추가 레이어, 플러그인, 미들웨어가 없다.
  
  4. Easy: API-Hour 는 이해하기 매우 쉽게 되는것을 의미한다: 가파른 러닝커브가 없고, 잃어야 할 문서의 산도 없다: 바로 사용할 수 있는 “Hello-world” 애플리케이션을 다운로드 하고, 거기서 당신의 애플리케이션 코딩을 즉시 시작한다.
  
  5. Packages-friendly and friendly-packages: 당신이 외부 패키지를 재작성 하지 않고 적용 시키고, "감싸거나" 끼워 넣어 사용할 수 있게 하려고 한다. 다른 한 편, API-Hour "플러그인"은 더 많은 사람들의 이득을 위해 프레임워크 밖에서 독립형 패키지로도 가능한 많이 사용 가능하도록 쓰여졌다.
  
  6. Asynchronous... or not: 만약 당신이 비동기 코드를 구성하기 위해 추가적인 복잡도를 원치 않는다면, 당신은 그럴 필요 없다 (당신은 여전히 엄청난 성능을 즐길 수 있다). 당신은 단지 요청을 전통적인 동기 방식으로 핸들 할 수 있다. 다른 한 편, 만약 당신의 프로젝트가 병렬 작업에서 이득을 얻을 수 있는 입출력이나 프로세싱이라면, 비동기의 능력 전체이다. IO, futures, corutine, task는 간단히 조작할 수 있다. 제공되는 모든 플러그인(데이터베이스 플러그인은 부분적으로)은 비동기 대응이다.


## Source code

프로젝트는 [GitHub](https://github.com/Eyepea/API-Hour)에 있다.
버그를 찾거나 라이브러리를 개선시킬 제안이 있다면 [bug tracker](https://github.com/Eyepea/API-Hour/issues) 에 이슈를 제출해 주기 바란다.


