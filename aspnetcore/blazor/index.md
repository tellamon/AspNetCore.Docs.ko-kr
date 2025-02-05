---
title: ASP.NET Core의 Blazor 소개
author: guardrex
description: ASP.NET Core 앱에서 .NET을 사용하여 대화형 클라이언트 쪽 웹 UI를 빌드하는 방법인 ASP.NET Core Blazor를 살펴봅니다.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 05/01/2019
uid: blazor/index
ms.openlocfilehash: d58115b17536cad0b3927e6d32b7dbe8db8e4b0f
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034427"
---
# <a name="introduction-to-blazor"></a>Blazor 소개

작성자: [Daniel Roth](https://github.com/danroth27) 및 [Luke Latham](https://github.com/guardrex)

Blazor를 시작합니다. 

Blazor는 .NET을 사용하여 대화형 클라이언트 쪽 웹 UI를 빌드하기 위한 프레임워크입니다.

* JavaScript 대신 C#을 사용하여 풍부한 대화형 UI를 만듭니다.
* .NET으로 작성된 서버 쪽 및 클라이언트 쪽 앱 논리를 공유합니다.
* 모바일 브라우저를 포함한 광범위한 브라우저 지원을 위해 UI를 HTML 및 CSS로 렌더링합니다.

클라이언트 쪽 웹 개발에 .NET을 사용하면 다음과 같은 이점이 있습니다.

* JavaScript 대신 C#으로 코드를 작성합니다.
* .NET 라이브러리의 기존 .NET 에코시스템을 활용합니다.
* 서버 및 클라이언트에서 앱 논리를 공유합니다.
* .NET의 성능, 안정성 및 보안을 활용합니다.
* Windows, Linux 및 macOS에서 Visual Studio를 사용하여 생산성을 유지합니다.
* 안정적이고, 기능이 풍부하고, 사용하기 쉬운 공통 언어, 프레임워크 및 도구 세트를 기반으로 빌드합니다.

## <a name="components"></a>구성 요소

Blazor 앱은 구성 요소를 기반으로 합니다.  Blazor의 구성 요소는 페이지, 대화 상자 또는 데이터 입력 양식과 같은 UI의 요소입니다. 구성 요소는 사용자 이벤트를 처리하고 유연한 UI 렌더링 논리를 정의합니다. 구성 요소는 중첩 및 재사용될 수 있습니다.

구성 요소는 [NuGet 패키지](/nuget/what-is-nuget)로 공유 및 배포될 수 있는 .NET 어셈블리에 기본 제공된 .NET 클래스입니다. 구성 요소 클래스는 일반적으로 *.razor* 파일 확장자를 가진 Razor 태그 페이지 형식으로 작성됩니다.

Blazor의 구성 요소는 경우에 따라 Razor 구성 요소라고도 합니다.  [Razor](xref:mvc/views/razor)는 개발자 생산성을 위해 설계된 HTML 태그와 C# 코드를 결합하는 구문입니다. Razor를 사용하여 [IntelliSense](/visualstudio/ide/using-intellisense) 지원으로 동일한 파일에서 HTML 태그와 C# 사이를 전환할 수 있습니다. Razor Pages 및 MVC에서도 Razor를 사용합니다. 요청/응답 모델을 중심으로 빌드된 Razor Pages 및 MVC와는 달리, 구성 요소는 특별히 클라이언트 쪽 UI 논리 및 컴퍼지션에 사용됩니다.

다음 Razor 태그는 다른 구성 요소 내에 중첩될 수 있는 구성 요소(*Dialog.razor*)를 보여 줍니다.

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="@OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#!");
    }
}
```

대화 상자의 본문 콘텐츠(`ChildContent`) 및 제목(`Title`)은 해당 UI에서 이 구성 요소를 사용하는 구성 요소에서 제공합니다. `OnYes`는 단추의 `onclick` 이벤트를 통해 트리거되는 C# 메서드입니다.

Blazor는 UI 컴퍼지션에 자연스러운 HTML 태그를 사용합니다. HTML 요소는 구성 요소를 지정하고 태그 특성은 구성 요소의 속성에 값을 전달합니다. `ChildContent` 및 `Title`은 Dialog 구성 요소(*Index.razor*)를 사용하는 구성 요소를 통해 설정됩니다.

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

대화 상자는 브라우저에서 부모(*Index.razor*)가 액세스될 때 렌더링됩니다.

![브라우저에서 렌더링되는 Dialog 구성 요소](index/_static/dialog.png)

이 구성 요소를 앱에서 사용할 경우 [Visual Studio](/visualstudio/ide/using-intellisense) 및 [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense)의 IntelliSense는 구문 및 매개 변수 완성을 통해 개발 속도를 높입니다.

구성 요소는 유연하고 효율적인 방법으로 UI를 업데이트하는 데 사용되는 ‘렌더링 트리’라는 브라우저 DOM의 메모리 내 표시로 렌더링됩니다. 

## <a name="blazor-client-side"></a>Blazor 클라이언트 쪽

Blazor 클라이언트 쪽은 .NET을 사용하여 대화형 클라이언트 쪽 웹앱을 빌드하기 위한 단일 페이지 앱 프레임워크입니다. Blazor 클라이언트 쪽은 플러그 인이나 코드 소스 간 컴파일 없이 개방형 웹 표준을 사용하며, 모바일 브라우저를 포함한 모든 최신 웹 브라우저에서 작동합니다.

웹 브라우저 내에서 .NET 코드를 실행하는 것은 [WebAssembly](http://webassembly.org)(약식 *wasm*)를 통해 가능합니다. WebAssembly는 개방형 웹 표준이고 플러그 인 없이 웹 브라우저에서 지원됩니다. WebAssembly는 빠른 다운로드와 최대 실행 속도를 위해 최적화된 압축 바이트 코드 형식입니다.

WebAssembly 코드는 JavaScript를 통해 브라우저의 전체 기능에 액세스할 수 있고, 이를 *JavaScript 상호 운용성*(또는 *JavaScript interop*)이라고 합니다. 브라우저에서 WebAssembly를 통해 실행되는 .NET 코드는 JavaScript와 동일한 신뢰할 수 있는 샌드박스에서 실행되므로, 실제로 앱이 클라이언트 머신에서 악의적인 동작을 수행할 기회를 제거합니다.

![Blazor 클라이언트 쪽에서는 WebAssembly와 함께 브라우저에서 .NET 코드를 실행합니다.](index/_static/blazor-client-side.png)

Blazor 클라이언트 쪽 앱이 빌드되고 브라우저에서 실행되는 경우:

* C# 코드 파일과 Razor 파일은 .NET 어셈블리로 컴파일됩니다.
* 어셈블리와 .NET 런타임이 브라우저에 다운로드됩니다.
* Blazor 클라이언트 쪽에서 .NET 런타임을 부트스트랩하고 앱의 어셈블리를 로드하도록 런타임을 구성합니다. Blazor 클라이언트 쪽 런타임은 JavaScript interop를 사용하여 DOM(문서 개체 모델) 조작 및 브라우저 API 호출을 처리합니다.

게시된 앱의 크기인 해당 페이로드 크기는 앱의 유용성에 중요한 성능 요소입니다.  대규모 앱은 브라우저에 다운로드되는 데 비교적 오랜 시간이 걸리므로 사용자 환경이 손상됩니다. Blazor 클라이언트 쪽에서는 페이로드 크기를 최적화하여 다운로드 시간을 줄입니다.

* [IL(중간 언어) 링커](xref:host-and-deploy/blazor/configure-linker)에서 게시하면 사용되지 않는 코드가 앱에서 제거됩니다.
* HTTP 응답이 압축됩니다.
* .NET 런타임 및 어셈블리가 브라우저에 캐시됩니다.

호스팅 모델 선택에 대한 자세한 내용과 지침은 <xref:blazor/hosting-models>를 참조하세요.

## <a name="blazor-server-side"></a>Blazor 서버 쪽

Razor는 UI 업데이트 적용 방법에서 구성 요소 렌더링 논리를 분리합니다. Blazor 서버 쪽에서는 ASP.NET Core 앱의 서버에서 Razor 구성 요소를 호스팅할 수 있도록 지원합니다. UI 업데이트는 [SignalR](xref:signalr/introduction) 연결을 통해 처리됩니다.

런타임은 브라우저에서 서버로 UI 이벤트 전송을 처리하고, 구성 요소를 실행한 후 서버에서 보낸 UI 업데이트를 브라우저에 다시 적용합니다.

Blazor 서버 쪽에서 브라우저와 통신하기 위해 사용하는 연결은 JavaScript interop 호출을 처리하는 데도 사용됩니다.

![Blazor 서버 쪽에서 서버의 .NET 코드를 실행하고 SignalR 연결을 통해 클라이언트의 문서 개체 모델과 상호 작용합니다.](index/_static/blazor-server-side.png)

호스팅 모델 선택에 대한 자세한 내용과 지침은 <xref:blazor/hosting-models>를 참조하세요.

## <a name="javascript-interop"></a>JavaScript interop

타사 JavaScript 라이브러리 및 브라우저 API를 필요로 하는 앱의 경우 구성 요소는 JavaScript와 상호 운용됩니다. 구성 요소는 JavaScript가 사용할 수 있는 모든 라이브러리 또는 API를 사용할 수 있습니다. C# 코드는 JavaScript 코드로 호출할 수 있으며, JavaScript 코드는 C# 코드로 호출할 수 있습니다. 자세한 내용은 <xref:blazor/javascript-interop>을 참조하세요.

## <a name="code-sharing-and-net-standard"></a>코드 공유 및 .NET Standard

Blazor는 [.NET Standard 2.0](/dotnet/standard/net-standard)을 구현합니다. .NET Standard는 .NET 구현에서 공통적인 .NET API의 공식 사양입니다. .NET Standard 클래스 라이브러리는 Blazor, .NET Framework, .NET Core, Xamarin, Mono, Unity 등 다양한 .NET 플랫폼 간에 공유할 수 있습니다.

웹 브라우저 내에서 적용되지 않는 API(예: 파일 시스템 액세스, 소켓 열기 및 스레딩)에서 <xref:System.PlatformNotSupportedException>을 throw합니다.

## <a name="additional-resources"></a>추가 자료

* [WebAssembly](http://webassembly.org/)
* [C# 가이드](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
