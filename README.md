# Blazor Authentication
> 부제: Blazor ID 구조와 외부 인증(OAuth) 끝내기

Blazor(Wasm) **Identity** 구조와 **OAuth** 인증에 대한 개념적인 설명과 샘플코드를 제공합니다.
<br>

## Contents
- [개요](#개요)
- [ID (Identity)](#id)
- [OAuth (Authentication)](#oauth)
- [개발환경](#개발환경)
- [IDE](#IDE)
- [프로젝트 생성](#hosted)
- [필수 어셈블리](#필수-어셈블리)
- [마이그레이션](#마이그레이션)
- [데이터베이스](#데이터베이스)
- [인증: 구글](#구글-인증)
- [인증: 페이스북](#페이스북-인증)
- [인증: 트위터](#트위터-인증)
- [인증: 애플](#애플-인증)
- [인증: 깃허브](#깃허브-인증)
- [인증: 카카오톡](#카카오톡-인증)
- [스케폴딩](#스케폴딩)
- 스케폴딩 구조 기반 실습
- 빈 프로젝트 기반 실습
<br>

***

## 개요
이 레포지토리는 **Blazor WebAssembly(WASM) 환경을 기반**으로 서버/클라이언트가 분리된 Hosted 구조에서 계정(ID)과 인증(OAuth)를 구현하는 방법에 대한 내용을 다루고 있습니다.

✔️ 세부 개념적인 내용은 아래와 같이 크게 4개로 나누어 설명합니다.  
1. ID(Identity) 스케폴더   
1. 데이터베이스 마이그레이션  
1. 엔터티프레임워크   
1. 주요 공급자(Provider) 인증

✔️ 소스코드는 아래 두 가지 버전으로 제공됩니다.
1. ID(Identity) 스케폴더를 제공받아 구현하는 방식    
1. 빈 프로젝트에서 새로 구현하는 방식

#### ❗ 학습 순서에 대한 안내
빈 프로젝트에서 처음부터 구현하기 위해서는 ID(Identity) 스케폴더 구조가 충분히 숙련된 상태에서 시작하는 것이 좋습니다. 스케폴더 구조가 생각보다 양이 많고 생소한 개념이 많아 진입장벽을 느낄 수 있기 때문에 스케폴더 구조를 더욱 자세하게 익히는 것을 권장합니다.

<br>

## ID
Blazor는 프로젝트를 생성하는 과정에서 **완성된 ID(Identity)** 모듈 사용 여부를 선택할 수 있습니다.  
그리고 이 모듈은 **닷넷** 팀에서 제공하며 신뢰할 수 있는 좋은 구조를 가집니다.

#### _이 ID 모듈을 선택하는 이유는..._
- 계정/인증과 관련해서 어렵고 복잡한 구현 부분을 도움 받을 수 있다
- 스케폴더 기법을 통해 원하는 부분만 편집이 가능
- DB 구조를 받을 수 있다 (마이그레이션)

#### _하지만 ID(Identity) 구조를 반드시 사용해야 하는 것은 아닙니다!_  
- 이미 존재하는 DB를 사용해야 하는 경우
- 독창적인 새 ID 인증 구조를 직접 만드는 경우
- 필요한 기능만을 구현하고자 하는 경우

<br>

## OAuth
유명 공급자(Provider)를 통해 제공되는 OAuth 인증 방식은 어느덧 인증 방식의 표준으로 자리잡았습니다.

🔐 **대표적인 OAuth 공급자**  

![](https://img.shields.io/badge/-Google-4285F4?style=for-the-badge&logo=Google&logoColor=white)
![](https://img.shields.io/badge/-Apple-000000?style=for-the-badge&logo=Apple&logoColor=white)
![](https://img.shields.io/badge/-Facebook-1877F2?style=for-the-badge&logo=Facebook&logoColor=white)
![](https://img.shields.io/badge/-Github-181717?style=for-the-badge&logo=Github&logoColor=white)
![](https://img.shields.io/badge/-Twitter-1DA1F2?style=for-the-badge&logo=Twitter&logoColor=white)
![](https://img.shields.io/badge/-Kakao-FFCD00?style=for-the-badge&logo=KakaoTalk&logoColor=black)
![](https://img.shields.io/badge/-Naver-03C75A?style=for-the-badge&logo=Naver&logoColor=white)

<br>

## 개발환경
- Microsoft Windows
- Apple MacOS
- Linux

Blazor는 Windows OS를 기반으로 한 닷넷프레임워크 환경에서 탈피한 **닷넷코어** 기반의 **웹앱**입니다. 따라서 더 이상 닷넷프레임워크에만 의존하지 않고 모든 운영체제 환경에서 동일한 소스코드를 쓸 수 있기 때문에 운영체제간 협업 역시 가능합니다. 또한 기존 `ASP.NET`처럼 IIS에 의존하지 않고 손쉽게 독립(Self-contained)적인 배포가 가능합니다.

<br>

## IDE
- Visual Studio (2022 Preview)
- Visual Studio for Mac
- Visual Studio Code

Visual Studio 뿐만 아니라 Visual Studio Code에서도 Blazor를 개발할 수 있습니다. 따라서 여러분이 익숙한 IDE를 선택하는 것이 좋습니다.

<br>

## 호스팅 
- Microsoft Azure
- Amazon AWS
- Server Hosting
- 직접 (노트북, 데스크톱, 라즈베리파이 등)

Blazor를 서비스하기 위한 가장 쉬운 방법은 클라우드 서비스를 이용하는 것입니다. 대표적인 클라우드 서비스로는 `Azure`와 `AWS`가 있습니다. `Azure`의 경우 무료로 웹앱을 서비스할 수 있기 때문에 개발단계에서 아주 유용하게 이용할 수 있습니다.

<br>

## 프로젝트 생성
Blazor 프로젝트를 생성하는 방법입니다.

### _Visual Studio_
1. **Blazor WebAssembly App** 프로젝트 선택
1. **Individual Accounts** 옵션 선택
1. **ASP.NET Core Hosted** 옵션 선택
<img src="https://user-images.githubusercontent.com/74305823/126189005-68a0c946-ce2f-455f-b2a1-25263bfd06d2.png" width="800"/>

### _Visual Studio Code / NET Core CLI_
```cli
dotnet new blazorwasm -au Individual -ho -o {APP NAME}
```
> ❗ 자세한 **dotnet new** 명령어는 [**여기**](https://docs.microsoft.com/ko-kr/dotnet/core/tools/dotnet-new)를 통해 학습할 수 있습니다.
<br>

## 필수 어셈블리
인증시스템을 구현하기 위해 필요한 **필수** **어셈블리**를 살펴보겠습니다.  
기본적으로 세팅되어있는 어셈블리 항목들을 살펴보면 Blazor의 인증 구조가 어떻게 제공되고 있는지 파악하는 것에 큰 도움이 됩니다.

#### ✔️ 클라이언트
▪️ &nbsp; Microsoft.AspNetCore.Components.WebAssembly  
▪️ &nbsp; Microsoft.AspNetCore.Components.WebAssembly.DevServer  
▪️ &nbsp; Microsoft.AspNetCore.Components.WebAssembly.Authentication  
▪️ &nbsp; Microsoft.Extensions.Http

#### ✔️ 서버
▪️ &nbsp; Microsoft.AspNetCore.Authentication.Google  
▪️ &nbsp; Microsoft.AspNetCore.Components.WebAssembly.Server  
▪️ &nbsp; Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore  
▪️ &nbsp; Microsoft.AspNetCore.Identity.EntityFrameworkCore  
▪️ &nbsp; Microsoft.AspNetCore.Identity.UI  
▪️ &nbsp; Microsoft.AspNetCore.ApiAuthorization.IdentityServer  
▪️ &nbsp; Microsoft.EntityFrameworkCore.SqlServer  
▪️ &nbsp; Microsoft.EntityFrameworkCore.Tools

<br>

## 데이터베이스 마이그레이션
> **Blazor Individual 모드**는 Authentication 인증 관련 데이터베이스 마이그레이션을 지원합니니다.

### _마이그레이션이란?_
필요한 데이터베이스를 현재 버전으로 자동 변경되도록 하는 기능입니다. Authentication 관련 인증 처리는 데이터베이스가 필수로 필요하기 때문에 반드시 데이터베이스가 먼저 준비되어있어야 합니다.

> DB 연결정보

```
Data Source=<IP-ADDRESS>;Initial Catalog=<DB-NAME>;User Id=<ACCOUNT>;Password=<PASSWORD>
```

> 파일 위치: **Server > appsettings.json**

```json
"ConnectionStrings": {
  "DefaultConnection": "Data Source=.;Initial Catalog=blazor-db;User Id=sa;Password=!@#$1234"
},
```
<br>

### _마이그레이션 시점은 언제이며 어떻게 동작합니까?_

**PM Console에서 실행**  
PM(패키지매니저) Console에서 `update-database`를 입력하면 마이그레이션을 시작하게 됩니다.
```terminal
PM > update-database
```

```
Build started...
Build succeeded.
Done.
```
 
 <br />
 
**브랴우저에서 실행**  
인증 수행이 진행될 때마다 데이터베이스 연결 및 마이그레이션 버전 정보를 확인합니다. 만약 생성된 데이터베이스가 없거나 새로운 버전의 마이그레이션이 필요할 경우에는 아래처럼 버튼이 활성화됩니다.

![image](https://user-images.githubusercontent.com/52397976/127735541-8c96bc30-c509-476a-b414-245bbbe877cf.png)

버튼을 클릭하면 엔터티를 통해 데이터베이스 마이그레이션 작업이 실행됩니다.

<br />

### _마이그레이션 형식_
마이그레이션은 EntityFramework 형태로 준비되어 있습니다. 그리고 서버 환경에 따라 `MS-SQL`, `SQLite`, `Oracle` 등 [**엔터티프레임워크를 지원하는 DB**](https://docs.microsoft.com/en-us/ef/core/providers/?tabs=dotnet-core-cli)를 선택할 수 있습니다.

<br>

## 데이터베이스 테이블
마이그레이션을 통해 생성된 데이터베이스 테이블 다이어그램입니다.

![image](https://user-images.githubusercontent.com/74305823/126245049-35ae709d-7589-4d8d-a3b2-4dad483c69b0.png)


#### 데이터베이스 테이블 _(.NET6.0 기준)_
|| 테이블명|설명|
|:---:|:-----|:---|
|1|__EFMigrationsHistory| 마이그레이션 히스토리 테이블 |
|2|AspNetRoleClaims| |
|3|AspNetRoles| |
|4|AspNetUserClaims| |
|5|AspNetUserLogins| 인증 제공자(예, 구글) 정보 |
|6|AspNetUserRoles| |
|7|AspNetUsers| |
|8|AspNetUserTokens| |
|9|DeviceCodes| |
|10|PersistedGrants| |

<br>

## OAuth
OAuth 방식은 구글, 페이스북, 트위터, 깃허브 등의 대규모 그룹에서 널리 쓰이는 표준 인증 방식으로, 사용자들이 타사 애플리케이션이나 웹사이트의 계정에 관한 정보를 공유할 수 있게 허용합니다.

> [OAuth Providers 확인](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers#Providers)

<br>

### 인증키 샘플
공급자(Provider)로부터 발급 받은 인증 정보(`ClientId`, `ClientSecret`)를 관리하는 형식입니다. 
> **server > appsettings.json**

```json
"Authentication": {
  "Google": {
    "ClientId": "xxx-xxx.apps.googleusercontent.com",
    "ClientSecret": "xxx-xxxxxx"
  }
},
```
[소스보기](https://github.com/devncore/blazor-authentication)

<br>

### 구글 인증
구글 계정 인증(Auth2.0)은 [Google Developer](https://developer.google.com)에서 발급받을 수 있습니다.

[![NuGet](https://buildstats.info/nuget/Microsoft.AspNetCore.Authentication.Google?includePreReleases=false)](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google/ "Download Microsoft.AspNetCore.Authentication.Google from NuGet.org")

```terminal
PM> install-package Microsoft.AspNetCore.Authentication.Google
```

```csharp
.AddGoogle(o =>
    {
        o.ClientId = Configuration["Authentication:Google:ClientId"];
        o.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
    });
```

<br />

### 페이스북 인증
페이스북 계정 인증(Auth2.0)은 [Facebook Developer](https://developers.facebook.com)에서 발급받을 수 있습니다.

[![NuGet](https://buildstats.info/nuget/Microsoft.AspNetCore.Authentication.Facebook?includePreReleases=false)](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook/ "Download Microsoft.AspNetCore.Authentication.Facebook from NuGet.org")

```terminal
PM> install-package Microsoft.AspNetCore.Authentication.Facebook
```

```csharp
.AddGoogle(o =>
    {
        o.ClientId = Configuration["Authentication:Facebook:ClientId"];
        o.ClientSecret = Configuration["Authentication:Facebook:ClientSecret"];
    });
```

<br />

### 트위터 인증
트위터 계정 인증(Auth2.0)은 [Twitter Developer](https://developer.twitter.com)에서 발급받을 수 있습니다.

[![NuGet](https://buildstats.info/nuget/Microsoft.AspNetCore.Authentication.Twitter?includePreReleases=false)](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/ "Download Microsoft.AspNetCore.Authentication.Twitter from NuGet.org")

```terminal
PM> install-package Microsoft.AspNetCore.Authentication.Twitter
```

```csharp
.AddGoogle(o =>
    {
        o.ConsumerKey = Configuration["Authentication:Twitter:ClientId"];
        o.ConsumerSecret = Configuration["Authentication:Twitter:ClientSecret"];
    });
```

<br />


### 애플 인증
> 애플은 API를 발급받기 위해 먼저 [Apple Developer Program](https://developer.apple.com/programs/)을 통해 개발자 등록 신청이 절차를 걸쳐야 합니다.

<br/>

![image](https://user-images.githubusercontent.com/52397976/127790316-4fcea24b-30a8-4c73-94d0-bf67e7033466.png)

애플 계정 인증(Auth2.0)은 [Apple Developer](https://developer.apple.com)에서 발급받을 수 있습니다.

[![NuGet](https://buildstats.info/nuget/AspNet.Security.OAuth.Apple?includePreReleases=false)](https://www.nuget.org/packages/AspNet.Security.OAuth.Apple/ "Download AspNet.Security.OAuth.Apple from NuGet.org")

```terminal
PM> install-package AspNet.Security.OAuth.Apple
```

```csharp
.AddGoogle(o =>
    {
        o.ClientId = Configuration["Authentication:Apple:ClientId"];
        o.ClientSecret = Configuration["Authentication:Apple:ClientSecret"];
    });
```

<br />

### 깃허브 인증
깃허브 계정 인증(Auth2.0)은 [GitHub Developer](https://docs.github.com/en/developers/apps/building-oauth-apps)에서 발급받을 수 있습니다.

[![NuGet](https://buildstats.info/nuget/AspNet.Security.OAuth.GitHub?includePreReleases=false)](https://www.nuget.org/packages/AspNet.Security.OAuth.GitHub/ "Download AspNet.Security.OAuth.GitHub from NuGet.org")
```
PM> install-package AspNet.Security.OAuth.GitHub
```

```csharp
.AddGitHub(o => 
    {
        o.ClientId = Configuration["Authentication:GitHub:ClientId"];
        o.ClientSecret = Configuration["Authentication:GitHub:ClientSecret"];
    });
```

<br>

### 카카오톡 인증

카카오톡 계정 인증(Auth2.0)은 [Kakao Developer](https://developers.kakao.com/)에서 발급받을 수 있습니다.

[![NuGet](https://buildstats.info/nuget/AspNet.Security.OAuth.KakaoTalk?includePreReleases=false)](https://www.nuget.org/packages/AspNet.Security.OAuth.KakaoTalk/ "Download AspNet.Security.OAuth.KakaoTalk from NuGet.org") 

```terminal
PM> install-package AspNet.Security.OAuth.KakaoTalk
```

```csharp
.AddKakaoTalk(o => 
    {
        o.ClientId = Configuration["Authentication:Kakao:ClientId"];
        o.ClientSecret = Configuration["Authentication:Kakao:ClientSecret"];
    });
```

<br>

### 네이버 인증

네이버 계정 인증(Auth2.0)은 [Naver Developers](https://developers.naver.com/apps/#/wizard/register)에서 발급받을 수 있습니다.

[![NuGet](https://buildstats.info/nuget/AspNet.Security.OAuth.KakaoTalk?includePreReleases=false)](https://www.nuget.org/packages/AspNet.Security.OAuth.KakaoTalk/ "Download AspNet.Security.OAuth.KakaoTalk from NuGet.org") 

```terminal
PM> install-package AspNet.Security.OAuth.Naver
```

```csharp
.AddNaver(o => 
    {
        o.ClientId = Configuration["Authentication:Naver:ClientId"];
        o.ClientSecret = Configuration["Authentication:Naver:ClientSecret"];
    });
```

<br>

## JWT 인증
JWT란 Json Web Token의 약자로써 이것은 웹상에서 서명과 인증 암호화 데이터를 만들기 위한 표준 기술입니다. Blazor Identity 모듈에서도 이 표준기술을 로그인 유저 세션으로 사용합니다.

<br>

## 스케폴딩
특정 구현 방식을 라이브러리(RCL) 형태로 제공받아 사용하고 필요한 부분을 스캐폴딩 하여 수정하는 것을 말합니다. 특히 스케폴딩을 통해 생성된 파일은 RCL보다 우선 적용되도록 설계되어 있으므로 재정의에 특화된 구조입니다. 또한 닷넷에서 제안하는 구현 방식을 토대로 학습 또는 확장이 용이하다는 장점을 가지고 있습니다.

<br>

## Identity 모듈
Identity 모듈은 웹 인증과 계정관리를 하나로 제공하는 라이브러리(RCL) 입니다. 

<details open>
  <summary><b>Account</b></summary>

  &nbsp;&nbsp; \- StatusMessage  
  &nbsp;&nbsp; \- ConfirmEmailChange  
  &nbsp;&nbsp; \- ForgetPasswordConfirmation  
  &nbsp;&nbsp; \- RegisterConfirmation  
  &nbsp;&nbsp; \- ResetPasswordConfirmation  
  &nbsp;&nbsp; \- LoginWith2fa  
  &nbsp;&nbsp; \- AccessDenied  
  &nbsp;&nbsp; \- ExternalLogin  
  &nbsp;&nbsp; \- Lockout  
  &nbsp;&nbsp; \- LoginWithReccoveryCode  
  &nbsp;&nbsp; \- ResendEmailConfirmation  
  &nbsp;&nbsp; \- ConfirmEmail  
  &nbsp;&nbsp; \- ForgetPassword  
  &nbsp;&nbsp; \- Login  
  &nbsp;&nbsp; \- Logout
</details>

<details open>
  <summary><b>Account/Manage</b></summary>

  &nbsp;&nbsp; \- Layout  
  &nbsp;&nbsp; \- ChangePassword  
  &nbsp;&nbsp; \- DownloadPersonalData  
  &nbsp;&nbsp; \- ExtarnalLogins  
  &nbsp;&nbsp; \- PersonalData  
  &nbsp;&nbsp; \- ShowRecoveryCodes  
  &nbsp;&nbsp; \- ManageNav  
  &nbsp;&nbsp; \- DeletePersonalData  
  &nbsp;&nbsp; \- Email  
  &nbsp;&nbsp; \- GenerateRecoveryCodes  
  &nbsp;&nbsp; \- ResetAuthenticator  
  &nbsp;&nbsp; \- TwoFactorAuthentication   
  &nbsp;&nbsp; \- StatusMessage  
  &nbsp;&nbsp; \- Disable2fa  
  &nbsp;&nbsp; \- EnableAuthenticator  
  &nbsp;&nbsp; \- Index  
  &nbsp;&nbsp; \- SetPassword  
  &nbsp;&nbsp; \- Register  
  &nbsp;&nbsp; \- ResetPassword
</details>

<details open>
  <summary><b>Account/Register</b></summary>

  &nbsp;&nbsp; \- Confirmation  
</details>

### 스케폴딩 요령
한 번에 모든 모듈을 스케폴딩 하는 것 보다는 수정하고자 하는 부분을 하나씩 순차적으로 스케폴딩 하는 것이 더욱 효율적입니다.

### 회원가입 플로우

- 로그인
- OAuth 인증
- 가입유무 DB 확인
- 이메일, OAuth 등록
- 확인 메일 체크
- 가입 최종 승인


__스케폴딩 임시 설명__

> Scaffold Identity in ASP.NET Core projects
> [MSDN](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/scaffold-identity?view=aspnetcore-5.0&tabs=visual-studio)

<br>

## 인증 방식
