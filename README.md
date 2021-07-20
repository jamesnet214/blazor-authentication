# Blazor-WASM Authentication
Blazor WebAssembly를 통한 인증 방법을 심도 있게 분석하고 제대로 활용하기 위한 Repository입니다.  
<br>

## Contents
- [개요](#개요)
- [개발환경](#개발환경)
- [프로젝트 생성](#hosted)
- [필수 어셈블리](#필수-어셈블리)
- [마이그레이션](#마이그레이션)
- [데이터베이스](#데이터베이스)
- [스케폴딩](#스케폴딩)
- 스케폴딩 구조 기반 실습
- 빈 프로젝트 기반 실습
<br>

***

## 개요
Visual Studio를 통해 프로젝트를 생성할 때 기본적인 요소들이 자동적으로 생성되므로 빠르고 쉽게 웹앱을 구현하는 것은 가능합니다. 하지만 이로 인해 원하지 않는 불필요한 구조나 소스코드, 데이터베이스가 남게 됩니다. 또한 프로젝트의 역할과 구조, 어셈블리, 그리고 설정들을 정확하게 인지하고 사용하기 어렵기 때문에 프로젝트를 관리하고 확장하는 데에 있어 어려움이 생길 수 밖에 없을 것입니다.

**그래서** 우리는 Blazor WebAssembly 구조에서 제공하는 인증 관련 시스템을 더욱 세부적으로 다루고 설명할 것입니다.

<br>

🔐 **대표적인 OAuth 인증**

![](https://img.shields.io/badge/-Google-4285F4?style=for-the-badge&logo=Google&logoColor=white)
![](https://img.shields.io/badge/-Apple-000000?style=for-the-badge&logo=Apple&logoColor=white)
![](https://img.shields.io/badge/-Facebook-1877F2?style=for-the-badge&logo=Facebook&logoColor=white)
![](https://img.shields.io/badge/-Github-181717?style=for-the-badge&logo=Github&logoColor=white)
![](https://img.shields.io/badge/-Twitter-1DA1F2?style=for-the-badge&logo=Twitter&logoColor=white)
![](https://img.shields.io/badge/-Kakao-FFCD00?style=for-the-badge&logo=KakaoTalk&logoColor=black)
![](https://img.shields.io/badge/-Naver-03C75A?style=for-the-badge&logo=Naver&logoColor=white)

<br>

## 운영체제
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

#### _마이그레이션이란?_
기능에 필요한 데이터베이스를 해당 Blazor 설치 버전에 맞게 자동으로 생성 또는 변경합니다. Authentication 관련 인증 처리는 데이터베이스가 필수로 필요하기 때문에 반드시 데이터베이스가 먼저 준비되어있어야 합니다.

#### _마이그레이션 시점은 언제이며 어떻게 동작합니까?_
Authentication 인증을 최초에 성공하면 데이터베이스 연결 유무 및 마이그레이션 버전 정보 확인을 통해 마이그레이션이 시작됩니다. 그러므로 직접 마이그레이션을 할 필요가 없습니다.

#### _마이그레이션 형식_
마이그레이션은 EntityFramework 형태로 준비되어 있습니다. 그리고 서버 환경에 따라 `MS-SQL`, `SQLite`, `Oracle` 등 [**엔터티프레임워크를 지원하는 DB**](https://docs.microsoft.com/en-us/ef/core/providers/?tabs=dotnet-core-cli)를 선택할 수 있습니다.

<br>

## 데이터베이스 인증  관계도
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

#### 데이터베이스 연결정보 추가

> 파일 위치: **Server > appsettings.json**

```json
"ConnectionStrings": {
  "DefaultConnection": "Data Source=.;Initial Catalog=blazor-db;User Id=sa;Password=!@#$1234"
},
```

<br>

## OAuth
OAuth 방식은 구글, 페이스북, 트위터, 깃허브 등의 대규모 그룹에서 널리 쓰이는 표준 인증 방식입니다.

> OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준이다. 이 매커니즘은 여러 기업들에 의해 사용되는데, 이를테면 아마존, 구글, 페이스북, 마이크로소프트, 트위터가 있으며 사용자들이 타사 애플리케이션이나 웹사이트의 계정에 관한 정보를 공유할 수 있게 허용한다. [위키백과](https://ko.wikipedia.org/wiki/OAuth)

<br>

### 구글 OAuth 
구글은 OAuth2.0 표준 사용자 인증 방식으로 제공하고 있습니다. 자세한 방법은 하단을 참조바랍니다.

```csharp
services.AddAuthentication()
    .AddIdentityServerJwt()
    .AddGoogle(o =>
    {
	o.ClientId = Configuration["Authentication:Google:ClientId"];
	o.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
    });
```
👉 [**구글 API**](https://console.cloud.google.com) 

# JWT 인증
TBD...

<br>

## 스케폴딩

**스케폴드 목록**
- _LoginPartial
- Account
- Login
- Logout

__스케폴딩 임시 설명__

> Scaffold Identity in ASP.NET Core projects
> [MSDN](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/scaffold-identity?view=aspnetcore-5.0&tabs=visual-studio)

> ASP.NET Core 2.1에는 UI 아티팩트를 라이브러리 및 NuGet 패키지와 공유할 수 있는 Razor UI 클래스 라이브러리 가 도입되었습니다 . UI 조각과 함께 제공되는 첫 번째 공식 패키지 중 하나는 ASP.NET Core Identity입니다. 이 블로그 게시물은 스캐폴딩을 사용하여 ASP.NET Core ID의 기본 보기를 재정의하는 방법을 보여줍니다.
> [ASP.NET Core ID 스캐폴딩](https://gunnarpeipman.com/aspnet-core-identity-scaffolding/)


### OAuth 2.0 TBD...
- **`Blazor to Google`**  
- **[`Blazor to GitHub`](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers/blob/dev/samples/Mvc.Client/Startup.cs)**  
- **`Blazor to Facebook`**


