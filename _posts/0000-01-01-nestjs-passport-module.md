# [passport module in NestJS](https://docs.nestjs.com/security/authentication)
## Authentication
* Passport 는 node.js authentication(이하 인증) library 이다. 
* @nestjs/passport 모듈 사용하면 된다.
* high level 에서, passport 는 몇 가지 단계를 실행한다.
  * "credentials(자격증명)"(username/password, JSON Web Token(JWT), 또는 Identity Provier 의 identity token) 을 통해서 사용자를 인증 한다.
  * (JWT 나 Express session 생성과 같은 portable token 을 발행을 통한) 인증 상태 관리
  * route handler 에서 추가로 사용하기 위해 ```Request``` 객체에 인증된 user 정보를 붙인다.
* 이 챕터에서, RESTful API server를 위한 완벽한 end-to-end 인증 솔루션을 구현한다.
## Authentication requirements
* let's flesh out our requirements.
* use case
  * clients 는 username 과 password로 인증을 시작한다.
  * 일단 인증되면, server는 JWT를 발행한다. 그리고 이 JWT는 인증 헤더에 bearer token 으로 담겨서 후속 요청에 인증용으로 사용될 수 있다.
  * 그리고, valid JWT을 갖는 요청만 접근할 수 있는 보호된 경로(protected route)를 생성한다.
* 우선 package 설치해야 한다. passport는 passport-local이라는 strategy를 제공한다. passport-local이라는 전략은 위 use case에 해당하는 username/password 인증 메커니즘을 구현한다.
```shell
$ npm install --save @nestjs/passport passport passport-local
$ npm install --save-dev @types/passport-local
```

## Implementing Passport strategies
* @nestjs/passport는 nodejs에서 사용가능한 passport 모듈을 nestjs에서 사용하기 좋게 wrapping 한 모듈이다.
* 우선 vanilla Passport 동작을 알아야 한다. vanilla Passport에서는 두 가지를 가지고 strategy를 설정한다.
  1. strategy에 한정된 옵션 집합. 예를 들어, JWT strategy에서는, token 서명을 위한 secret을 제공해야 한다.
  2. 사용자 저장소(user store)와 어떻게 상호작용하는지 passport에게 알려주는 "verify callback". 여기서는 user가 존재하는지, 그들의 credentials가 유효한지 확인한다. passport library는 validation succeed라면 이 callback이 full user를 return 하기를 기대한다. 만약 fail 이라면 null을 return하길 기대한다.(failure는 user not found 또는 passport-local에 있어서 password가 일치하지 않음 둘 다를 포함한다)
* @nestjs/passport를 가지고 PassportStrategy class를 extends 함으로써 passport strategy를 설정할 수 있다. super()를 사용해서 위의 1번 옵션을 설정할 수 있고, validate()를 사용해서 위의 2번째 옵션(callback)을 설정할 수 있다.

## etc
* middleware

# keyword
* single sign-on
* [OAuth](https://opentutorials.org/module/3668)
  * "User - 서비스 a - 서비스 b"
  * User는 서비스 b를 사용하고 있다.
  * 서비스 a는 User 의 정보를 얻어 서비스 b에 접근하고 싶은 상태다.
  * 서비스 b에서 accessToken을 발행하면 서비스 a는 OAuth를 통해 accessToken을 얻어서 서비스 b에 접근할 수 있게 된다.
* token-based credential
  * Services that expose an API often require token-based credentials to protect access.
* JWT: JSON Web Token
* identity token
* route handler
* RESTful API server: 
* bearer token: 전달자 토큰?. API에 접속하기 위해서는 access token을 API 서버에 제출해서 인증을 해야 합니다. 이 때 사용하는 인증 방법이 Bearer Authentication 입니다. 이 방법은 OAuth를 위해서 고안된 방법이고, RFC 6750에 표준명세서가 있습니다. [출처](https://gist.github.com/egoing/cac3d6c8481062a7e7de327d3709505f)