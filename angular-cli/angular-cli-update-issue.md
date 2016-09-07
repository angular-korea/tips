## AngularCLI 업데이트 이슈 ##

업데이트 과정에서 발생하는 이슈에 대해 해결방법들을 공유드립니다.


### beta.11-webpack.2에서 beta.11-webpack.3 업데이트후 발생하는 문제 ###
> 2016-09-7

AngularCLI가 웹팩 베타으로 업데이트가 되었습니다. 
금일 기준 angular-cli@1.0.0-beta.11-webpack.8 버전이 가장 최신 버전입니다. 

 문제는 angular-cli@1.0.0-beta.11-webpack.2 버전에서 angular-cli@1.0.0-beta.11-webpack.3 이상 버전으로 업데이트 하고 ng 명령어를 실행하면 **Error while running script** 에러가 발생합니다.

관련 문제는 다음 문서에서 찾을 수 있습니다.

- [https://github.com/angular/angular-cli/issues/1883](https://github.com/angular/angular-cli/issues/1883)

해결방법은 [Node.JS](https://nodejs.org/ko/)를 6.5 이상으로 업데이트 하시면 해결됩니다. 아쉽게도 공식 문서에는 이러한 내용을 안내해 놓지 않고 있습니다.