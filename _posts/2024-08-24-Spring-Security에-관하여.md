---
layout: post
title: "Spring Security에 관하여"
date: 2024-08-24 03:00:00 +09:00
categories: [Spring, SpringSecurity]
tags: 
  - Java
  - Spring
  - Security
  - SpringSecurity
  - Session
  - Cookie
  - Interceptor
  - Filter
  - JWT
  - Authorization
  - Authentication
  - 소프트웨어개발

toc: true
---


해당 프레임워크를 설명하기에 앞서 간단한 개념들을 먼저 설명하겠습니다.<br>

### <span class="subTitle">Authorization 과 Authentication </span>

- <span class="feature">인증(Authorization)</span>: 처음 서비스에 접근할 때 나의 신원을 확인하는 단계입니다. 내가 누구인지 알려주는 단계입니다. (로그인)<br>

- <span class="feature">인가(Authentication)</span>: 내가 특정 서비스에 대한 권한을 확인하는 단계입니다. 권한에 따라 접근 가능한 서비스가 다르기 때문에 이러한 작업이 필요합니다.<br>
관리자와 일반 사용자(ex. 상품 판매자, 구매자) 들은 각자의 역할에 따라 보고 행동할 수 있는 것들이 다릅니다.

### <span class="subTitle">Filter 와 Interceptor </span>

Spring Security를 깊게 알아보기에 앞서 Http 요청의 전처리를 하는 두 방법에 대해 알아보겠습니다. <br>

- <span class="feature">Filter</span>: 모든 Http요청이 들어왔을 때 가장 먼저 처리되는 Layer 입니다.<br>

- <span class="feature">Interceptor</span>: Filter 이후에 Controller로 요청이 Mapping이 되기 이전과 이후에 처리되는 Layer 입니다.<br>
<!-- 
<br><img src="/assets/img/SpringSecurity/Filter,Interceptor.png" alt="Filter,Interceptor Image" width="500"/> <br>  -->



참고로 Spring Security는 필터 기반입니다.

(자세한 내용은 이후 글에서 다루겠습니다.)




### <span class="subTitle">Spring Security란?: 보안(인증, 인가, 권한)을 담당하는 스프링 하위 프레임워크</span>

 <br><img src="/assets/img/SpringSecurity/SpringSecurity.png" alt="SpringSecurity Image" width="700"/> <br>


Spring Security는 애플리케이션의 보안을 책임지는 강력한 프레임워크입니다.<br>
가장 큰 장점은 복잡한 로직을 필요로 하지 않고 개발자가 원하는 <span class="feature">인증(Authentication)</span>과  <span class="feature">인가(Authorization)</span>에 대한 처리를 쉽게 설정할 수 있다는 것입니다.<br>
기본적으로 스프링 시큐리티는 <span class="feature">세션 기반 인증</span>을 제공합니다.<br>



#### 1. <span class="subTitle"> Spring Security의 동작 과정 </span>

<br><img src="/assets/img/SpringSecurity/process.png" alt="process Image" width="800"/>


1. **사용자 요청**: 사용자가 로그인 폼에 아이디와 패스워드를 입력하면, 해당 정보는 `HttpServletRequest` 객체에 담겨 서버로 전송됩니다. (Http 요청)
   
2. **Authentication Filter**: Spring Security의 첫 번째 관문은 `AuthenticationFilter`입니다. 이 필터는 요청을 가로채어, 사용자가 입력한 아이디와 비밀번호의 유효성을 검사합니다. 이때 유효성 검사가 성공하면, `UsernamePasswordAuthenticationToken`이라는 인증용 객체가 생성됩니다.

3. **AuthenticationManager**: 생성된 `UsernamePasswordAuthenticationToken`은 `AuthenticationManager`에게 전달됩니다. `AuthenticationManager`는 인증을 처리하는 핵심 구성 요소로, 여러 인증 제공자(`AuthenticationProvider`)들을 조합하여 인증 로직을 실행합니다.

4. **AuthenticationProvider**: `AuthenticationManager`는 전달된 인증 객체를 적절한 `AuthenticationProvider`에게 넘겨줍니다. `AuthenticationProvider`는 실제 인증 로직을 수행하며, 사용자의 아이디와 비밀번호가 올바른지 확인합니다.

5. **UserDetailsService와 UserDetails**: 사용자의 아이디가 유효한지 확인하기 위해 `UserDetailsService`가 호출됩니다. 이 서비스는 데이터베이스에서 사용자의 정보를 조회하고, 해당 정보를 `UserDetails` 객체에 담아 반환합니다.

6. **DB에서 사용자 정보 조회**: `UserDetailsService`는 DB에서 사용자 정보를 조회한 후, `UserDetails` 객체로 반환합니다. 이 객체는 사용자의 비밀번호와 권한 정보를 포함하고 있습니다.

7. **인증 처리**: `AuthenticationProvider`는 `UserDetails` 객체와 사용자가 입력한 정보를 비교하여 인증을 수행합니다. 인증에 성공하면, `SecurityContextHolder`에 인증 정보를 저장합니다.

8. **인증 성공/실패 처리**: 인증이 성공하면 `AuthenticationSuccessHandler`가, 실패하면 `AuthenticationFailureHandler`가 실행됩니다. 이 핸들러들은 인증 성공 시 리다이렉션을 처리하거나 실패 시 오류 메시지를 출력하는 등의 작업을 수행합니다.

쉽게 정리하면 
1. 로그인 정보의 유효성 검사 -> 통과시 토큰 얻음 <br>
2. 해당 토큰은 AuthenticationManager 에게 전달되고 AuthenticationManager가 관리하는 Provider들에게 토큰과 실제db 를 비교하게 시킴 (인증로직 시작)
3. Provider들은 내부 로직에서 실제 DB의 user정보와 입력된 user정보를 비교하기 위해 Service객체(UserDetailsService)를 이용함 -> Service 객체는 실제 DB의 유정정보(UserDetails)를 반환함
4. Provider들은 Service객체가 반환한 UserDetails 정보와 토큰값을 비교해봄(실제 인증)
5. 이후 결과에 따라 핸들러들 실햄됨


#### 2. Spring Security의 유연성과 확장성

Spring Security는 각 컴포넌트가 인터페이스로 추상화되어 있어, 필요에 따라 커스터마이징이 가능합니다. 예를 들어, `AuthenticationProvider`나 `UserDetailsService`를 커스터마이징하여, 애플리케이션의 특정 요구 사항에 맞는 인증 로직을 구현할 수 있습니다. 이처럼 Spring Security는 기본 제공되는 기능 외에도 유연하게 확장할 수 있는 구조를 가지고 있습니다.

#### 3. 결론

Spring Security는 애플리케이션의 보안을 강화하기 위해 요청 처리 과정에서 다양한 보안 로직을 수행합니다. 필터 기반의 구조로 설계된 Spring Security는 요청이 들어올 때마다 이를 필터링하고, 유효성 검사 및 인증 로직을 실행합니다. 또한, 필요에 따라 각 구성 요소를 커스터마이징하여 특정 요구 사항에 맞는 보안 기능을 구현할 수 있습니다. 이와 같은 Spring Security의 구조를 이해하면, 애플리케이션 보안을 더욱 견고하게 설계할 수 있을 것입니다.




##### version
    1. SpringBoot: 3.2.2
    2. SpringSecurity: 6.2.1




### 출처

1. 공식문서:  [https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html#publish-authentication-manager-bean](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html#publish-authentication-manager-bean)<br><br>


<style>
    .custom-box {
        border: 1px solid #8B4513 !important; /* 브라운 테두리로 변경 */
        padding: 20px !important;
        background-color: #f1eeed !important; /* 연한 베이지 배경색 */
        border-radius: 15px !important; /* 둥근 모서리 */
        color: #353130 !important; /* 어두운 갈색 텍스트 */
        font-weight: 700 !important; /* 굵은 글자 */
        font-size: 1.1em !important; /* 글자 크기 확대 */
        font-family: 'Helvetica Neue', Arial, sans-serif !important; /* 세련된 폰트 */
        box-shadow: 0px 6px 10px rgba(0, 0, 0, 0.4) !important; /* 더 강한 그림자 */
        transition: transform 0.3s ease, box-shadow 0.3s ease !important; /* 마우스 오버 시 애니메이션 효과 */
    }

    .custom-box:hover {
        transform: translateY(-5px) !important; /* 마우스 오버 시 박스가 살짝 위로 이동 */
        box-shadow: 0px 10px 15px rgba(0, 0, 0, 0.5) !important; /* 마우스 오버 시 그림자가 더 깊어짐 */
    }

    .custom-box .highlight {
        color: #dd3333 !important; /* 연한 붉은색 강조 텍스트 */
        font-weight: 600 !important; /* 굵은 강조 텍스트 */
        font-size: 1.2em !important; /* 강조된 텍스트 크기 확대 */
        background-color: #f1eeed !important; /* 배경과 동일한 색상 */
        padding: 5px !important;
        border-radius: 5px !important;
        border-bottom: 2px solid #8B4513 !important; /* 강조 텍스트 아래에 브라운 색상 밑줄 */
    }

    .subTitle{
      color:#f0f0f0 !important;/* 밝은 회색 텍스트 */
      font-weight: 700 !important; /* 두껍게 설정 */
    }

    .feature{
      color: #e33f3f !important;
    }



  

</style>
