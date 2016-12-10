# RC6 에서 정식버전으로 업데이트 #
> RC에서 정식버전으로 업데이트할때 필요한 사항


##submit -> ngSubmit ##

옳은 예

	<form [formGroup]="form"  (submit)="submitForm($event)">


## ngModelOptions ##

- [ngModelOptions]="{standalone: true}" 은 사용하지 않습니다.



## ReactiveFormsModule ##

- REACTIVE_FORM_DIRECTIVES 대신 ReactiveFormsModule 이용을 이용합니다.


	import { ReactiveFormsModule }          from '@angular/forms';