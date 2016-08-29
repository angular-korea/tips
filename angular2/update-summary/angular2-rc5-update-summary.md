# Angular 2 RC5 업데이트 (2016-08-10) #

RC5 로 업데이트 되었습니다. RC5의 가장 큰 특징은 NgModule 장식자를 추가가 되었다는것 입니다.

## 1) @NgModule 장식자 지원 ##

NgModules의 등장이유는 컴포넌트 마다 빈번히 가져다 쓰는 파이프나 컴포넌트, 지시자 선언의 불편함을 해소 하기 위해서입니다. NgModules의 장점은  사전 컴파일(Ahead of Time compilation) 방식을 이용해 전체적인 어플리케이션 속도가 올라갑니다.

NgModule 장식자는 다음과 같이 이용합니다. 아래와 갈이 app.module.ts 파일 있다고 합시다.

	@NgModule({
	  imports: [ BrowserModule, FormsModule ],
	  declarations: [ MyComponent, 사용자의 컴포넌트 .... ],
	  providers: [ 서비스들...]
	  bootstrap: [ AppComponent  ]
	})
	class MyAppModule {}

위 파일은 main.ts에서 다음과 같이 가져옵니다.

	import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
	import { AppModule } from './app.module';
	platformBrowserDynamic().bootstrapModule(AppModule);


## 2) NgModule을 통해 쉬워진 모듈구성 ##

FormsModule, RouterModule, Material Design modules를 보다 쉽게 이용할 수 있게 되었습니다.

예를들어 라우터를 이용해 상업디자인모듈(material design modules)을 사용 할때 예전 방식은 다음과 같았습니다.

	import {Component} from ‘@angular/core’
	import {MD_BUTTON_DIRECTIVES} from ‘@angular-material2/button’
	import {MD_SIDENAV_DIRECTIVES} from ‘@angular-material2/sidenav’
	import {MD_CARD_DIRECTIVES} from ‘@angular-material2/card’
	import {provideRouter, ROUTER_DIRECTIVES} from ‘@angular/router’
	@Component({
	  selector: ‘my-component’,
	  providers: [ provideRouter(routeConfig) ],
	  directives: [
	    MD_BUTTON_DIRECTIVES,
	    MD_SIDENAV_DIRECTIVES,
	    MD_CARD_DIRECTIVES,
	    ROUTER_DIRECTIVES
	  ]
	})
	class MyComponent {}

디자인을 지시자로 만들어 놓고 사용하면 지시자 특성상 반복적인 호출이 많습니다.  
그럴때 routeConfig에 전달할 목적으로 @NgModule을 미리 묶어서 정의할 수 있습니다.

	import {Component} from ‘@angular/core’
	import {MdButtonModule} from ‘@angular-material2/button’
	import {MdSideNavModule} from ‘@angular-material2/sidenav’
	import {MdCardModule} from ‘@angular-material2/card’
	import {RouterModule} from ‘@angular/router’
	@NgModule({
	  imports: [
	    MdButtonModule,
	    MdSideNavModule,
	    MdCardModule,
	    RouterModule.forRoot(routeConfig)
	  ]
	})

class MyAppModule {}

이렇게 되면 모든 컴포넌트가 모듈에 접근할 수 있습니다.
그래서 더이상 컴포넌트마다 지시자를 반복적으로 선언하지 않아도 됩니다.

	@Component({
	  selector: ‘my-component’,
	  templateUrl: ‘my-component.html’
	})
	class MyComponent {}



### 3) NgModule을 이용한 느린로딩 지원 ###

NgModule 장식자는 라우터를 통해 느린로딩을 지원합니다. 예로 보면 다음과 같습니다.

위 MyAppModule은 부트스트랩에 다시 이렇게 선언됩니다.

	import {platformBrowser} from ‘@angular/platform-browser’
	import {MyAppModuleNgFactory} from ‘./app.ngfactory’ //generated code
	platformBrowser().bootstrapModuleFactory(MyAppModule);
	
	import {RouterModule} from ‘@angular/router’
	import {NgModule} from ‘@angular/core’
	@NgModule({
	  declarations: [ MyComponent, MyHomeRoute ],
	  bootstrap: [ MyComponent ],
	  imports: [
	    RouterModule.forRoot([
	      { path: ‘home’, component: MyHomeRoute },
	      { path: ‘lazy’, loadChildren: ‘./my-lazy-module’ }
	    ])
	})
	class MyAppModule {}

NgModule 장식자에서 loadChildren 프로퍼티가 추가 된 것이 확인 됩니다.
이 프로퍼티는 하위 모듈들을 가져오기 위한 프로퍼티입니다. 다음과 같이 사용합니다.

	import {RouterModule} from ‘@angular/router’
	import {NgModule} from ‘@angular/core’
	
	@NgModule({
	  declarations: [ MyLazyHome, MyLazyChild ],
	  imports: [
	    RouterModule.forChild([
	      { path: ‘’, component: MyLazyHome },
	      { path: ‘a’, component: MyLazyChild }
	    ])
	  ]
	})
	class MyLazyModule {}


RC6 버전 이후부터는 NgModule을 이용하는 이유로  컴포넌트 메타데이터에 선언하였던  
directives, pipes 프로퍼티는 폐기됩니다. 일시적으로 RC5에서는 지원합니다.
다음 버전 RC를 대비해서 directives와 파이프는 미리미리 declarations으로 옮겨 놓으라는 지침이 있습니다.

## 관련링크 ##

본 게시물과 관련한 보다 자세한 내용은 다음 문서들을 참고할 수 있습니다.

RC4에서 RC5로 마이그레이션
> [https://angular.io/docs/ts/latest/cookbook/rc4-to-rc5.html](https://angular.io/docs/ts/latest/cookbook/rc4-to-rc5.html)

RC5 라이브데모
> [http://plnkr.co/edit/?p=preview](http://plnkr.co/edit/?p=preview)

업데이트 관련문서
> [https://angular.io/docs/ts/latest/cookbook/rc4-to-rc5.html](https://angular.io/docs/ts/latest/cookbook/rc4-to-rc5.html) 
> [https://github.com/angular/angular/blob/master/CHANGELOG.md](https://github.com/angular/angular/blob/master/CHANGELOG.md)
> [http://angularjs.blogspot.kr](http://angularjs.blogspot.kr)
