## Angular 5의 특징

> by happygrammer

Angualr 5는 프로그레시브 웹 앱(progressive web app)을 지원합니다. Angular 5로 개발시 프로그레시브 웹 앱과  서버 사이드 렌더링을 함께 고려하면 모바일 디바이스에서 보다 새로운 사용자 경험이 생길것입니다.



### 변경된점

- @angular/http가 deprecate 되고 @angular/common/http를 사용합니다.
- @angular/compiler
  - 타입스크립트 2.4.x 버전 이상(템플릿 타입 검사 및 기타 기능 제공)을 필요로 합니다.
- Reflective-Injector(폴리필에 의존) 대신 Static-Injector로 변경 됐습니다.
- router에서 RouterOutlet 속성인 `locationInjector` 와 `locationFactoryResolver`가 제거 됐습니다.
- Angular 4.0에 존재했던 ngGetConentSelectors가 deprecated 됐고 대신 ComponentFactory.ngContentSelectors를 사용하게 됐습니다.



### 추가된점

- Form에서 updateOn의 blur와 sumit 옵션이 추가 됨 

  https://next.angular.io/api/forms/FormControl


- 라우터 이벤트(GuardsCheckStart, GuardsCheckEnd, ResolveStart, ResolveEnd)가 추가 됐습니다.

- @angular/service-worker가 추가됐습니다.

  - Progressive 웹 앱을 가능케 하는 패키지

- 안정화된 NgTemplateOutlet API가 추가됨

- animations에서 query() 사용시 limit 추가

  https://next.angular.io/api/animations/query

- animations에서 @.disabled 속성이 추가돼 자식 애니메이션을 disable 할 수 있게 함

  ​

### 개선된 점

- 번들 사이즈 감소
- addEventListener를 사용해 렌더링 속도를 개선
- 새로운 build-optimizer로 업데이트 됨
- 빌드 속도 향상(AOT 컴파일러가 디폴트이며 Aot 빌드, Prod 빌드에 대한 속도가 빨라졌습니다.)
  - 1) aot 옵션 추가 예 : ng build -aot -w  
  - 2) prod 옵션 추가 예 : ng build --prod --watch
- 컴포넌트에 대한 트리쉐이킹 지원
- 파이프(Plural,decimal, percent/currency)에 locale 옵션이 추가 됐습니다.
- 테스팅 개선, 하이브리드 애플리케이션에 대한 성능 향상
- 캐싱, 로깅, XSRF, Testing, Lazy loading 등에 대한 개선
- 에러 메시지가 보다 개선 됨

- invalid CSS 속성을 감지할 수 있게 됐습니다.

- 기타 버그 수정