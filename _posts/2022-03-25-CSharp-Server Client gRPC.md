---
title: gRPC C# Server/Client 예제
author: BeomBeomJoJo
date: 2022-03-25 11:33:00 +0800
categories: [C#, .NET6, gRPC]
tags: [C#, .NET6, gRPC]
math: true
mermaid: true
---

{% include adsense.html %}

## **참조**
* [참고 사이트](https://grpc.io/docs/languages/csharp/dotnet/)
* [참고 사이트](https://narup.tistory.com/124)
* [참고 사이트](https://narup.tistory.com/126)

<br/>

## **소개**
* C#에서 간단한 gRPC Server/Client 예제 코드를 작성해 보았습니다.


<br/>

## **개발 환경**
* .NET Version : .NET 6
* 개발 도구 : Visual Studio 2022

<br/>

## **gRPC 서버 설정**
* 먼저, Visual Studio 2022를 이용하여 gRPC 서버 프로젝트를 생성합니다.
* gRPC 서버 프로젝트를 생성하려면, 기본적으로 ASP.NET Core가 설치되어 있어야 합니다.
* 다음과 같이 gRPC 서버 프로젝트를 생성합니다.

![1](https://user-images.githubusercontent.com/22911504/160106394-88bcd601-a00b-4e4d-8ea1-bbebc633ead9.png)

<br/>

### **환경 설정**
* 기본적으로 gRPC 서버를 생성하게 되면, **Properties**, **Protos**, **Services**, **Program.cs** 폴더 및 파일들이 생성됩니다.
* 여기서 **launchSettings.json** 파일의 **applicationUrl** 만 본인이 원하는 주소로 설정합니다.

```json
{
  "profiles": {
    "gRPCServer": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "https://localhost:5001",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}

```

<br/>

## **gRPC Client 생성**
* 다음으로 C# 콘솔 프로젝트를 생성하여 **gRPC Client** 프로젝트를 생성합니다.
* 생성이 완료 되면, **Protos/greet.proto** 폴더및 proto 파일을 생성합니다.
* greet.proto 파일 내용은 아래와 같이 작성합니다.

```csharp
syntax = "proto3";

option csharp_namespace = "gRPCTest";

package greet;

// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```

<br/>

* 다음으로 Program.cs 에 아래 코드를 작성합니다.
* 참고로, **GRPC.Tools** NuGet Package 도 설치했습니다.
* GRPC.Tools NuGet Package 가 설치된 후, **gRPCclient.csproj** 프로젝트에 GRPC NuGet 패키지 관련 항목들을 추가해 줍니다.

 
```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Grpc.Net.Client" Version="2.43.0" />
    <PackageReference Include="Grpc.Tools" Version="2.44.0">
      <PrivateAssets>all</PrivateAssets>
      <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
    </PackageReference>
  </ItemGroup>

  <ItemGroup>
   <!-- protobuf 추가 -->
   <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
   <PackageReference Include="Grpc.AspNetCore" Version="2.28.0" />
   <PackageReference Include="Google.Protobuf" Version="3.11.4" />
   <PackageReference Include="Grpc.Net.Client" Version="2.31.0" />
   <PackageReference Include="Grpc.Tools" Version="2.28.1" />
  </ItemGroup>

</Project>

```

* 마지막으로 Program.cs 코드를 아래와 같이 작성합니다.

```csharp
using Grpc.Net.Client;
using gRPCTest;

using var httpClientHandler = new HttpClientHandler
{
    ServerCertificateCustomValidationCallback = HttpClientHandler.DangerousAcceptAnyServerCertificateValidator
};
using var httpClient = new HttpClient(httpClientHandler);
using var channel = GrpcChannel.ForAddress("https://localhost:5001", new GrpcChannelOptions { HttpClient = httpClient });

var client = new Greeter.GreeterClient(channel);

var reply = await client.SayHelloAsync(
        new HelloRequest { Name = "GreeterClient" });

Console.WriteLine("Greeting: " + reply.Message);
Console.WriteLine("Press any key to exit...");
Console.ReadKey();
```

<br/>

## **프로젝트 구성**
* 프로젝트 구성을 보게 되면 Server/Client 2개의 프로젝트로 구성된 것을 확인할 수 있습니다.
* 이제 Server를 먼저 실행한 후, Client를 실행하여 Server 로부터 받은 메시지가 Client 로 전송되어 출력 되는지 확인합니다.

![3](https://user-images.githubusercontent.com/22911504/160106403-43f40120-ff98-4bdb-986d-6d491360af31.png)

<br/>

## **실행 결과**
* 정상적으로 메시지가 출력 되는 것을 확인할 수 있습니다.

![2](https://user-images.githubusercontent.com/22911504/160106399-67ec50ef-172e-4dda-954b-29e9c2e8bd6b.png)

{% include adsense.html %}