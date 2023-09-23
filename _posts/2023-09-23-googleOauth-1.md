---
title: "Google OAuth를 활용하여 SSO 구성해보기"
date: 2023-09-23 15:29:12 +0900
categories:
  - blog
tags:
  - Google
  - OAuth
  - Security
  - SSO
 
---

`Google OAuth`를 이용하여 `Single Sign-On(SSO)`를 구성에 대한 고려가 필요해졌다.<br>
Google OAuth의 기본 원리와 함께 SSO 구성에 대해 알아보려고한다.<br><br><br>

## [01] Google OAuth의 개념
`Google OAuth`는 Google에서 제공하는 `OAuth 2.0` 기반의 인증 및 권한 부여 서비스다.<br><br>
이를 통해 사용자는 Google 계정을 통해 다양한 서드 파티 애플리케이션에 로그인하고, 해당 애플리케이션에 개인 정보의 제한된 접근을 허용할 수 있다.<br>
또한 Google Calendar API, Mail API와 같은 Google 리소스에 대한 접근 권한도 위임 받을 수 있다.<br><br><br>

## [02] Google OAuth 사용자 로그인 프로세스
Google Oauth를 통하여 로그인을 할때 다음과 같은 단계를 거친다.<br><br>

### 1. 사용자 인증 및 권한 부여
사용자는 클라이언트 애플리케이션을 통해 리소스에 접근하려 한다.<br>
이 때, Authorization Server(Google)로 리다이렉트되어 인증과 권한 부여 과정을 거친다. 사용자가 권한을 부여하면,<br> 
클라이언트 애플리케이션은 Access Token과 Refresh Token을 받게 된다.<br><br>

### 2. 리소스 접근
클라이언트 애플리케이션은 발급받은 Access Token을 이용하여 Resource Server(Google)에 사용자의 리소스에 접근한다.<br><br>

### 전체적인 흐름도

![Google OAuth 로그인](/assets/images/Oauth-1.png)
<br><br>

- **Resource Owner**: 리소스의 실질적인 소유자로서 일반적으로 사용자를 의미한다. (사용자)<br>
- **Authorization Server**: 사용자의 권한을 확인하고 토큰을 발급하는 서버를 의미한다. (Google)<br>
- **Resource Server**: 사용자의 리소스가 저장되어 있는 서버를 의미한다. (Google)<br>
- **Client**: 사용자 대신 리소스에 접근하려는 애플리케이션을 의미한다. (Client Application)<br>
- **Access Token**: 클라이언트가 리소스 서버에 접근할 때 사용하는 토큰이다.<br>
- **Redirect URI**: Authorization Server에서 인증이 성공한 후 사용자를 Redirect할 URI다.<br>
- **Authorization Code**: Authorization Server로부터 Client에게 전달되는 코드다.<br><br><br>


## [03] Google OAuth로 SSO 구성해보기

A시스템과 B시스템이 있다고 가정하고, Google OAuth를 활용하여 SSO 구성은 다음과 같이 진행하였다.<br><br>


1. A시스템에서 'USER1'로 구글 로그인: 사용자가 직접 로그인한다.<br>
2. **Redirect URI 설정:** 구글 인증 후, Redirect URL을 A시스템으로 설정한다.<br>
3. **Access Token 발급:** Redirect URL에 포함된 Authorization Code로 'USER1'의 Access Token을 발급받는다.<br>
4. **Access Token 전달:** A시스템은 'USER1'의 Access Token을 B시스템으로 전달한다.<br>
5. **이메일 인증:** B시스템은 Access Token을 이용하여 구글 API(`https://www.googleapis.com/oauth2/v3/userinfo`)로 사용자 확인을 진행,<br>
응답 값으로 'USER1'의 이메일을 받아 사용자를 확인하고 인증한다.<br><br><br>

## [04] 고려사항
구현 과정에서는 시스템의 효율성과 확장성도 고려되어야 한다. 불필요한 API 호출을 최소화하고, 캐싱 전략을 적용하여 시스템의 부하를 줄이는 방법을 고려해야한다. 
Google OAuth 방식은 기존의 사용자명/비밀번호 방식이나 Bearer 토큰을 이용한 SSO 인증에 비해 API 호출 단계가 많을 수 있으므로, 본인의 시스템 트래픽에 맞게 최적화된 방법을 적용하는 것이 중요하다고 생각한다.<br><br><br>


## [05] 결론
Google OAuth를 활용한 SSO 구성은 사용자 인증과 권한 부여를 효율적으로 관리할 수 있는 방법을 제공한다. 이러한 시스템을 성공적으로 구현하기 위해서는, 상세한 요구사항 분석과 철저한 보안 고려, 사용자 중심의 설계, 그리고 꼼꼼한 테스트 및 검증 과정이 필수적이다. 이 모든 고려사항과 원칙들이 적절히 이행되면, Google OAuth를 활용한 SSO 구성은 사용자와 조직 모두에게 보안성과 신뢰성을 제공할 수 있을 것 같다.
