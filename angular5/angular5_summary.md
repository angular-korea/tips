## Angular 5의 특징 요약

> by [happygrammer](https://twitter.com/happygrammer)

Angualr 5는 [서비스 워커(service worker)](https://github.com/angular/angular/pull/19274)를 이용해 [프로그레시브 웹 앱](https://developers.google.com/web/fundamentals/codelabs/your-first-pwapp/?hl=ko)(progressive web app)을 지원합니다.  Angular 5로 개발 시 프로그레시브 웹 앱과  [서버 사이드 렌더링](https://next.angular.io/guide/universal)을 함께 고려하면 모바일 디바이스에서 보다 새로운 사용자 경험이 생길것입니다. 다음은 Angular 5에 대한 특징 요약입니다.



### Angular 5 환경

- Angular 5는 `Angular CLI 1.5` 버전 이상에서 지원됨
- 타입스크립트 2.4 이상으로 업데이트 해야함
  - [@angular/core의 컴파일러](https://next.angular.io/api/core/Compiler)는 [타입스크립트 2.4.x 버전](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-4.html) 이상을 필요로 하게 됨(beta.7)




### 추가된 특징

- animations

  - animations에서 [@.disabled](https://next.angular.io/api/animations/trigger) 속성이 추가돼 자식 애니메이션을 disable 할 수 있게 함(beta.5)
  - invalid CSS 속성을 감지해 에러를 표시할 수 있게 됨(beta.5)
  - :increment , :decrement에 대한 전환 별칭(aliases)을 지원하게 됨([6f45519](https://github.com/angular/angular/commit/6f45519))(beta.0)
  - animations에서 [query()](https://next.angular.io/api/animations/query) 사용시 limit 추가 됨(rc.0)

- 개선된 장식자 지원

  - 장식자(decorators)내에서 useFactory, useValue의 값을 표현할 때 명명된 함수(named function) 대신에 람다를 사용할 수 있게 됐습니다.

    ```
    Component({
      provider: [{provide: SOME_TOKEN, useFactory: () => null}]
    })
    export class MyClass {}
    ```

    ```
    Component({
      provider: [{provide: SOME_TOKEN, useValue: SomeEnum.OK}]
    })
    export class MyClass {}

    ```

- [@angular/service-worker](https://next.angular.io/api/service-worker)가 추가 됨(rc.0)

  - Progressive 웹 앱을 가능케 하는 패키지
  - 관련 자료
    - [Angular Service Workers](http://pascalprecht.github.io/slides/angular-and-service-workers/#/)

- **컴파일러 향상**
  - TS 2.4 이상을 사용함으로써 템플릿 타입 검사(template type checking)가 가능하게 됨
  - 여러 exportAs 이름을 사용할 수 있게됨([#18723](https://github.com/angular/angular/issues/18723)) ([7ec28fe](https://github.com/angular/angular/commit/7ec28fe))(beta.6)

    ```
    @Component({
      moduleId: module.id,
      selector: 'a[mat-button], a[mat-raised-button], a[mat-icon-button], a[mat-fab], a[mat-mini-fab]',
      exportAs: 'matButton, matAnchor',
      .
      .
      .
    }
    ```

- Form에서 [updateOn](https://next.angular.io/api/forms/FormControl) 설정시 blur와 submit 옵션을 설정할 수 있게 됨(beta.3)
  - updateOn 옵션에 blur와 submit  값을 설정해 값 변경이 일어날 때마다 검증을 수행할 수 있게 됐습니다.

    ```
    const c = new FormControl('', { updateOn: 'blur' });
    ```

  - 템플릿 주도 폼

    - 이전

      ```
      <input name="firstName" ngModel>

      ```

    - 이후

      ```
      <input name="firstName" ngModel [ngModelOptions]="{updateOn: 'blur'}">
      또는
      <form [ngFormOptions]="{updateOn: 'submit'}">
      ```

  - 반응형 폼

    - 이전

      ```
      new FormGroup(value);
      new FormControl(value, [], [myValidator])
      ```

    - 이후

      ```
      new FormGroup(value, {updateOn: 'blur'}));
      new FormControl(value, {updateOn: 'blur', asyncValidators: [myValidator]})
      ```

  - https://next.angular.io/api/forms/FormControl


- 새로운 라우터 생명주기 이벤트가 추가됨
  - [GuardsCheckStart](https://next.angular.io/api/router/GuardsCheckStart), [GuardsCheckEnd](https://next.angular.io/api/router/GuardsCheckEnd), [ResolveStart](https://next.angular.io/api/router/ResolveStart), [ResolveEnd](https://next.angular.io/api/router/ResolveEnd))가 추가 됨(beta.7)
  - `ChildActivationStart`, `ActivationStart`,  `ActivationEnd`, `ChildActivationEnd`가 추가됨
- platform-server 패키지에서 [TransferState](https://next.angular.io/api/platform-browser/TransferState) API가 추가됨(rc.0)



### 변경된 점

- I18n 파이프(beta.5)

  - 더이상 초기화를 위한 폴리필을 사용하지 않아도 됨
  - angular는 en-US 언어에 대한 locale 데이터를 디폴트로 포함함
  - LOCALE_ID값을 다른 locale로 변경하면 해당 언어에 대한 locale 데이터를 가져와야함

- 파이프([Plural](https://next.angular.io/api/common/I18nPluralPipe),[decimal](https://next.angular.io/api/common/DecimalPipe), [percent](https://next.angular.io/api/common/PercentPipe)/[currency](https://next.angular.io/api/common/CurrencyPipe))에 locale 옵션이 추가 됨(beta.5)

  - [Date 파이프](https://next.angular.io/api/common/DatePipe)

    - 포맷 변경있고 다음과 같은 포맷이 추가됨
      - `long`, `full`, `longTime`, `fullTime` 포맷이 추가됨
      - ```yyy ```포맷이 지원되고 기타 여러 포맷이 추가됨

  - [currency 파이프](https://next.angular.io/api/common/CurrencyPipe)

    - default 심볼이 USD4.99 대신 en-US인 $4.99로 변경됨

      ```
      <!--output '$0.259'-->
      <p>A: {{a | currency}}</p>
      ```

  - [percent 파이프](https://next.angular.io/api/common/PercentPipe)

    - {{ 3.141592 | percent }}일때 314.1592%가 아닌 ```en-US``` locale에 따라 314%로 출력됨

    - {{ 0.259 | percent}}일 때의 출력은 26%가 됨

      ```
      @Component({
        selector: 'percent-pipe',
        template: `
          <!--'26%'가 출력됨-->
          {{0.259 | percent}}`
      })
      export class PercentPipeComponent { }
      ```



### 성능 개선점

- 애니메이션

  - AST classes를 제거해 번들 사이즈의 용량을 줄임 ([#19539](https://github.com/angular/angular/issues/19539)) ([d5c9c5f](https://github.com/angular/angular/commit/d5c9c5f))(rc.2)

- 새로운 빌드 옵티마이저로 업데이트 됨

  - 불필요한 코드를 제거해 애플리케이션의 사이즈를 줄임

  - Angular CLI는 옵티마이저를 기본 옵션으로 사용하므로 번들 사이즈가 감소됐습니다.

- Angular Universal State Transfer API와 DOM 지원

  - ServerTransferStateModule가 추가 됨에 따라 HTTP 재요청 없이 서버의 상태를 클라이언트로 전달할 수 있게 됐습니다.

- 커스텀 존(zone)을 이용한 애플리케이션의 부트스트랩 지원([#17672](https://github.com/angular/angular/issues/17672)) ([344a5ca](https://github.com/angular/angular/commit/344a5ca))

  - 성능을 갖춘 애플리케이션을 개발 하기 위해 zones을 사용하지 않을 수 있습니다.

    - ngZone 속성에 noop을 사용해 애플리케이션을 bootstrap하면 됩니다.

      ```
      platformBrowserDynamic().bootstrapModule(AppModule, {ngZone: 'noop'}).then( ref => {} );
      ```

- core가 [addEventListener](https://github.com/angular/angular/commit/6279e50)를 사용하도록 해 렌더링 속도를 개선함

- 빌드 속도가 향상됨
  - [AOT 컴파일러](https://next.angular.io/guide/aot-compiler)가 디폴트
  - Aot 빌드, Prod 빌드에 대한 속도가 빨라짐
  - watch 모드시 속도가 빨라짐 ([#19275](https://github.com/angular/angular/issues/19275)) ([6665d76](https://github.com/angular/angular/commit/6665d76))
    - 명령어(aot+watch) 예 : ng build -aot -w  
    - 명령어(prod+watch) 예 : ng build --prod --watch




### Deprecated 코드

- ReflectiveInjector(폴리필에 의존)가 deprecated됐고 [StaticInjector](https://github.com/angular/angular/commit/d9d00bd)인 Injector.create를 사용하게 됨(beta.3)
  - 변경전 : ReflectiveInjector.resolveAndCreate(providers);
  - 변경후 : Injector.create(providers);
- OpaqueToken이 deprecated 됨(beta.6)
- @angular/http 라이브러리가 deprecated됨
  - [HttpClient](https://next.angular.io/api/common/http/HttpClient)는 @angular/common 패키지를 통해 제공됨
- router : [RouterOutlet](https://next.angular.io/api/router/RouterOutlet) 속성인 `locationInjector` 와 `locationFactoryResolver`가 deprecated  됨(beta.7)
- v4에서 사용되던 NgFor가 deprecated되고 [NgForOf](https://next.angular.io/api/common/NgForOf)를 사용해야 함(beta.5)
- NgTemplateOutlet#ngOutletContext가 deprecated됨 Testability#findProviders를 사용해야 함(beta.5)
- DebugNode#source가 defrecated됨(beta.5)
- TrackByFn가 deprecated됨, [TrackByFunction](https://next.angular.io/api/core/TrackByFunction)를 사용해야함(beta.5)
- [UpgradeAdapter](https://next.angular.io/api/upgrade/UpgradeAdapter)는 v5에서 deprecated됐고 대신 upgrade/static을 사용함
- platform-webworker가 deprecated됨, [SerializerTypes.PRIMITIVE](https://next.angular.io/api/platform-webworker/SerializerTypes)를 사용해야함(beta.5)




### 기타

- [Material 디자인 컴포넌트](https://material.angular.io/)가 서버 사이드 렌더링에서 호환됨
- Angular 5는 RxJS를 5.5.2 이상 버전으로 업데이트됨
- 에러 메시지의 개선, 테스팅 개선, hybrid 애플리케이션에 대한 성능 향상, 캐싱, 로깅, [XSRF](https://next.angular.io/api/http/XSRFStrategy), 등에 대한 개선
- 성능 개선(performance improvements) 통계
  - 5.0.0-rc에서는 총 10건의 성능 개선
    - rc.2(4건), rc.1(3건), rc.0(3건)
  - 5.0.0-beta에서는 총 5건의 성능 개선
    - beta.4(2건), beta.3(1건), beta.1(2건)
- 버그 수정 통계
  - 5.0.0-rc에서는 총 72건의 버그 수정
    - ... rc.6(4건), rc.5(1건), rc.4(3건), rc.3(14건), rc.2(3건), rc.1( 13건), rc.0(34건)
  - 5.0.0.beta에서는 총 69건의 버그 수정
    - ... beta.6(21건), beta.5(11건), beta.5(11건), beta.4(3건), beta.3(12건) beta.2(2건), beta.1(9건)




### 참고 링크

- [Version 5.0.0 of Angular Now Available](https://blog.angular.io/version-5-0-0-of-angular-now-available-37e414935ced) 
- Angular의 [CHANGELOG](https://github.com/angular/angular/blob/master/CHANGELOG.md)


- Angular 5의 문서(https://next.angular.io/docs)