## Angular 5의 특징 요약

> by [happygrammer](https://twitter.com/happygrammer)

Angualr 5는 [프로그레시브 웹 앱](https://developers.google.com/web/fundamentals/codelabs/your-first-pwapp/?hl=ko)(progressive web app)을 지원합니다. Angular 5로 개발 시 프로그레시브 웹 앱과  [서버 사이드 렌더링](https://next.angular.io/guide/universal)을 함께 고려하면 모바일 디바이스에서 보다 새로운 사용자 경험이 생길것입니다.



### 변경된 점

- @angular/http가 deprecated 됐고 [@angular/common/http](https://next.angular.io/api/common/http/HttpClient)를 사용합니다.
- [@angular/core의 컴파일러](@angular/core의 컴파일러)
  - [타입스크립트 2.4.x 버전](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-4.html) 이상(템플릿 타입 검사 및 기타 기능 제공)을 필요로 합니다.
- Reflective-Injector(폴리필에 의존) 대신 [StaticInjector](https://github.com/angular/angular/commit/d9d00bd)를 사용하게 됐습니다.
- router에서 [RouterOutlet](https://next.angular.io/api/router/RouterOutlet) 속성인 `locationInjector` 와 `locationFactoryResolver`가 제거 됐습니다.
- Angular 4.0에 존재했던 ngGetConentSelectors가 deprecated 됐고 대신 [ComponentFactory](https://next.angular.io/api/core/ComponentFactory)가 [ngContentSelectors](https://github.com/angular/angular/pull/15214)를 사용하게 됐습니다.



### 추가된 점

- Form에서 [updateOn](https://next.angular.io/api/forms/FormControl) 설정시 blur와 sumit 옵션을 설정할 수 있게 됨

  - https://next.angular.io/api/forms/FormControl


- 라우터 이벤트([GuardsCheckStart](https://next.angular.io/api/router/GuardsCheckStart), [GuardsCheckEnd](https://next.angular.io/api/router/GuardsCheckEnd), [ResolveStart](https://next.angular.io/api/router/ResolveStart), [ResolveEnd](https://next.angular.io/api/router/ResolveEnd))가 추가 됐습니다.

- [@angular/service-worker](@angular/service-worker)가 추가됐습니다.

  - Progressive 웹 앱을 가능케 하는 패키지

- 안정화된 [NgTemplateOutlet](https://next.angular.io/api/common/NgTemplateOutlet) API가 추가됨

- animations에서 query() 사용시 limit 추가

  - https://next.angular.io/api/animations/query

- animations에서 [@.disabled](https://next.angular.io/api/animations/trigger) 속성이 추가돼 자식 애니메이션을 disable 할 수 있게 함

- 파이프([Plural](https://next.angular.io/api/common/I18nPluralPipe),[decimal](https://next.angular.io/api/common/DecimalPipe), [percent](https://next.angular.io/api/common/PercentPipe)/[currency](https://next.angular.io/api/common/CurrencyPipe))에 locale 옵션이 추가 됐습니다.

  ​

### 개선된 점

- [번들 사이즈 감소](https://next.angular.io/guide/webpack)
- core가 [addEventListener](https://github.com/angular/angular/commit/6279e50)를 사용하도록 해 렌더링 속도를 개선함
- 새로운 build-optimizer로 업데이트 됨
- 빌드 속도 향상([AOT 컴파일러](https://next.angular.io/guide/aot-compiler)가 디폴트입니다.
  - Aot 빌드, Prod 빌드에 대한 속도가 빨라졌습니다.
    - 1) watch 시 aot 옵션 추가 예 : ng build -aot -w  
    - 2) watch 시 prod 옵션 추가 예 : ng build --prod --watch
- 테스팅 개선, 하이브리드 애플리케이션에 대한 성능 향상, 캐싱, 로깅, [XSRF](https://next.angular.io/api/http/XSRFStrategy), 등에 대한 개선
- invalid CSS 속성을 감지할 수 있게 됐습니다.
- 에러 메시지가 보다 개선 됨
- 기타 버그 수정(Lazy loading시 모듈이 없는 경우 throw 등)