> [!WARNING]
> 보안상의 이유로 페이지 모델 속성에 `GET` 요청 데이터를 바인딩하기 위해 옵트인해야 합니다. 속성에 매핑하기 전에 사용자 입력을 확인합니다. 쿼리 문자열이나 경로 값을 사용하는 시나리오를 해결할 때 `GET` 바인딩을 옵트인하면 유용합니다.
>
> `GET` 요청에 속성을 바인딩하려면 [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) 특성의 `SupportsGet` 속성을 `true`로 설정합니다. `[BindProperty(SupportsGet = true)]`
