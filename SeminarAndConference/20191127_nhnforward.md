# NHN TOAST FORWARD 2019

> 2019.11.27

![img](https://github.com/namjunemy/TIL/blob/master/SeminarAndConference/img/nhn_forward_2019/0.jpg?raw=true)

![img](https://github.com/namjunemy/TIL/blob/master/SeminarAndConference/img/nhn_forward_2019/1.jpg?raw=true)

## 0. 초청 연사 특강

> 한양대학교 - 김상욱 교수

### Recommandation Systems

* 추천 시스템을 적용해서 성공한 대표적인 기업
    * 아마존, 넷플릭스, 유튜브, 인스타그램 등
* 추천 시스템의 기반 기술
    * Content-based approach
        * 아이템의 컨텐츠와 사용자 히스토리의 컨텐츠를 매칭
    * Trust-based approach
        * 많은 사용자들의 신뢰도 기반
    * **Collaborative Filtering (CF) approach**
        * 협업 필터링
        * 나와 취향이 비슷한 이웃들에 의해 많이 추천된 아이템을 추천한다.

## 1.  '깃' 깔나는 Git 워크플로우 알아보기

> NHN Edu 서버개발팀 - 신승엽

![img](https://github.com/namjunemy/TIL/blob/master/SeminarAndConference/img/nhn_forward_2019/2.jpg?raw=true)

### 주요 Git 워크플로우 살펴보기

* Git flow
    * 항상 존재하는 브랜치
        * master 브랜치
        * develop 브랜치
    * 서포팅 브랜치
        * 필요할 때 생성 후 삭제하는 브랜치
        * feature 브랜치
            * 특정 feature의 개발이 완료되면 다시 develop으로 merge 이때, fast forward 하지 않도록 주의
        * release 브랜치
            * feature들이 포함된 develop 에서 따고, 릴리즈 후엔 master와 develop에서 merge
        * hotfix 브랜치
            * master에서 출발
* GitHub flow
    * Git glow는 대부분의 케이스에서는 너무 복잡하다
    * 사람들이 워크 플로우를 이해하기 쉽게 되어 실수가 없어지고 헤메지 않게 되었다고 설명함
    * **Master 브랜치**가 항상 stable한 상태여야 한다. 현재 master 그대로 배포되어도 이상하지 않은 상태로 유지해야 한다.
    * 새로운 기능을 개발할 때는, **Topic 브랜치**를 master 브랜치에서 딴다.
        * 기능 개발
        * Pull request 개설
        * 코드 리뷰 / 논의
        * 완료된 토픽 브랜치를 그대로 배포한다.
            * 토픽 브랜치 배포시 반드시 CI 빌드를 통과해야 하며
            * 락이 가능하다
            * Master 브랜치의 최신 커밋이 존재하는지 확인해서 충돌을 막는다.
        * 배포 후 이상없다면, master에서 해당 토픽 브랜치 merge
            * 이때 배포 락이 해제 된다.
* GitLab flow
    * git flow는 너무 복잡하며, github flow는 너무 간단하다
    * 지속적인 배포가 어려울 때
        * github flow는 마스터를 실서버에 그대로 배포하는 컨셉인데, 대부분의 회사에서 불가능할 수 있다.
        * 이럴경우 production 브랜치를 관리하고, production에서 master를 merge해서 배포한다.
        * 배포버전 관리에 용이하다.
    * **환경별 배포**가 필요할 때
        * master는 staging에 자동 배포되고
        * Pre-production, production 브랜치 두개를 관리할 수 있다.
    * **릴리즈 소프트웨어**일 때
        * 마스터에서 버전별 stable 브랜치를 따서 배포한 후
        * 핫픽스의 경우 master에서 수정하고 각 버전의 stable 브랜치로 cherry pick 한다.
        * 이것은 마치 리눅스에서 hotfix 하고, 각 배포판에 적용하는 것과 같은 원리이다.

### 우리는 이렇게 해요

* 브랜치 전략
    * 상황
        * **단기간의 배포 일정**인 경우
            * 배포 날짜로 관리. develop-20191121 등
        * **장기간의 배포 일정**인 경우
            * 코드네임으로 관리. develop-tomato 등
    * git flow base
    * develop 브랜치에서 위의 상황에 맞는 각 sub develop 브랜치를 따고
    * sub develop 브랜치 개발이 완료되면 QA를 진행한다.
    * 사실 이 정책의 경우 릴리즈 브랜치가 필요 없다. sub develop이 명확하다.
    * sub develop 개발 후 QA가 끝나서 배포가 완료되면, master와 develop에서 merge한다.
    * 나머지 sub develop을 현재 develop에 rebase 한다.
        * 연속되는 충돌이 있을 경우 -> 영상 참조
        * 같은 이름의 sub develop 브랜치를 새로 따고, 체리픽 활용
    * 핫픽스는 master에서 따서 완료후 master, develop merge
    * 핫픽스 브랜치 merge 후에 sub develop들을 develop에 다시 merge.
    * **현재 포털개발팀 TV줌파트에서 쓰는 방식과 매우 유사함.** (릴리즈 브랜치 관리는 협업 환경에 따라 다르다고 생각함)
* 개발 플로우
    
    ![img](https://github.com/namjunemy/TIL/blob/master/SeminarAndConference/img/nhn_forward_2019/3.jpg?raw=true)

## 2. DDD-Lite@Spring

> NHN Dooray 개발실 - 정명주
>
> '현대의 웹 애플리케이션은 여러 서브 도메인으로 이루어져 있습니다.
> 여기에서 가장 중요한 핵심 도메인은 그에 걸맞은 방법론이 필요합니다.
> 이 복잡성의 끝에서 만날 수 있는 DDD-Lite를 인기 있는 Spring 프레임워크를 통해서 알아보겠습니다.'
>
> 1. 복잡성과 위기
> 2. 지식 탐구: 위키
> 3. 구현: Model-Driven VS Data-Driven
> 4. 아키텍처와 모듈
> 5. 결론

### 복잡성과 위기

* 복잡성
    * 우리가 해결해야 할 문제 자체의 복잡성
    * 우리가 사용하는 기술과 도구의 복잡성
* 위기
    * **빠르고 간단하게 구축(일정 드리븐 개발)**
        * 문제의 출발점
    * 새로운 요구사항
    * 복잡성 증가
    * 가파른 비용증가, 급격한 생산성 감소
    * 위기를 맞고
    * 차세대 개편 + 레거시 의존
        * **잘못된 레거시에 의존하기 때문에 계속되서 악순환이 일어난다.**
* 시간이 지날수록 복잡성과 비용은 폭발적으로 우상향하게 된다.
* DDD로 이 위기를 극복해보자

### 지식 탐구

* 해결해야할 문제 = 문제 공간
* 이것을 해결 공간으로 바꿔야 한다.
* 이해 당사자의 해결해야 할 문제를 도메인 지식을 결합해서 해결 공간을 뽑아 내려고 하는 것이 DDD의 전술적 패턴이다.
* 전술적 패턴 - 큰 그림
* 전략적 패턴 - 세부 그림
* DDD 전술적 패턴 일부를 적용하는 것을 DDD-Lite라고 한다.
* **주의할 점.**
    * 엔티티는 행위가 우선한다.
    * 필드는 행위를 하는데 필요하는 경우에만 명세한다.
    * 기존에 DB 스키마 만들고 엔티티에 필드 명세하면 끝나는게 도메인 엔티티 생성이 아니다.
    * 예를 들면, create() 메서드를 먼저 생성하고 메서드에 필요한 필드를 추가 한다.
    * 연관관계도 마찬가지다. 필요한 경우에만 단방향으로 매핑하자. 
    * 중복 필드는 Value Object로 해결할 수 있다.
    * Value Object에도 행위를 명시할 수 있기 때문에 도메인이 풍부해 질 수 있다.
* Data Driven vs Domain Driven 차이점 중요.
* 도메인 서비스는 무상태.

### 아키텍처와 모듈

* 헥사고날 아키텍처
* 기술보단 도메인이 먼저다.

## 3. 레거시 웹 서비스 길들이기 : 서버 개발자의 SPA 적용기

> NHN 클라우드 프레임워크 개발팀 - 최강훈
>
> 웹 개발은 백엔드와 프런트엔드로 나눠서 전문성을 가지고 개발하는 게 트렌드입니다.
> 그러나, 이미 서비스 중인 덩치 큰 일체형 웹 서비스를 나누는 건 막막하고 두려운 일입니다.
> 이 세션에서는 서버 개발자 관점에서 SPA를 도입하여 레거시 웹 서비스라는 괴물을 길들였던 경험과 고민을 공유하고자 합니다.
>
> 1. 프롤로그: 무엇이 문제인가?
> 2. 1막: 워밍업
> 3. 2막: 본격 분리 작업
> 4. 에필로그: 1년간 운영해보니...

![img](https://github.com/namjunemy/TIL/blob/master/SeminarAndConference/img/nhn_forward_2019/4.jpg?raw=true)

### 프롤로그 : 무엇이 문제인가?

* 일체형 웹 서비스(with MVC) 방식은 기능과 화면이 적을 땐 괜찮았다.
* 점점 비대해지는 웹 서비스
    * 모두가 꺼리게 된 자바스크립트 코드
    * 페이지는 무거워지고 서스테이닝은 점점 어려워 진다.
* 어떻게 하면 될까요?
    * SPA를 도입하자.
    * 웹 페이지 구현에 필요한 모든 정적 리소스를 최초 한번 다운 받고
    * 이후 필요한 데이터는 그때그때 비동기로 받아서 화면을 만든다.
    * 하나의 페이지만 존재하고 페이지 전환 및 구성을 자바스크립트로 구현한다.
        * 장점
            * 성능 개선
            * 불필요한 네트워크 통신이 없어 진다
        * 단점
            * 검색엔진 최적화에 어려움이 있다
        * 언제 쓰면 좋은가?
            * UI 구성이 복잡하고, 데이터가 많이 필요한 웹 페이지
            * 페이지 내에 정적인 요소보다 동적인 요소가 많은 경우
            * 로그인을 필수로 해야되는 경우(검색엔진 최적화가 불필요한 경우)
        * 개편에 앞서 동료와 협업 부서 설득하기.

### 워밍업

* 무기를 고르자
    * 앵귤러, 리액트, vue.js
        * 뭐가 제일 좋을까?(X)
        * 뭐가 우리 조직에 잘 맞을까?(O)
        * 앵귤러
            * 모든걸 갖췄지만 러닝커브가 높다
        * 리액트
            * 강력한 기능을 가지고 있지만, 더 필요한 도구는 필요에 따라서 선택해야 한다.
        * vue.js
            * 최소한의 있을건 다 있고, 배우기 쉬우며, 빠르게 만들어 낼 수 있다.
* 우리 팀은 어떤가?
    * 자바 개발자가 많고, 자바스크립트 개발 비중은 상대적으로 적은 편
* Vue.js 를 골랐다
    * 러닝커브가 가장 낮고, 가이드 문서도 잘 되어 있다.
    * single file component로 개발하면 하나의 파일에서 컴포넌트 단위로 작업할 수 있다.(퍼블리싱 팀과 협업시 용이)
* 많이 변해버린 자바스크립트
    * ES6 문법이다.
        * let, const
        * Arrow function
        * import, export
        * class
        * 등 구글링해서 10분만 보자.
    * webpack 설정은 어떻게?
        * 프론트엔드 프레임워크를 도입할때 최대 고비.
        * gradle 학습할때와 비슷한 느낌으로 공부하자..

### 본격 분리 작업

* 프로젝트 구성
    * Back-end, Front-end 어떻게 구성할까?
    * 어떻게든 둘을 합쳐보자
        * Spring-boot-vuejs
            * 부트와 vuejs를 maven으로 한번에 빌드
    * 빌드 프로세스
        * 프론트 엔드 빌드
        * 백엔드 리소스 디렉토리에 copy
        * 백엔드 빌드
    * 모든 요청을 was에서 해결하기
        * static 파일인데도 was로 서비스 해야되나?
        * UI 변경이 잦은 편인데, 매번 백엔드 포함하서 빌드/배포 해야되나?
        * 전과 차리가 없는데?
        * UI요청과 API 요청을 구분하자
            * 리소스 요청은 웹서버에서
            * 나머지는 was에서
    * 결론
        * 굳이 둘을 섞어서 프로젝트를 구성할 필요가 없었다.
        * 모놀리스 개발을 하다보니 합치는 것에 강박이 있었던 것 같다.
        * **백엔드 프로젝트와 프론트엔드 프로젝트는 과감하게 나누자**
* API 호출 방식
    * 일체형 웹 서비스에서는?
        * 서버 렌더를 위해 API 명세에 맞는 VO클래스를 만들어야 한다.
        
        * 클라이언트 렌더링에서도 이렇게 해야 될까?
            
            * API Gateway 사용 - 많은 기능중 API 라우팅 기능에 집중
            * Zuul 사용
            
                * 부트에서 간단하게 적용 가능
            
                  ![img](https://github.com/namjunemy/TIL/blob/master/SeminarAndConference/img/nhn_forward_2019/5.jpg?raw=true)
            
        * 백엔드는 API Gateway의 역할을 충실하게 하게 된다.
            
            * 기존의 VO를 제거하고 json으로 쭉쭉 내리면 된다.
    * 결론
        
        * API Gateway를 적용하고 불필요한 중복 코드를 걷어내자.
* 상태 관리
    * 프론트엔드의 복잡한 컴포넌트 간 데이터 전달
    * 컴포넌트간 의존 관계가 복잡하다.
        * 컴포넌트 통신이나 이벤트 버스로는 벅차다
        * 중략 ...
        * **상태 관리 라이브러리 vuex 사용하자는 결론**
    * vuex 사용 팁
        * api 호출 횟수 줄여보기
            * lodash의 debounce를 적용하여 여러 번 호출해도 한 번만 호출되도록 구성
            * 이미 로드한 데이터라면 추가로 호출되지 않도록
        * 데이터 가공은 어떻게?
            * RxJS에는 데이터 가공을 위한 유용한 연산자가 많이 존재함
        * vuex의 state, getter는 컴포넌트뿐만 아니라 다른 곳에서도 호출이 가능하다.
            * **router에서 사용하면 데이터 로딩이 완료된 다음에 화면을 노출할 수 있음(서버 렌더링 처럼 보이는 효과)**
    * 결론
        * 상태관리 라이브러리 사용해서 중앙 집중식 데이터 관리 하자
* 다국어 처리
    * Spring MVC에서 다국어 처리
        * Spring MessageSource 사용
        * 서버 렌더 방식이다 보니 언어 변경을 하려면 반드시 새로 고침
    * 이제 다국어 처리도 프론트엔드에서
        * vue-i18n 플러그인 사용
        * 새로고침 없이도 언어 변경 가능
    * 아직 남아있는 문제점
        * 문구 수정 요청이 잦다
        * 문구 수정을 위해서는 여전히 빌드/배포가 필요하다.
    * 해결 책
        * 메시지 서비스를 만들자
        * 다국어 메세지를 관리하는 통합 메세지 서비스
        * 동일한 key를 사용하여 4개 국어 저장
        * 빌드 배포를 하지 않더라도 메세지 서비스에 요청해서 처리할 수 있다.
* 배포 환경에 따른 설정 값 처리
    * 프론트엔드에서는 배포 환경에 따라 설정값을 어떻게 처리?
        * development 
        * production
    * 실무에서는?
        * 팀, 프로젝트마다 사용중인 환경설정이 제각각
    * webpack 설정 파일의 분리
        * 각 환경별로 webpack 설정 파일 분리
        * DefinePlugin 사용하여 각 환경별로 다른 설정값 추가
        * package.json에서 각 환경에 맞게 빌드 명령어 구성

### 에필로그 : 1년간 운영해보니...

* 성능 측면
    * 확실히 줄어든 API 호출 횟수
    * 웹 리소스 최적화
        * 웹팩 빌드 최적화 되면서 리소스 용량 작아짐
* 운영 측면
    * 빌드/배포는 꼭 필요할 때 필요한 모듈만
* 개발 측면
    * 프론트엔드 프레임워크의 도입
        * 규칙과 일관성이 생긴 코드
        * ESLint가 많이 도와줌
        * 가독성 증가
        * 재사용 가능한 컴포넌트 개발 지향
    * 백엔드는 데이터에만 집중하기
        * 서버 렌더링이 없으니 데이터만 생각하면 됨
        * API Gateway의 역할에 집중

## 4. Spring Data JPA의 사실과 오해

> NHN Dooray개발실 - 신동민
>
> 1. Spring JPA의 사실과 오해
> 2. 연관 관계 맵핑에 대한 모든 것
> 3. Spring Data JPA Repository의 숨겨진(?) 기능

![img](https://github.com/namjunemy/TIL/blob/master/SeminarAndConference/img/nhn_forward_2019/6.jpg?raw=true)

### 요약

* 연관관계 매핑
    * 사실상 단방향 매핑만으로 연관관계 매핑은 이미 완료
    * 대개의 경우 단방향 매핑이면 충분하다.
    * 하지만, 일대다 단방향 연관관계 매핑에서 영속성 전이(cascade)를 사용할 경우 양방향으로 변경하자
        * 추가 update 쿼리 방지
* Spring Data JPA Repository
    * JpaRepository 상속하면 웬만한 CRUD, Paging, Sorting 메서드 사용가능
    * 메서드 이름 규칙을 통한 쿼리 생성 가능
        * 이름 규칙에 따라 interface에 메서드 선언만 하면 쿼리 생성
    * JPA Repository 메서드로도 **JOIN 쿼리 수행 가능**
        * 이름 규칙에 따라 Entity 내 연관관계 필드 탐색함
    * JPA Repository 메서드에서도 다양한 **DTO Projection 지원**
        * Dynamic DTO Projection도 가능하다.

## 5. 바르게, 빠르게! Reactive를 품은 Spring Kafka

> NHN 이병찬
>
> 1. Kafka 메시지를 비동기로 처리하는 방법
> 2. ReactiveX에서 제공하는 연산자를 활용하는 사례
> 3. Project Reactor의 내부 구조(Publisher-Subscriber 간 처리 흐름)
>
> 예제 코드 저장소 - https://github.com/EleganceLESS/nhn-forward-2019

![img](https://github.com/namjunemy/TIL/blob/master/SeminarAndConference/img/nhn_forward_2019/7.jpg?raw=true)

### 01. Kafka 그리고 Spring

* 가장 인기 있고 대중적인 스트리밍 플랫폼
* Spring에서 Kafka 쓰는 방법이 매우 간단해 졌다. JMS 리스너와 유사.
* 제약 조건
    * 1스레드당 1개의 일감을 처리할 수 있는데, 일감이 오래걸리면 병목이 발생한다.
    * 병렬로 처리할 순 있으나 동일한 문제는 여전히 존재 한다.
* 스트리밍 플랫폼의 본질?
    * producer의 publish
    * consumer의 subscribe
    * 그 사이의 stream
* Kafka를 reactive하게 사용하기 위해 **Reactor Kafka 또는 Spring Kafka 사용**
    * producer가 publish 할 때, Flux를 만들어서 stream에 밀어 넣는다.
    * consumer도 subscribe 할 때, Flux로 가져와서 처리한다.

### 02. 적용 프로젝트 소개

* 서버 모니터링 시스템에서 reactive kafka를 활용했다
* 기본 로직은 서버에서 메트릭 정보를 가져와서 메트릭DB에 넣고 그것을 보여준다.
* 그 사이에서 Detector(관찰자)가 존재하여 메트릭 정보를 활용해 사용자에게 Event를 전달한다.
* 이 때 Detector는 수 많은 서버에서 오는 메트릭을 다 관장해야 한다. 커버링 범위가 매우 크다.
    * 1000 대 이상의 서버에서 동시다발적으로 이벤트가 감지되면?
    * 해당 이벤트가 짧은 간격으로 수차례 반복하면?
* 어쨋든 이상 이벤트 통지에 지연이 발생해서는 안되며, 각 이벤트는 상호 독립성이 보장되어야 한다.

### 03. 기본 기능 구현

* 이벤트가 발생하면 Detector는 그 정보를 메세지로 만들고 스트림에 집어 넣는다.
* 이벤트 프로세서에서는 메세지를 읽고 DB에 기록 후, 필요하면 메세지 발송 요청을 보내자.

### 04. 메세지 중복 제거

* 서버에서 Dector에게 데이터 보내고, 문제가 있으면 Event에 집어 넣는다.
* 우리는 Detector가 매우 중요하므로 HA 구성을 하게 된다.
* 2대의 Detector는 2개의 input과 2개의 ouput이 발생한다. 이벤트 중복을 제거하자.
    * Flux Operator의 sampleFirst(), groupBy()를 이용해서 처리

### 05. 데이터 모아서 처리하기

* 최악의 상황의 경우? 동시다발적 또는 많은 양의 메세지 발생
* 기준 시간 동안 발생한 여러 이벤트는 하나의 메세지로 모아서 통지하자.
    * 발생하는대로 메세지를 보내게 되면 1000건 발생하면 1000개의 메세지를 받게 된다.
    * ex) 30초 단위로 버퍼에 쌓아서 1건으로.
* Flux Operator의  buffer() 이용해서 처리 가능하다.

```java
public void process() {
  consume().flatMap(this::recordToNotifyObject)
    .groupBy(Message::key)
    .flatMap(flux -> flux.buffer(Duration.ofSeconds(30))) //버퍼링 - 30초
    .flatMap(this::notify)
    .flatMap(this::saveResult)
    .subscribe();
}
```

### 06. 정해진 양 만큼만 처리

* 우리가 잘 만들었다고 쳐도, 이 시스템을 사용하는 연관 시스템이 해당 API Call을 견딜 수 있는지 확인해야 한다.
* API 서버도 reactive하게 만들었다. 해결 됐지 않나?
    * **결국 그 요청은 DB가 다 받는다.**
* 발송 시스템의 경우에도 과도하게 요청을 다 보내면 과금이 어마어마하게 늘어난다.
* Custom Subscriber를 만들어서 subscribe할 때, **hookOnSubscribe()** 를 정의할 수 있다. (정해진 양 만큼 발송)

### 07. 시간을 달리는 메시지

* subscribe 시작시점에는 순차지만, 비동기기 때문에 끝나는 시점에 순서를 보장할 수 없다.
* request(4)로 요청해서 1243 순서로 끝났는데 서버가 뻗었다.
* 그 다음 요청은 3부터 시작해서 onNext() 4567을 처리하게 된다.
* 해결 - offset이 증가하는 경우에만 commit을 하자.
    * 코드는 추후 제공되는 영상 참조

### 08. 몇 가지 결론

* 많이 공부하고, 많이 고민하고, 적게 코딩하자