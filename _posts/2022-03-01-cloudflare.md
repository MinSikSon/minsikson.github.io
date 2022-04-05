---
layout: post
title: "[cloudflare] cloud server 구축"
---
[cloud-flare](https://www.cloudflare.com/ko-kr/)

# 0. 기초
* [DNS 에서 CNAME과 A 레코드](https://dev.plusblog.co.kr/30)

# 1. cloudflare 계정 생성
# 2. domain 과 page 연결
## 기존 domain site 에서 도메인 네임스페이스를 cloudflare 로 변경
* GoDaddy 이동
* 
# 3. front-end 와 back-end 분리
* back-end 는 ```worker``` 라는 것을 사용해야 한다. local 에서 작업하려면 역기 wrangler 라는 것을 사용해서 작업해야 한다.
* 무료 버전에서는 js 기반 back-end 서버만 구축 가능할 듯 하다. 즉, back-end 는 전부 다시 코딩해야 한다.
## repository 하위폴더 분리해서 새로운 repository 만들기
* 참고: https://sustainable-dev.tistory.com/119
* 내 제품
* DNS 관리
* ```네임서버``` 를 cloudflare 로 변경
  * georgia.ns.cloudflare.com
  * oswald.ns.cloudflare.com
# 4. db 분리
* cloudflare 는 KV 방식으로 데이터 저장 한다. bulk 로 데이터 저장하기 위해서는 wrangler 라는 것을 사용해서 작업 해야 한다.
* 참고: https://blog.cloudflare.com/tag/mysql/
* 