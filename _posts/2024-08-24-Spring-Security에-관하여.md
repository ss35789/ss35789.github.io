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


<div style = "font-size : 20px">해당 프레임워크를 설명하기에 앞서 간단한 개념들을 먼저 설명하겠습니다.<br></div>

### <span class="subTitle">Authorization 과 Authentication </span>
<div class="custom-box">
- <span class="feature">인증(Authorization)</span>: <br>처음 서비스에 접근할 때 나의 신원을 확인하는 단계입니다. 내가 누구인지 알려주는 단계입니다. (로그인)<br>

- <span class="feature">인가(Authentication)</span>: <br>내가 특정 서비스에 대한 권한을 확인하는 단계입니다. 권한에 따라 접근 가능한 서비스가 다르기 때문에 이러한 작업이 필요합니다.<br>
관리자와 일반 사용자(ex. 상품 판매자, 구매자) 들은 각자의 역할에 따라 보고 행동할 수 있는 것들이 다릅니다.
</div>
### <span class="subTitle">Filter 와 Interceptor </span>

<div class="custom-box">

- <span class="feature">Filter</span>: <br>모든 Http요청이 들어왔을 때 가장 먼저 처리되는 Layer 입니다.<br>

- <span class="feature">Interceptor</span>: <br>Filter 이후에 Controller로 요청이 Mapping이 되기 이전과 이후에 처리되는 Layer 입니다.<br>
<!-- 
<br><img src="/assets/img/SpringSecurity/Filter,Interceptor.png" alt="Filter,Interceptor Image" width="500"/> <br>  -->

</div>
<div style = "font-size : 20px">
<br>
참고로 Spring Security는 필터 기반입니다.
<br>
(자세한 내용은 이후 글에서 다루겠습니다.)
<br>
</div>
<br><br><br><br>


### <span class="subTitle">Spring Security란?: 보안(인증, 인가, 권한)을 담당하는 스프링 하위 프레임워크</span>

 <br><img src="/assets/img/SpringSecurity/SpringSecurity.png" alt="SpringSecurity Image" width="700"/> <br><br><br>

<div style = "font-size : 20px">
Spring Security는 애플리케이션의 보안을 책임지는 강력한 프레임워크입니다.<br>
가장 큰 장점은 복잡한 로직을 필요로 하지 않고 개발자가 원하는 <span class="feature">인증(Authentication)</span>과  <span class="feature">인가(Authorization)</span>에 대한 처리를 쉽게 설정할 수 있다는 것입니다.<br>
기본적으로 스프링 시큐리티는 <span class="feature">세션 기반 인증</span>을 제공합니다.<br>
</div>
<br><br><br><br>

#### 1. <span class="subTitle"> Spring Security의 동작 과정 </span>

<br><img src="/assets/img/SpringSecurity/process.png" alt="process Image" width="800"/><br><br><br>


1. **<span class="feature">사용자 요청</span>**: 사용자가 로그인 폼에 아이디와 패스워드를 입력하면, 해당 정보는 `HttpServletRequest` 객체에 담겨 서버로 전송됩니다. (Http 요청)<br>
   
2. **<span class="feature">Authentication Filter</span>**: Spring Security의 첫 번째 관문은 `AuthenticationFilter`입니다. 이 필터는 요청을 가로채어, 사용자가 입력한 아이디와 비밀번호의 유효성을 검사합니다. 이때 유효성 검사가 성공하면, `UsernamePasswordAuthenticationToken`이라는 인증용 객체가 생성됩니다.<br>

3. **<span class="feature">AuthenticationManager</span>**: 생성된 `UsernamePasswordAuthenticationToken`은 `AuthenticationManager`에게 전달됩니다. `AuthenticationManager`는 인증을 처리하는 핵심 구성 요소로, 여러 인증 제공자(`AuthenticationProvider`)들을 조합하여 인증 로직을 실행합니다.<br>

4. **<span class="feature">AuthenticationProvider</span>**: `AuthenticationManager`는 전달된 인증 객체를 적절한 `AuthenticationProvider`에게 넘겨줍니다. `AuthenticationProvider`는 실제 인증 로직을 수행하며, 사용자의 아이디와 비밀번호가 올바른지 확인합니다.<br>

5. **<span class="feature">UserDetailsService와 UserDetails</span>**: 사용자의 아이디가 유효한지 확인하기 위해 `UserDetailsService`가 호출됩니다. 이 서비스는 데이터베이스에서 사용자의 정보를 조회하고, 해당 정보를 `UserDetails` 객체에 담아 반환합니다.<br>

6. **<span class="feature">DB에서 사용자 정보 조회</span>**: `UserDetailsService`는 DB에서 사용자 정보를 조회한 후, `UserDetails` 객체로 반환합니다. 이 객체는 사용자의 비밀번호와 권한 정보를 포함하고 있습니다.<br>

7. **<span class="feature">인증 처리</span>**: `AuthenticationProvider`는 `UserDetails` 객체와 사용자가 입력한 정보를 비교하여 인증을 수행합니다. 인증에 성공하면, `SecurityContextHolder`에 인증 정보를 저장합니다.<br>

8. **<span class="feature">인증 성공/실패 처리</span>**: 인증이 성공하면 `AuthenticationSuccessHandler`가, 실패하면 `AuthenticationFailureHandler`가 실행됩니다. 이 핸들러들은 인증 성공 시 리다이렉션을 처리하거나 실패 시 오류 메시지를 출력하는 등의 작업을 수행합니다.<br>

<span style ="font-size: 25px">쉽게 정리하면 </span>
1. 로그인 정보의 유효성 검사 -> 통과시 토큰 얻음 <br>
2. 해당 토큰은 AuthenticationManager 에게 전달되고 AuthenticationManager가 관리하는 Provider들에게 토큰과 실제db 를 비교하게 시킴 (인증로직 시작)
3. Provider들은 내부 로직에서 실제 DB의 user정보와 입력된 user정보를 비교하기 위해 Service객체(UserDetailsService)를 이용함 -> Service 객체는 실제 DB의 유정정보(UserDetails)를 반환함
4. Provider들은 Service객체가 반환한 UserDetails 정보와 토큰값을 비교해봄(실제 인증)-> 성공시 SecurityContextHolder에 인증정보를 저장
5. 이후 결과에 따라 핸들러들 실햄됨

<br><br><br>

#### 2. <span class="subTitle">Spring Security의 유연성과 확장성</span>
<div style="font-size:20px">
Spring Security는 각 컴포넌트가 인터페이스로 추상화되어 있어, 필요에 따라 커스터마이징이 가능합니다. <br>
예를 들어, `AuthenticationProvider`나 `UserDetailsService`를 커스터마이징하여, 애플리케이션의 특정 요구 사항에 맞는 인증 로직을 구현할 수 있습니다. <br>
이처럼 Spring Security는 기본 제공되는 기능 외에도 유연하게 확장할 수 있는 구조를 가지고 있습니다.<br>
</div>
<br><br><br>

#### 3. <span class="subTitle">결론</span>
<div style="font-size:20px">
Spring Security는 애플리케이션의 보안을 강화하기 위해 요청 처리 과정에서 다양한 보안 로직을 수행합니다. <br>
필터 기반의 구조로 설계된 Spring Security는 요청이 들어올 때마다 이를 필터링하고, 유효성 검사 및 인증 로직을 실행합니다. <br>
또한, 필요에 따라 각 구성 요소를 커스터마이징하여 특정 요구 사항에 맞는 보안 기능을 구현할 수 있습니다. 이와 같은 Spring Security의 구조를 이해하면, 애플리케이션 보안을 더욱 견고하게 설계할 수 있을 것입니다.<br>
</div>
<br><br><br><br>

### <span class="subTitle">구현 예시</span>
<div style="font-size:20px">
제가 예전에 했던 토이 프로젝트의 예시입니다.<br>
따로 참조되는 객체들이나 다른 파일들의 로직이 있으니 그대로 복붙해서 이용하시면 작동이 안될 확률이 높습니다.<br><br>
</div>
LoginFilter.java

```java

@RequiredArgsConstructor
public class LoginFilter extends UsernamePasswordAuthenticationFilter {

    private final AuthenticationManager authenticationManager;
    private final UserDetailsService userDetailsService;
    private final JwtUtil jwtUtil;

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) throws AuthenticationException {
        try {
            LoginRequest loginRequest = new ObjectMapper().readValue(request.getInputStream(), LoginRequest.class);
            UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(
                    loginRequest.getEmail(), loginRequest.getPassword());
            return authenticationManager.authenticate(authenticationToken);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    protected void successfulAuthentication(HttpServletRequest request, HttpServletResponse response,
                                            FilterChain chain, Authentication authResult) throws IOException, ServletException {
        SecurityContextHolder.getContext().setAuthentication(authResult);

        String authorizationHeader = request.getHeader("Authorization");

        // JWT 토큰 발급 등의 추가적인 성공 처리 로직
        UserDetails userDetails = (UserDetails) authResult.getPrincipal();
        UserDTO userDTO = new UserDTO();
        userDTO.setEmail(userDetails.getUsername());
        userDTO.setPassword(""); // 비밀번호는 클레임에 포함하지 않습니다.

        String accessToken = jwtUtil.createAccessToken(userDTO);

        response.setHeader("Authorization", "Bearer " + accessToken);



    @Configuration
    @RequiredArgsConstructor
    public class SecurityConfig {

        private final UserDetailsService userDetailsService;
        private final PasswordEncoder passwordEncoder;
        private final JwtUtil jwtUtil;

        @Bean
        public SecurityFilterChain securityFilterChain(HttpSecurity http, AuthenticationManager authenticationManager) throws Exception {
    //        LoginFilter loginFilter = new LoginFilter(authenticationManager, userDetailsService, jwtUtil);
    //        loginFilter.setFilterProcessesUrl("/api/user/login"); // 로그인 URL 설정

            http
                    .cors(withDefaults()) // CORS 설정 추가
                    .csrf(csrf -> csrf.disable())  // CSRF 보호 비활성화
                    .authorizeRequests(authorize -> authorize
                            .requestMatchers("/api/items",
                                    "/api/items/{itemKey}",
                                    "/api/user/register",
                                    "/api/user/login",
                                    "/api/user/logout").permitAll()


                            .requestMatchers(HttpMethod.POST, "/api/items").hasRole("ADMIN")

                            .requestMatchers(HttpMethod.DELETE, "/api/items/{itemKey}").hasRole("ADMIN")

                            .anyRequest().authenticated()
                    )
                    .sessionManagement(session -> session
                            .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                    )
    //                .addFilter(loginFilter)
                    .formLogin(form -> form.disable())
                    .httpBasic(httpBasic -> httpBasic.disable());

            return http.build();
        }

        @Bean
        public CorsConfigurationSource corsConfigurationSource() {
            CorsConfiguration configuration = new CorsConfiguration();
            configuration.setAllowedOrigins(List.of("http://localhost:3000")); // 허용할 출처 설정
            configuration.setAllowedMethods(List.of("GET", "POST", "PUT", "DELETE", "OPTIONS"));
            configuration.setAllowedHeaders(List.of("Authorization", "Content-Type"));
            configuration.setAllowCredentials(true);

            UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
            source.registerCorsConfiguration("/**", configuration);
            return source;
        }
    }


        }

        @Override
        protected void unsuccessfulAuthentication(HttpServletRequest request, HttpServletResponse response,
                                                AuthenticationException failed) throws IOException, ServletException {
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            response.getWriter().write("Invalid email or password");
        }
    }
```
SecurityConfig.java

```java
@Configuration
@RequiredArgsConstructor
public class SecurityConfig {

    private final UserDetailsService userDetailsService;
    private final PasswordEncoder passwordEncoder;
    private final JwtUtil jwtUtil;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http, AuthenticationManager authenticationManager) throws Exception {
//        LoginFilter loginFilter = new LoginFilter(authenticationManager, userDetailsService, jwtUtil);
//        loginFilter.setFilterProcessesUrl("/api/user/login"); // 로그인 URL 설정

        http
                .cors(withDefaults()) // CORS 설정 추가
                .csrf(csrf -> csrf.disable())  // CSRF 보호 비활성화
                .authorizeRequests(authorize -> authorize
                        .requestMatchers("/api/items",
                                "/api/items/{itemKey}",
                                "/api/user/register",
                                "/api/user/login",
                                "/api/user/logout").permitAll()


                        .requestMatchers(HttpMethod.POST, "/api/items").hasRole("ADMIN")

                        .requestMatchers(HttpMethod.DELETE, "/api/items/{itemKey}").hasRole("ADMIN")

                        .anyRequest().authenticated()
                )
                .sessionManagement(session -> session
                        .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                )
//                .addFilter(loginFilter)
                .formLogin(form -> form.disable())
                .httpBasic(httpBasic -> httpBasic.disable());

        return http.build();
    }

    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOrigins(List.of("http://localhost:3000")); // 허용할 출처 설정
        configuration.setAllowedMethods(List.of("GET", "POST", "PUT", "DELETE", "OPTIONS"));
        configuration.setAllowedHeaders(List.of("Authorization", "Content-Type"));
        configuration.setAllowCredentials(true);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
}
```
<div style="font-size:20px">
제가 토이 프로젝트를 진행했던 당시에는 저도 Spring Security를 처음 접했던 때라 잘못된 부분이 있을 수 있습니다.<br>
저는 계속 되는 순환참조 에러 이슈로 인해서 객체들을 빈으로 빼서 따로 주입시켜주는 방식으로 해결했던 경험이 있습니다.<br>
그 때는 프로젝트의 아키텍쳐에 관해 많은 고민을 했었는데 많은 도움이 되었던 것 같습니다.<br>

<br><br>
ps. 글을 쓰면서 생각난건데 SecurityConfig 부분에 permitAll() 되었던 경로들은 인증이 없어도 접근이 가능한 경로인데<br>
처음에 저것을 필터를 지나치지 않는다고 이해했었다.<br>
permitAll()은 다른 경로와 마찬가지로 필터는 지나치지만 필터내부에서 예외처리가 발생해도 해당 api로의 함수는 정상작동되는 설정이였다.<br>
</div>

##### version
    1. SpringBoot: 3.2.2
    2. SpringSecurity: 6.2.1




### 출처

1. 공식문서:  [https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html#publish-authentication-manager-bean](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/index.html#publish-authentication-manager-bean)<br><br>
2. 블로그: [https://velog.io/@dh1010a/Spring-Spring-Security%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EA%B5%AC%ED%98%84-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-3.X-%EB%B2%84%EC%A0%84-1](https://velog.io/@dh1010a/Spring-Spring-Security%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EA%B5%AC%ED%98%84-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-3.X-%EB%B2%84%EC%A0%84-1)
3. 내 깃허브 : [https://github.com/ss35789/ShopEase-BE/issues](https://github.com/ss35789/ShopEase-BE/issues)
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
