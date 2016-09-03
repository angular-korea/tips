# Angular 2 스타일 가이드 #

> 마지막 수저일 : 2016-09-03 (v0.1)

본 문서는 [Angular 2 Korea User Group](https://www.facebook.com/groups/angular2korea)이 제공하는 스타일 가이드에 대한 요약본입니다. 본 문서의 내용의 기준은 [Angular 스타일 가이드 문서](https://angular.io/styleguide)와 [마이크로스프트 타입스크립트 컨벤션 문서](https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines)입니다. 본 스타일 가이드는 Angular 버전 업데이트를 고려하여 업데이트할 예정입니다.


## 파일분할 ##

구성요소 단위별로 파일분할을 원칙으로 합니다. 구성 요소 컴포넌트, 모듈, 서비스, 모델, 목의 구성요소에 대해 컨벤션을 살펴보고 파일 이름을 정의하고자 합니다.

### 컴포넌트 ###

[추천](https://github.com/angular/angular-ja/blob/master/public/docs/_examples/style-guide/ts/02-07/app/heroes/hero.component.ts)

	import { Component } from '@angular/core';
	

	@Component({
	  template: '<div>hero component</div>',	  
	  selector: 'toh-hero'
	})
	export class HeroComponent {}
	
비추천

	import { Component } from '@angular/core';
	
	// HeroComponent is in the Tour of Heroes feature
	@Component({
	  selector: 'hero'
	})
	export class HeroComponent {}	




[추천](https://github.com/angular/angular-ja/blob/master/public/docs/_examples/style-guide/ts/05-17/app/heroes/hero-list/hero-list.component.ts)

탬플릿에 로직을 두지 말고, 프리젠테이션에 로직을 둡니다.


	import { Component } from '@angular/core';
	
	import { Hero } from '../shared/hero.model.ts';
	
	// #docregion example
	@Component({
	  selector: 'toh-hero-list',
	  template: `
	    <section>
	      Our list of heroes:
	      <toh-hero *ngFor="let hero of heroes" [hero]="hero">
	      </toh-hero>
	      Total powers: {{totalPowers}}<br>
	      Average power: {{avgPower}}
	    </section>
	  `
	})
	export class HeroListComponent {
	  heroes: Hero[];
	  totalPowers: number;
	
	  // #enddocregion example
	  // testing harness
	  constructor() {
	    this.heroes = [];
	  }
	
	  // #docregion example
	  get avgPower() {
	    return this.totalPowers / this.heroes.length;
	  }
	}


[비추천](https://github.com/angular/angular-ja/blob/master/public/docs/_examples/style-guide/ts/05-17/app/heroes/hero-list/hero-list.component.avoid.ts)

탬플릿에 로직을 두지 않습니다.

	import { Component } from '@angular/core';
	
	import { Hero } from '../shared/hero.model';
	// #docregion example
	/* avoid */
	
	@Component({
	  selector: 'toh-hero-list',
	  template: `
	    <section>
	      Our list of heroes:
	      <hero-profile *ngFor="let hero of heroes" [hero]="hero">
	      </hero-profile>
	      Total powers: {{totalPowers}}<br>
	      Average power: {{totalPowers / heroes.length}}
	    </section>
	  `
	})
	export class HeroListComponent {
	  heroes: Hero[];
	  totalPowers: number;
	}



### 모듈 ###

	import { NgModule } from '@angular/core';
	import { BrowserModule } from '@angular/platform-browser';
	import { RouterModule } from '@angular/router';
	import { AppComponent } from './app.component';
	import { HeroesComponent } from './heroes/heroes.component';
	@NgModule({
	  imports: [
	    BrowserModule,
	  ],
	  declarations: [
	    AppComponent,
	    HeroesComponent
	  ],
	  exports: [ AppComponent ],
	  bootstrap: [ AppComponent ]
	})
	export class AppModule { }

### 서비스 ###

추천

컴포넌트에 재사용 가능하도록 서비스에 로직을 둡니다.  서비스에 로직을 두면 로직이 독립되므로 테스트가 용이해 집니다. 그리고 서비스로 로직을 컴포넌트 외부로 독립함으로서 로직 의존성을 제거할 수 있습니다.

	import { Component, OnInit } from '@angular/core';
	import { Hero, HeroService } from '../shared';
	@Component({
	  selector: 'toh-hero-list',
	  template: `...`
	})
	export class HeroListComponent implements OnInit {
	  heroes: Hero[];
	  constructor(private heroService: HeroService) {}
	  getHeroes() {
	    this.heroes = [];
	    this.heroService.getHeroes()
	      .subscribe(heroes => this.heroes = heroes);
	  }
	  ngOnInit() {
	    this.getHeroes();
	  }
	}

비추천

컴포넌트안에 로직을 두지 않습니다.

	import { OnInit } from '@angular/core';
	import { Http, Response } from '@angular/http';
	import { Observable } from 'rxjs/Observable';
	import { Hero } from '../shared/hero.model';
	const heroesUrl = 'http://angular.io';
	export class HeroListComponent implements OnInit {
	  heroes: Hero[];
	  constructor(private http: Http) {}
	  getHeroes() {
	    this.heroes = [];
	    this.http.get(heroesUrl)
	      .map((response: Response) => <Hero[]>response.json().data)
	      .catch(this.catchBadResponse)
	      .finally(() => this.hideSpinner())
	      .subscribe((heroes: Hero[]) => this.heroes = heroes);
	  }
	  ngOnInit() {
	    this.getHeroes();
	  }
	  private catchBadResponse(err: any, source: Observable<any>) {
	    // log and handle the exception
	    return new Observable();
	  }
	  private hideSpinner() {
	    // hide the spinner
	  }
	}



추천

	import { Injectable } from '@angular/core';
	
	import { Hero } from './hero.model';
	
	@Injectable()
	export class HeroCollectorService {
	  hero: Hero;
	
	  constructor() { }
	  // #enddocregion example
	  // testing harness
	  getHero() {
	    this.hero = {
	      name: 'RubberMan',
	      power: 'He is so elastic'
	    };
	
	    return this.hero;
	  }
	  // #docregion example
	}
	


비추천


	import { Injectable } from '@angular/core';
	
	import { IHero } from './hero.model.avoid';
	
	@Injectable()
	export class HeroCollectorService {
	  hero: IHero;
	
	  constructor() { }
	}
	// #enddocregion example


### 모델 ###



[추천](https://github.com/angular/angular-ja/blob/master/public/docs/_examples/style-guide/ts/03-03/app/shared/hero.model.ts)

	// #docregion
	// #docregion example
	export class Hero {
	  name: string;
	  power: string;
	}

[비추천](https://github.com/angular/angular-ja/blob/master/public/docs/_examples/style-guide/ts/03-03/app/shared/hero.model.avoid.ts)

	export interface IHello {
	  name: string;
	  power: string;
	}
	
	export class Hello implements IHello {
	  name: string;
	  power: string;
	}

### 목 ###

	import { Hero } from './hero.model';
	export const HEROES: Hero[] = [
	  {id: 1, name: 'Bombasto'},
	  {id: 2, name: 'Tornado'},
	  {id: 3, name: 'Magneta'},
	];



### 일반 파일이름 ###

서비스, 지시자, 컴포넌트, 파이프, 모델에 대한 파일이름은 다음과 같이 정합니다.


	hello.service.ts
	hello.directive.ts
	hello.component.ts
	hello.pipe.ts
	hello.model.ts

### 테스트 파일이름 ###

테스트 파일이름에는.ts 확장자 전에 spec을 붙여 줍니다.

	hello.service.spec.ts
	hello.directive.spec.ts
	hello.component.spec.ts
	hello.pipe.spec.ts

### 종단 대 종단 테스트 파일이름 ###

종단 대 종답(End to End) 테스트 파일 이름은 .e2e-spec을 붙여 줍니다.

	hello.e2e-spec.ts



## 네이밍 ##

### 클래스이름 ###

클래스 이름은 첫글자는 대문자로 시작하며 타입이름도 대문자로 시작됩니다.

	@Injectable()
	export class HelloService

	@Directive({ })
	export class HelloDirective {}

	@Component({ })
	export class HelloComponent {}

	@Pipe({ })
	export class HelloPipe implements PipeTransform {}


### const ###

	export const mockHeroes   = ['Sam', 'Jill']; // 추천
	export const heroesUrl    = 'api/heroes';    // 추천
	export const VILLAINS_URL = 'api/villains';  // 비추천 

### interface 호출 ###

	import { IHero } from './hero.model.avoid'; // 비추천
	import { Hero } from './hero.model';

### 함수이름 ###

	private _log() // 비추천
	private log()

### import 구문 ###

디스트럭처링시에 스페이스를 입력합니다.

	import {Http, Response} from '@angular/http'; // 비추천
	import { Http, Response } from '@angular/http'; 


Overall Structural Guidelines

컴포넌트에는 총 4가지 파일이 존재합니다.

	*.html, *.css, *.ts, and *.spec.ts


### Shared Folder ###
> 준비중


### Folders-by-Feature Structure ###
> 준비중


### Layout Components ###
> 준비중


### import 시 모듈은 알파벳순으로 정렬 ###

알파벳 ABC 순으로

	import { ExceptionService, SpinnerService, ToastService } from '../../shared';

### third party 호출과 구분 ###

비추천

	import { ExceptionService, SpinnerService, ToastService } from '../../shared';
	import { Http, Response } from '@angular/http';
	import { Injectable } from '@angular/core';
	import { Hero } from './hero.model';

추천 : third party를 호출할 경우 한줄을 띈다.

	import { Injectable } from '@angular/core';
	import { Http, Response } from '@angular/http';

	import { Hero } from './hero.model';
	import { ExceptionService, SpinnerService, ToastService } from '../../shared';


### Create and Import Barrels ###

비추천

	import { Component, OnInit } from '@angular/core';
	import { CONFIG } from '../shared/config';
	import { EntityService } from '../shared/entity.service';
	import { ExceptionService } from '../shared/exception.service';
	import { FilterTextComponent } from '../shared/filter-text/filter-text.component';
	import { InitCapsPipe } from '../shared/init-caps.pipe';
	import { SpinnerService } from '../shared/spinner/spinner.service';
	import { ToastService } from '../shared/toast/toast.service';
	@Component({
	  selector: 'toh-heroes',
	  templateUrl: 'app/+heroes/heroes.component.html'
	})
	export class HeroesComponent implements OnInit {
	  constructor() { }
	  ngOnInit() { }
	}

추천
	
	import { Component, OnInit } from '@angular/core';
	import {
	  CONFIG,
	  EntityService,
	  ExceptionService,
	  SpinnerService,
	  ToastService
	} from '../shared';
	@Component({
	  selector: 'toh-heroes',
	  templateUrl: 'heroes.component.html'
	})
	export class HeroesComponent implements OnInit {
	  constructor() { }
	  ngOnInit() { }
	}


### 컴포넌트 셀렉터 이름 ###

비추천

	@Component({
	  selector: 'tohHeroButton',
	  templateUrl: 'hero-button.component.html'
	})
	export class HeroButtonComponent {}

추천 : 대시 - 를 넣어서 이름이 명확히 드러나도록 합니다.
	
	@Component({
	  selector: 'toh-hero-button',
	  templateUrl: 'hero-button.component.html'
	})
	export class HeroButtonComponent {}


### 외부파일로 둘것 ###

입력 코드가 3라인 이상인 경우 탬플릿과 스타일을 추출하여 외부 파일로 독립합니다.

비추천

	@Component({
	  selector: 'toh-heroes',
	  template: `
	    <div>
	      <h2>My Heroes</h2>
	      <ul class="heroes">
	        <li *ngFor="let hero of heroes">
	          <span class="badge">{{hero.id}}</span> {{hero.name}}
	        </li>
	      </ul>
	      <div *ngIf="selectedHero">
	        <h2>{{selectedHero.name | uppercase}} is my hero</h2>
	      </div>
	    </div>
	  `,
	  styleUrls:  [`
	    .heroes {
	      margin: 0 0 2em 0; list-style-type: none; padding: 0; width: 15em;
	    }
	    .heroes li {
	      cursor: pointer;
	      position: relative;
	      left: 0;
	      background-color: #EEE;
	      margin: .5em;
	      padding: .3em 0;
	      height: 1.6em;
	      border-radius: 4px;
	    }
	    .heroes .badge {
	      display: inline-block;
	      font-size: small;
	      color: white;
	      padding: 0.8em 0.7em 0 0.7em;
	      background-color: #607D8B;
	      line-height: 1em;
	      position: relative;
	      left: -1px;
	      top: -4px;
	      height: 1.8em;
	      margin-right: .8em;
	      border-radius: 4px 0 0 4px;
	    }
	  `]
	})
	export class HeroesComponent implements OnInit {
	  heroes: Hero[];
	  selectedHero: Hero;
	  ngOnInit() {}
	}

추천
	
	@Component({
	  selector: 'toh-heroes',
	  templateUrl: 'heroes.component.html',
	  styleUrls:  ['heroes.component.css']
	})
	export class HeroesComponent implements OnInit {
	  heroes: Hero[];
	  selectedHero: Hero;
	  ngOnInit() { }
	}
	

### input, output은 인라인으로 둡니다 ###

비추천

	/* 비추천 */
	@Component({
	  selector: 'toh-hero-button',
	  template: `<button></button>`,
	  inputs: [
	    'label'
	  ],
	  outputs: [
	    'change'
	  ]
	})
	export class HeroButtonComponent {
	  change = new EventEmitter<any>();
	  label: string;
	}

추천

	@Component({
	  selector: 'toh-hero-button',
	  template: `<button>{{label}}</button>`
	})
	export class HeroButtonComponent {
	  @Output() change = new EventEmitter<any>();
	  @Input() label: string;
	}


### Input Output 장식자에 대해 이름 변경을 하지 않습니다 ###

비추천 : 이름을 renaming 함으로서 export시 혼동을 줄 수 있습니다.

	/* 비추천 */
	@Component({
	  selector: 'toh-hero-button',
	  template: `<button>{{label}}</button>`
	})
	export class HeroButtonComponent {
	  @Output('changeEvent') change = new EventEmitter<any>();
	  @Input('labelAttribute') label: string;
	}


추천

	@Component({
	  selector: 'toh-hero-button',
	  template: `<button>{{label}}</button>`
	})
	export class HeroButtonComponent {
	  @Output() change = new EventEmitter<any>();
	  @Input() label: string;
	}

### 멤버 시퀀스 ###


비추천 

	/* 비추천 */
	export class ToastComponent implements OnInit {
	  private defaults = {
	    title: '',
	    message: 'May the Force be with You'
	  };
	  message: string;
	  title: string;
	  private toastElement: any;
	  ngOnInit() {
	    this.toastElement = document.getElementById('toh-toast');
	  }
	  // private methods
	  private hide() {
	    this.toastElement.style.opacity = 0;
	    window.setTimeout(() => this.toastElement.style.zIndex = 0, 400);
	  }
	  activate(message = this.defaults.message, title = this.defaults.title) {
	    this.title = title;
	    this.message = message;
	    this.show();
	  }
	  private show() {
	    console.log(this.message);
	    this.toastElement.style.opacity = 1;
	    this.toastElement.style.zIndex = 9999;
	    window.setTimeout(() => this.hide(), 2500);
	  }
	}

추천 : 공개 프로퍼티를 상단에 듭니다. 공개 메소드를 상단에 둔다. 그리고 초기화 함수 그리고 비공개 메소드를 하단에 둡니다.

	export class ToastComponent implements OnInit {
	  // public properties
	  message: string;
	  title: string;
	  // private fields
	  private defaults = {
	    title: '',
	    message: 'May the Force be with You'
	  };
	  private toastElement: any;
	  // public methods
	  activate(message = this.defaults.message, title = this.defaults.title) {
	    this.title = title;
	    this.message = message;
	    this.show();
	  }
	  ngOnInit() {
	    this.toastElement = document.getElementById('toh-toast');
	  }
	  // private methods
	  private hide() {
	    this.toastElement.style.opacity = 0;
	    window.setTimeout(() => this.toastElement.style.zIndex = 0, 400);
	  }
	  private show() {
	    console.log(this.message);
	    this.toastElement.style.opacity = 1;
	    this.toastElement.style.zIndex = 9999;
	    window.setTimeout(() => this.hide(), 2500);
	  }
	}



### 서비스에 로직을 둔다 ###

컴포넌트 클래스에는 뷰와 관련된 로직에 제한해서 둔다. 그리고 로직은 서비스에 맡긴다.

재사용 가능한 로직은 서비스로 이관합니다. 컴포넌트는 가급적 심플하게 유지하고, 목적에 맞게 금 합니다.

비추천

	/* 비추천 */
	import { OnInit } from '@angular/core';
	import { Http, Response } from '@angular/http';
	import { Observable } from 'rxjs/Observable';
	import { Hero } from '../shared/hero.model';
	const heroesUrl = 'http://angular.io';
	export class HeroListComponent implements OnInit {
	  heroes: Hero[];
	  constructor(private http: Http) {}
	  getHeroes() {
	    this.heroes = [];
	    this.http.get(heroesUrl)
	      .map((response: Response) => <Hero[]>response.json().data)
	      .catch(this.catchBadResponse)
	      .finally(() => this.hideSpinner())
	      .subscribe((heroes: Hero[]) => this.heroes = heroes);
	  }
	  ngOnInit() {
	    this.getHeroes();
	  }
	  private catchBadResponse(err: any, source: Observable<any>) {
	    // log and handle the exception
	    return new Observable();
	  }
	  private hideSpinner() {
	    // hide the spinner
	  }
	}


추천

	import { Component, OnInit } from '@angular/core';
	import { Hero, HeroService } from '../shared';
	@Component({
	  selector: 'toh-hero-list',
	  template: `...`
	})
	export class HeroListComponent implements OnInit {
	  heroes: Hero[];
	  constructor(private heroService: HeroService) {}
	  getHeroes() {
	    this.heroes = [];
	    this.heroService.getHeroes()
	      .subscribe(heroes => this.heroes = heroes);
	  }
	  ngOnInit() {
	    this.getHeroes();
	  }
	}


### Output 프로퍼티에는 접두어를 생략합니다 ###

비추천

on은 생략합니다.

	@Component({
	  selector: 'toh-hero',
	  template: `...`
	})
	export class HeroComponent {
	  @Output() onSavedTheDay = new EventEmitter<boolean>();
	}


추천


	export class HeroComponent {
	  @Output() savedTheDay = new EventEmitter<boolean>();
	}



### 컴포넌트 클래스에 프리젠테이션 로직을 둔다 ###

비추천

	/* 비추천 */
	@Component({
	  selector: 'toh-hero-list',
	  template: `
	    <section>
	      Our list of heroes:
	      <hero-profile *ngFor="let hero of heroes" [hero]="hero">
	      </hero-profile>
	      Total powers: {{totalPowers}}<br>
	      Average power: {{totalPowers / heroes.length}}
	    </section>
	  `
	})
	export class HeroListComponent {
	  heroes: Hero[];
	  totalPowers: number;
	}

추천

	@Component({
	  selector: 'toh-hero-list',
	  template: `
	    <section>
	      Our list of heroes:
	      <toh-hero *ngFor="let hero of heroes" [hero]="hero">
	      </toh-hero>
	      Total powers: {{totalPowers}}<br>
	      Average power: {{avgPower}}
	    </section>
	  `
	})
	export class HeroListComponent {
	  heroes: Hero[];
	  totalPowers: number;
	  get avgPower() {
	    return this.totalPowers / this.heroes.length;
	  }
	}


### 지시자 사용 ###

엘리먼트에 지시자를 적극적으로 사용하여 기능을 향상시킨다.

지시자를 사용하여 탬플릿에서 프리젠테이션 로직을 없앤다.

	@Directive({
	  selector: '[tohHighlight]'
	})
	export class HighlightDirective {
	  @HostListener('mouseover') onMouseEnter() {
	    // do highlight work
	  }
	}

	<div tohHighlight>Bombasta</div>



### HostListener 와 HostBinding ###

비추천

	@Directive({
	  selector: '[tohValidator]',
	  host: {
	    '(mouseenter)': 'onMouseEnter()',
	    'attr.role': 'button'
	  }
	})
	export class ValidatorDirective {
	  role = 'button';
	  onMouseEnter() {
	    // do work
	  }
	}

추천 : HostBinding, HostListener 추가가 쉬우며, 가독성이 좋다.

	@Directive({
	  selector: '[tohValidator]'
	})
	export class ValidatorDirective {
	  @HostBinding('attr.role') role = 'button';
	  @HostListener('mouseenter') onMouseEnter() {
	    // do work
	  }
	}
	

### 서비스제공 ###

최상위 컴포넌트에서 서비스를 제공할때 인스턴스를 공유되며 자식 컴포넌트에서 이용할 수 있답.

추천 : providers를 이용하면 자식 컴포넌트에서도 서비스 인스턴스를 공유하도록 하는 것을 추천

	import { Component } from '@angular/core';
	import { HeroService } from './heroes';
	@Component({
	  selector: 'toh-app',
	  template: `
	      <toh-heroes></toh-heroes>
	    `,
	  providers: [HeroService]
	})
	export class AppComponent {}


비추천 : 컴포넌트에서만 공유하는 것은 비추천


	import { Component, OnInit } from '@angular/core';
	import { Hero, HeroService } from '../shared';
	@Component({
	  selector: 'toh-heroes',
	  template: `
	      <pre>{{heroes | json}}</pre>
	    `
	})
	export class HeroListComponent implements OnInit {
	  heroes: Hero[] = [];
	  constructor(private heroService: HeroService) { }
	  ngOnInit() {
	    this.heroService.getHeroes().subscribe(heroes => this.heroes = heroes);
	  }
	}


### @Injectable() 사용 ###

비추천

	export class HeroArena {
	  constructor(
	      @Inject(HeroService) private heroService: HeroService,
	      @Inject(Http) private http: Http) {}
	}



추천 

	@Injectable()
	export class HeroArena {
	  constructor(
	    private heroService: HeroService,
	    private http: Http) {}
	}


### 라이프사이클 훅 인터페이스 ###

비추천 : onInit은 잘못된 스펠링입니다.

	@Component({
	  selector: 'toh-hero-button',
	  template: `<button>OK<button>`
	})
	export class HeroButtonComponent {
	  onInit() { // misspelled
	    console.log('The component is initialized');
	  }
	}
	

추천 : ngOnInit으로 합니다.

	@Component({
	  selector: 'toh-hero-button',
	  template: `<button>OK</button>`
	})
	export class HeroButtonComponent implements OnInit {
	  ngOnInit() {
	    console.log('The component is initialized');
	  }
	}