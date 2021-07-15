# Blazor-WASM Authentication
블레이저 웹어셈블리를 통한 인증 방법을 심도 있게 분석하고 제대로 활용하기 위한 Repository입니다.

## OrverView
프로젝트를 생성할 때 기본적인 모든 요소들이 Visual Studio를 통해 자동으로 생성되므로 빠르고 쉽게 웹앱을 구현하는 것은 가능하지만 프로젝트 역할과 구조, 그리고 어셈블리, 설정들을 정확하게 인지하고 사용하기에는 어려움이 있습니다. 또한 원하지 않는 불필요한 구조나 소스코드, 데이터베이스가 남게 됩니다.

**그래서** 우리는 블레이저 웹어셈블리 구조에서 제공하는 인증 관련 시스템을 더 세부적으로 다루고 설명할 것입니다.


## Client Side
- Microsoft.AspNetCore.Components.WebAssembly
- Microsoft.AspNetCore.Components.WebAssembly.DevServer
- Microsoft.AspNetCore.Components.WebAssembly.Authentication
- Microsoft.Extensions.Http

## Server Side
- Microsoft.AspNetCore.Authentication.Google
- Microsoft.AspNetCore.Components.WebAssembly.Server
- Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
- Microsoft.AspNetCore.Identity.EntityFrameworkCore
- Microsoft.AspNetCore.Identity.UI
- Microsoft.AspNetCore.ApiAuthorization.IdentityServer
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.EntityFrameworkCore.Tools

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
  > `Data Source=.;Initial Catalog=blazor-db;User Id=sa;Password=!@#$1234`

## 테이블 마이그레이션
- 데이터테이블 마이그레이션입니다.

# Blazor to Google
# Blazor to GitHub
> https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers/blob/dev/samples/Mvc.Client/Startup.cs
# Blazor to Facebook
