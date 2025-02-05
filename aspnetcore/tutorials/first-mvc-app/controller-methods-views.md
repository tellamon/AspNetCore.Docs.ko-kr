---
title: ASP.NET Core의 컨트롤러 메서드 및 보기
author: rick-anderson
description: ASP.NET Core에서 컨트롤러 메서드, 보기 및 DataAnnotations를 사용하는 방법을 배웁니다.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 36c8141ba5827366572dabcfd0fdf9600c745706
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889768"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>ASP.NET Core의 컨트롤러 메서드 및 보기

작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)

동영상 앱을 적절하게 시작했지만 프레젠테이션은 이상적이지 않습니다. 예를 들어 **ReleaseDate**는 두 단어여야 합니다.

![인덱스 보기: 릴리스 날짜는 한 단어(공백 없음)이며 모든 동영상 릴리스 날짜는 오전 12시를 표시합니다.](working-with-sql/_static/m55.png)

*Models/Movie.cs* 파일을 열고 아래 표시된 강조 표시된 줄을 추가합니다.

[!code-csharp[](start-mvc/sample/MvcMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

다음 자습서에서는 [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6)를 다룹니다. [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) 특성은 필드의 이름에 표시할 대상을 지정합니다(이 경우 "ReleaseDate" 대신 "Release Date"). [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) 특성은 필드에 저장된 시간 정보가 표시되지 않도록 데이터의 형식(날짜)을 지정합니다.

Entity Framework Core가 `Price`를 데이터베이스의 통화에 올바르게 매핑할 수 있도록 `[Column(TypeName = "decimal(18, 2)")]` 데이터 주석이 필요합니다. 자세한 내용은 [데이터 형식](/ef/core/modeling/relational/data-types)을 참조하세요.

`Movies` 컨트롤러로 이동하고 **편집** 링크 위에 마우스 포인터를 놓으면 대상 URL이 표시됩니다.

![브라우저 창에서 편집 링크에 마우스를 가져가면 https://localhost:5001/Movies/Edit/5의 링크 Url이 표시됩니다.](~/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

**편집**, **세부 정보** 및 **삭제** 링크는 *Views/Movies/Index.cshtml* 파일의 Core MVC 앵커 태그 도우미에 의해 생성됩니다.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[태그 도우미](xref:mvc/views/tag-helpers/intro)를 사용하면 서버 쪽 코드를 Razor 파일에서 HTML 요소를 만들고 렌더링하는 데 사용할 수 있습니다. 위의 코드에서는 `AnchorTagHelper`가 컨트롤러 작업 메서드와 경로 ID로부터 HTML `href` 특성 값을 동적으로 생성합니다. 선호하는 브라우저에서 **소스 보기**를 사용하거나 개발자 도구를 통해 생성된 표시를 검사합니다. 생성된 HTML의 일부는 다음과 같습니다.

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

*Startup.cs* 파일의 [라우팅](xref:mvc/controllers/routing) 설정에 대한 형식을 다시 호출합니다.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

ASP.NET Core는 `https://localhost:5001/Movies/Edit/4`를 매개 변수 `Id`가 4인 `Movies` 컨트롤러의 `Edit` 작업 메서드에 대한 요청으로 해석합니다. 컨트롤러 메서드는 작업 메서드라고도 합니다.

[태그 도우미](xref:mvc/views/tag-helpers/intro)는 ASP.NET Core의 가장 인기 있는 새로운 기능 중 하나입니다. 자세한 내용은 [추가 리소스](#additional-resources)를 참조하세요.

`Movies` 컨트롤러를 열고 두 `Edit` 작업 메서드를 검사합니다. 다음 코드는 동영상을 페치하여 *Edit.cshtml* Razor 파일에서 생성된 편집 양식에 기입하는 `HTTP GET Edit` 메서드를 보여 줍니다. 

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

다음 코드는게시된 동영상 값을 처리하는 `HTTP POST Edit` 메서드를 보여 줍니다.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

다음 코드는게시된 동영상 값을 처리하는 `HTTP POST Edit` 메서드를 보여 줍니다.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[Bind]` 특성은 하나의 [과도 게시](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) 방지 방법입니다. 변경하려는 속성만 `[Bind]` 특성에 포함해야 합니다. 자세한 내용은 [과도 게시로부터 컨트롤러 보호](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)를 참조하세요. [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)는 과도 게시를 방지하기 위한 다른 방법을 제공합니다.

두 번째 `Edit` 작업 메서드는 `[HttpPost]` 특성 뒤에 옵니다.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2&highlight=1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

::: moniker-end

`HttpPost` 특성은 이 `Edit` 메서드가 `POST` 요청에 *대해서만* 호출될 수 있음을 지정합니다. `[HttpGet]` 특성을 첫 번째 편집 메서드에 적용할 수 있으나 `[HttpGet]`이 기본값이므로 그럴 필요가 없습니다.

`ValidateAntiForgeryToken` 특성은 [요청 위조 방지](xref:security/anti-request-forgery)를 위해 사용되며 편집 보기 파일 (*Views/Movies/Edit.cshtml*)에서 생성된 위조 방지 파일과 쌍을 이룹니다. 편집 보기 파일은 [Form 태그 도우미](xref:mvc/views/working-with-forms)를 통해 위조 방지 토큰을 생성합니다.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[Form 태그 도우미](xref:mvc/views/working-with-forms)는 동영상 컨트롤러의`Edit` 메서드에서 생성된 `[ValidateAntiForgeryToken]` 위조 방지 토큰과 일치하는 숨겨진 위조 방지 토큰을 생성합니다. 자세한 내용은 [위조 방지 요청](xref:security/anti-request-forgery)을 참조하세요.

`HttpGet Edit` 메서드는 동영상 `ID` 매개 변수를 취하며, Entity Framework `FindAsync` 메서드를 사용하여 동영상을 검색하고, 선택된 동영상을 편집 보기에 반환합니다. 동영상을 찾을 수 없는 경우 `NotFound`(HTTP 404)를 반환합니다.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

편집 보기에서 스캐폴딩 시스템이 만들어질 때 `Movie` 클래스를 조사하고 클래스의 각 속성에 대해 `<label>` 및 `<input>` 요소를 렌더링하기 위한 코드를 만들었습니다. 다음 예제에서는 Visual Studio 스캐 폴딩 시스템에서 생성된 편집 보기를 보여 줍니다.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/EditOriginal.cshtml)]

파일 맨 위에서 보기 템플릿이 `@model MvcMovie.Models.Movie` 문을 어떻게 나타내는지 확인합니다. `@model MvcMovie.Models.Movie`는 보기에서 보기 템플릿에 대한 모델에서 `Movie` 유형을 기대한다고 지정합니다.

스캐폴드 코드는 몇 가지 태그 도우미 메서드를 사용하여 HTML 마크업을 간소화합니다. [레이블 태그 도우미](xref:mvc/views/working-with-forms)는 필드 이름을 표시합니다("제목", "발표일", "장르", "가격" 등). [입력 태그 도우미](xref:mvc/views/working-with-forms)는 HTML `<input>` 요소를 렌더링합니다. [유효성 검사 태그 도우미](xref:mvc/views/working-with-forms)는 해당 속성과 연결된 모든 유효성 검사 메시지를 표시합니다.

애플리케이션을 실행하고 `/Movies` URL로 이동합니다. **편집** 링크를 클릭합니다. 브라우저에서 페이지에 대한 소스를 봅니다. `<form>` 요소에 대해 생성된 HTML은 다음과 같습니다.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

`<input>` 요소는 `action` 특성이 `/Movies/Edit/id` URL에 게시되도록 설정된 `HTML <form>` 요소에 있습니다. 양식 데이터는 `Save` 단추를 클릭하면 서버에 게시됩니다. `</form>` 요소를 닫기 전 마지막 줄은 [양식 태그 도우미](xref:mvc/views/working-with-forms)에서 생성된 숨겨진 [XSRF](xref:security/anti-request-forgery) 토큰을 나타냅니다. 

## <a name="processing-the-post-request"></a>POST 요청 처리

다음 목록은 `Edit` 작업 메서드의 `[HttpPost]` 버전을 보여 줍니다.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[ValidateAntiForgeryToken]` 특성은 [Form 태그 도우미](xref:mvc/views/working-with-forms)의 위조 방지 토큰 생성기에서 생성된 숨겨진 [XSRF](xref:security/anti-request-forgery) 토큰의 유효성을 검사합니다.

[모델 바인딩](xref:mvc/models/model-binding) 시스템에서는 게시된 양식 값을 취하며 `movie` 매개 변수`Movie` 변수로 전달되는 개체를 만듭니다. `ModelState.IsValid` 메서드는 양식에서 제출된 데이터가 `Movie` 개체를 수정하는 데(편집 또는 업데이트) 사용될 수 있음을 확인합니다. 데이터가 유효하면 저장됩니다. 업데이트된(편집된) 동영상 데이터는 데이터베이스 컨텍스트의 `SaveChangesAsync` 메서드를 호출하여 데이터베이스에 저장됩니다. 데이터를 저장한 후 코드는 사용자를 `MoviesController` 클래스의 `Index` 동작 메서드로 다시 전달하며, 여기에는 방금 수행한 변경 사항을 포함하여 동영상 컬렉션이 표시됩니다.

양식을 서버에 게시하기 전에 클라이언트 쪽 유효성 검사에서는 필드의 모든 유효성 검사 규칙을 확인합니다. 유효성 검사 오류가 있으면 오류 메시지를 표시하고 양식을 게시하지 않습니다. JavaScript를 사용하지 않는 경우 클라이언트 쪽 유효성 검사는 없으나 서버에서 유효하지 않은 게시 값을 탐지하며 양식 값이 오류 메시지와 함께 다시 표시됩니다. 이 자습서의 뒷부분에서 [모델 유효성 검사](xref:mvc/models/validation)를 더 자세히 다룹니다. *Views/Movies/Edit.cshtml* 보기 템플릿의 [Validation 태그 도우미](xref:mvc/views/working-with-forms)는 적합 오류 메시지 표시를 담당합니다.

![보기 편집: abc의 올바르지 않은 가격 값 예외에서는 필드 가격이 숫자여야 한다고 표시합니다. xyz의 잘못된 출시일 값 예외에서는 올바른 날짜를 입력하라고 표시합니다.](~/tutorials/first-mvc-app/controller-methods-views/_static/val.png)

동영상 컨트롤러의 모든 `HttpGet` 메서드는 유사한 패턴을 따릅니다. 동영상 개체(`Index`의 경우 개체 목록)을 가져오고 해당 개체(모델)를 보기에 전달합니다. `Create` 메서드는 빈 동영상 개체를 `Create` 보기에 전달합니다. 생성, 편집, 삭제 또는 어떤 식으로든 데이터를 수정하는 모든 메서드는 메서드의 `[HttpPost]` 오버로드에서 해당 작업을 수행합니다. `HTTP GET` 메서드에서 데이터를 수정하는 것은 보안상 위험합니다. `HTTP GET` 메서드에서의 데이터 수정은 GET 요청이 애플리케이션 상태를 변경해서는 안 되는 HTTP 모범 사례와 구조적 [REST](http://rest.elkstein.org/) 패턴에도 위배됩니다. 다시 말해 GET 작업 수행은 부작용 없이 안전하고 영속 데이터를 수정하지 않는 방법으로 이루어져야 합니다.

## <a name="additional-resources"></a>추가 자료

* [전역화 및 지역화](xref:fundamentals/localization)
* [태그 도우미 소개](xref:mvc/views/tag-helpers/intro)
* [태그 도우미 작성](xref:mvc/views/tag-helpers/authoring)
* [요청 위조 방지](xref:security/anti-request-forgery)
* [과도 게시로부터 컨트롤러 보호](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [Form 태그 도우미](xref:mvc/views/working-with-forms)
* [Input 태그 도우미](xref:mvc/views/working-with-forms)
* [Label 태그 도우미](xref:mvc/views/working-with-forms)
* [Select 태그 도우미](xref:mvc/views/working-with-forms)
* [Validation 검사 태그 도우미](xref:mvc/views/working-with-forms)

> [!div class="step-by-step"]
> [이전](working-with-sql.md)
> [다음](search.md)  
