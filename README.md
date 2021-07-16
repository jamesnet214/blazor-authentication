# Blazor-WASM Authentication
Blazor WebAssembly를 통한 인증 방법을 심도 있게 분석하고 제대로 활용하기 위한 Repository입니다.

## Contents
- [Overview](#overview)
- [개발환경](#개발환경)
- [Hosted](#hosted)
- [어셈블리](#어셈블리)
- [클라이언트](#클라이언트)

## Overview
프로젝트를 생성할 때 기본적인 모든 요소들이 Visual Studio를 통해 자동으로 생성되므로 빠르고 쉽게 웹앱을 구현하는 것은 가능하지만 프로젝트 역할과 구조, 그리고 어셈블리, 설정들을 정확하게 인지하고 사용하기에는 어려움이 있습니다. 또한 원하지 않는 불필요한 구조나 소스코드, 데이터베이스가 남게 됩니다. 그렇기 때문에 이를 사용하고 관리, 확장함에 있어 어려움이 생길 수 밖에 없을 것입니다.

**그래서** 우리는 블레이저 웹어셈블리 구조에서 제공하는 인증 관련 시스템을 더 세부적으로 다루고 설명할 것입니다.

**대표적인 인증**

<img src="https://user-images.githubusercontent.com/52397976/125898615-828c5e0b-fd64-4197-993b-c9ec4f2feadc.png" height="50"></img>

## 개발환경

- Visual Studio for Windows
- Visual Studio for Mac
- Visual Studio Code Anywhere


## Hosted

TBD...

## 어셈블리
인증시스템을 구현하기 위해 필요한 **최소한**의 **어셈블리**를 살펴보겠습니다.


### 클라이언트
- Microsoft.AspNetCore.Components.WebAssembly
- Microsoft.AspNetCore.Components.WebAssembly.DevServer
- Microsoft.AspNetCore.Components.WebAssembly.Authentication
- Microsoft.Extensions.Http

### 서버
- Microsoft.AspNetCore.Authentication.Google
- Microsoft.AspNetCore.Components.WebAssembly.Server
- Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
- Microsoft.AspNetCore.Identity.EntityFrameworkCore
- Microsoft.AspNetCore.Identity.UI
- Microsoft.AspNetCore.ApiAuthorization.IdentityServer
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.EntityFrameworkCore.Tools

기본적으로 셋팅되어있는 어셈블리 항목들을 살펴보면 블레이저 인증 구조가 어떻게 제공되고 있는지 파악하기에 큰 도움이 됩니다.

## 데이터베이스 마이그레이션
Blazor Indivisual 모드는 Authentication 인증 관련 데이터베이스 마이그레이션을 지원합니니다.

***마이그레이션이란?***
> 기능에 필요한 데이터베이스를 해당 Blazor 설치 버전에 맞게 자동으로 생성 또는 변경합니다. Authentication 관련 인증 처리는 데이터베이스가 필수로 필요하기 때문에 반드시 데이터베이스가 먼저 준비되어있어야 합니다.

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

(_.NET6.0 기준_)

![image](https://user-images.githubusercontent.com/52397976/125908364-1d1704ab-f36c-4578-907f-5b6b68412ca0.png)



### 엔터티프레임워크
기본적으로 제공하는 인증 구조에는 EntityFramework가 반드시 필요합니다. 필요에 따라 MS-SQL, SQLite, Oracle 등 다양한 데이터베이스를 구성할 수 있으며 EntityFramework 기술을 통해 필요한 데이터베이스 마이그레이션까지 제공되고 있습니다.

### 데이터베이스
인증을 정확하게 구현하기 위해서는 데이터베이스 마이그레이션이 필수이므로 반드시 DbConnection을 설정해주어야 합니다.

#### 1. 데이터베이스 연결정보 추가

EntityFrameworkCore 연결에 사용될 DB 커넥션 정보를 입력합니다.
파일 위치: **Server -> appsettings.json**

```json
"ConnectionStrings": {
  "DefaultConnection": "Data Source=.;Initial Catalog=blazor-db;User Id=sa;Password=!@#$1234"
},
```
EntityFrameworkCore는 다양한 데이터베이스를 지원합니다. (MS-SQL, SQLite, MySql, Oracle 등) 예제는 MS-SQL을 사용하지만 여러분은 DB 선택에 맞게 추가적으로 NugetPackage를 설치하시기 바랍니다.

엔터티에서 지원하는 DB 목록
| 데이터베이스 | 어셈블리 (Nuget Package) |
|:---|:----|
|MS-SQL|Microsoft.EntityFrameworkCore.SqlServer|
|SQLite|Microsoft.EntityFrameworkCore.SQLite|
|[Oracle](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/5.21.1/ReportAbuse)|Oracle.EntityFrameworkCore|
|[MySql](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore/8.0.22/ReportAbuse)|MySql.Data.EntityFrameworkCore|
|Cosmos|Microsoft.EntityFrameworkCore.Cosmos|

## OAuth2.0 인증
OAuth 방식은 구글, 페이스북, 트위터, 깃허브 등의 대규모 그룹에서 널리 쓰이는 표준 인증 방식입니다.

#### 2. 구글 OAuth2.0
구글은 OAuth2.0 표준 사용자 인증 방식으로 제공하고 있습니다. [구글 API](https://console.cloud.google.com) 자세한 방법은 하단을 참조바랍니다.

> OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준이다.[1] 이 매커니즘은 여러 기업들에 의해 사용되는데, 이를테면 아마존,[2] 구글, 페이스북, 마이크로소프트, 트위터가 있으며 사용자들이 타사 애플리케이션이나 웹사이트의 계정에 관한 정보를 공유할 수 있게 허용한다. [위키백과](https://ko.wikipedia.org/wiki/OAuth)

```csharp
services.AddAuthentication()
    .AddIdentityServerJwt()
    .AddGoogle(o =>
    {
	o.ClientId = Configuration["Authentication:Google:ClientId"];
	o.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
    });
```


## Project

![image](https://user-images.githubusercontent.com/74305823/125865426-09aaa9ab-17f7-4dd3-a86b-ae748ae5ae27.png)

![image](https://user-images.githubusercontent.com/74305823/125865475-9e38a65c-5156-4d1d-9a4e-8a93c0fea72a.png)

## Identity 스케폴딩 구조 사용을 위한 옵션 체크
- Invidual 선택
![image](https://user-images.githubusercontent.com/74305823/125865489-536f9886-1998-4600-9afa-d1596beda955.png)

## 스케폴딩이란?
- Identity modules

## 엔터티프레임워크 연결
- DbConnection
  ```
  Data Source=.;Initial Catalog=blazor-db;User Id=sa;Password=!@#$1234
  ```

## 테이블 마이그레이션
- 데이터테이블 마이그레이션입니다.

# Blazor to Google
# Blazor to GitHub
> https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers/blob/dev/samples/Mvc.Client/Startup.cs
# Blazor to Facebook
