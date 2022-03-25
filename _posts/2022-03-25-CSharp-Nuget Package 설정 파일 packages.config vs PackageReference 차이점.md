---
title: Nuget Package 설정 파일 packages.config vs PackageReference 차이점
author: BeomBeomJoJo
date: 2022-03-25 11:33:00 +0800
categories: [C#, NuGet Package]
tags: [C#, NuGet Package]
math: true
mermaid: true
---

# Nuget Package 설정 파일 packages.config vs PackageReference 차이점 (VS 2017 이상)

<br/>

# **소개**
* **packages.config** 와 **PackageReference** 의 차이점을 알아봅니다.

<br/>

# **참조**
* https://docs.microsoft.com/ko-kr/nuget/reference/packages-config
* https://docs.microsoft.com/ko-kr/nuget/consume-packages/package-references-in-project-files
* https://docs.microsoft.com/ko-kr/dotnet/core/tools/dependencies
* https://sitecore.stackexchange.com/questions/15293/what-are-packagereferences-and-how-will-they-help-optimise-the-way-i-deal-with-n/15294

<br/>

# **packages.config 파일이란?**
* **packages.config** 파일은 프로젝트에서 참조하는 패키지 목록을 유지하기 위해 일부 프로젝트 형식에서 사용합니다.
* 이렇게 하면 모든 패키지를 포함하지 않고 빌드 서버와 같은 다른 컴퓨터에 프로젝트를 전송할 때 NuGet에서 프로젝트의 종속성을 쉽게 복원할 수 있습니다.
* **packages.config** 파일은 첫 번째 NuGet 작업이 실행 될 때 자동으로 생성 되지만, **nuget restore** 명령을 통해 수동으로도 생성 가능합니다.



# **packages.config 스키마**
* **packages.config** 의 스키마는 단순합니다. 
* 다음과 같은 표준 XML 헤더는 각 참조에 하나씩 하나 이상의 `<package>` 요소가 포함된 단일 `<packages>` 노드입니다.
* 각 `<package>` 요소에는 다음과 같은 특성이 있을 수 있습니다. 

| **Attribute**   |      **Necessary**      |  **Description** |
| ------ | ------ |  ------ |
| **id** | 예| Newtonsoft.json 또는 Microsoft.AspNet.Mvc와 같은 패키지의 식별자입니다. |
| **version** | 예 | 3.1.1 또는 3.2.5.11-beta 등 설치할 정확한 버전의 패키지입니다. |
| **targetFramework** | 아니요 | 패키지를 설치하는 경우 적용할 대상 프레임워크입니다. |
| **allowedVersions** | 아니요 | 패키지 업데이트 중에 적용되는 이 패키지에 허용된 버전의 범위입니다. |
| **developmentDependency** | 아니요 | 사용 중인 프로젝트 자체에서 NuGet 패키지를 만드는 경우 종속성을 위해 이 값을 `true` 로 설정하면 사용 중인 패키지를 만들 때 해당 패키지를 포함하지 않도록 방지합니다. 기본값은 `false` 입니다. |

# **packages.config 예시**
* VS 2019 .NET Framework 환경에서 Akka NuGet Package 설치한 예제입니다.
* **packages.config** 파일 내용은 아래와 같습니다.
* 메인 패키지인 Akka 패키지와 종속된 패키지 10개가 모두 **packages.config** 파일에 기록된 것을 확인할 수 있습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Akka" version="1.4.17" targetFramework="net472" />
  <package id="Newtonsoft.Json" version="12.0.3" targetFramework="net472" />
  <package id="System.Buffers" version="4.5.1" targetFramework="net472" />
  <package id="System.Collections.Immutable" version="5.0.0" targetFramework="net472" />
  <package id="System.Configuration.ConfigurationManager" version="4.7.0" targetFramework="net472" />
  <package id="System.Memory" version="4.5.4" targetFramework="net472" />
  <package id="System.Numerics.Vectors" version="4.5.0" targetFramework="net472" />
  <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.3" targetFramework="net472" />
  <package id="System.Security.AccessControl" version="4.7.0" targetFramework="net472" />
  <package id="System.Security.Permissions" version="4.7.0" targetFramework="net472" />
  <package id="System.Security.Principal.Windows" version="4.7.0" targetFramework="net472" />
</packages>
```

# **PackageReference 란?**
* .NET Framework에서는 패키지 참조는 **packages.config** 파일에서 관리를 했던것에 비해, .NET 5 에서는 **PackageReference** 노드를 이용하여 프로젝트 파일 내에서 직접 NuGet 종속성을 관리합니다.
* **PackageReferecne** 를 사용하면 NuGet의 다른 측면이 영향을 받지 않습니다.
* 또한 **PackageReference** 를 사용하면 MSBuild 조건을 사용하여 대상 프레임워크 또는 기타 그룹화당 패키지 참조를 선택할 수 있습니다.
* 자세한 내용은 [MSBuild 대상으로서의 NuGet pack 및 restore](https://docs.microsoft.com/ko-kr/nuget/reference/msbuild-targets) 를 참조하시면 됩니다.

# **PackageReference 형식지원**
* 기본적으로 **PackageReference** 는 Windows 10 빌드 이상을 대상으로 하는 **.NET Core, .NET Standard 및 UWP 프로젝트** 에 사용됩니다.
* .NET Framework 프로젝트는 **PackageReference** 를 지원하지만 현재 기본값은 **packages.config** 입니다.
* **PackageReference** 에 대한 자세한 내용은 [프로젝트 파일의 패키지 참조](https://docs.microsoft.com/ko-kr/nuget/consume-packages/package-references-in-project-files) 에서 확인 가능합니다.

# **PackageReference 예시**
* VS 2019 .NET 5 환경에서 Akka NuGet Package 설치한 내용입니다.
* **[+.NET 5는 VS 2019 이상에서 실행할 수 있습니다.+]**
* **packages.config** 파일이 생성되지 않고, **.csproj** 파일에 **PackageReference** 노드로 설치한 패키지가 기록됩니다.
* **packages.config** 파일과 비교를 했을 때, **.csproj** 파일의 내용이 매우 간단한 것을 확인할 수 있습니다.
  
```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Akka" Version="1.4.17" />
  </ItemGroup>

</Project>
```

# **packages.config vs PackageReference**
* .NET Framework 환경에서 NuGet Package 설치를 하면, 설치 패키지들이 **packages.config** 파일에 기록되고 관리합니다.
* VS 2017 이상부터는 **PackageReference** 노드를 사용하여 **.csproj** 파일 내에서 직접 NuGet Package 종속성을 관리할 수 있습니다.
* **PackageReference** 를 이용하는 것은  **packages.config** 파일 사용하는 것과 동일한 기능을 제공하고 모든 설정 또한 동일하게 적용됩니다.
* **PackageReference** 를 이용하면 NuGet 종속성을 훨씬 더 많이 제어할 수 있습니다.
* 예를 들어 MSBuild 조건을 사용하여 대상 프레임 워크, 구성, 플랫폼 등에 대한 패키지 참조를 선택할 수 있습니다.

# **PackageReference 사용의 이점**
* **모든 프로젝트 종속성을 한 곳에서 관리할 수 있습니다.**
* 프로젝트의 실제 종속성을 더 쉽게 볼 수 있습니다.
*  **PackageReference** 를 사용하면 **.csproj** 파일에 직접적인 종속성만 추가합니다. NuGet 패키지에 종속성이 있으면 더이상 프로젝트 파일을 복잡하게 만들지 않습니다.
*  **전역 저장소에서 패키지를 참조합니다.** 
* 패키지는 로컬 솔루션 폴더 대신 글로벌 저장소 폴더에서 참조됩니다. 이로인해 디스크 공간을 적게 사용하고 성능을 향상시킵니다.

# **정리**
* .NET Framework 환경에서 NuGet Package 설치하면 **packages.config** 파일에서 관리합니다.
* **PackageReference** 는 미래이며, **packages.config** 에 비해 많은 이점이 있습니다.
* **packages.config** 에서 **PcakageReference** 로 마이그레이션 할 수 있습니다.
* **다만 아직은 개발 과정에 있어 기본 패키지, 예제 구성 파일을 포함하는 Unicorn과 같은 Sitecore 모듈에는 제한사항이 있습니다.**