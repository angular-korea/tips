# angular cli beta.11-webpack.2로 업데이트하는 방법 #

> 2016년 08월 22일 기준

[**angular-cli**](https://github.com/angular/angular-cli)는 angular 2 프로젝트 생성 및 개발에 기반이 될 수 있는 **커맨드 라인 인터페이스**입니다.

최신 angular-cli버전은 angular-cli@1.0.0-beta.11-webpack.2 입니다.

beta.10 에서는 RC4가 반영되어 동작하였으며 beta.11 부터는 RC5가 반영되었습니다.

### webpack.2 버전으로 업데이트시 발생하는 문제 ###

angular 2가 RC5로 업데이트되면서 angular-CLI가 web pack 기반으로 변경되었습니다. beta. 11-web pack 업데이트되면서 angular-CLI 설치와 운영을 위해 버전 업데이트가 필요합니다. 

- Node 버전은 4 이상이 필요
- npm은 3 버전 이상이 필요

여기서 유의할 점은 npm 버전입니다. npm버전은 1.0.0.beta.11-webpack.x 이후부터는 2 버전이 아닌 3 버전 이상으로 설치되어야 합니다.  만약 npm 2.x 버전으로 1.0.0.beta.11-webpack.x 설치를 진행한다면 설치가 원만히 진행되지 않습니다. 만약 설치가 원만히 진행되지 않는다면 npm을 업데이트해 주어야 합니다. 

### npm 업데이트 ###

npm을 업데이트 해 주기 전에 먼저 자신의 npm 버전을 확인합니다.

	npm --version

만약 npm이 2점대 버전이라면 업데이트를 진행해야 합니다. 업데이트는 다음 명령어를 입력함으로써 진행합니다. 

	npm install -g npm@latest

업데이트가 완료되면 다시 npm 버전을 확인해 볼 수 있습니다. 

정상적으로 버전 업데이트가 되었다면 작성일 기준(16-08-22) npm 버전은 다음과 같습니다.

	3.10.6

### angular-cli 업데이트 ###

업데이트 완료되었다면 Angular-cli를 업데이트합니다. 

업데이트를 위해서는 기존에 설치된 angular-cli를 언인스톨 해주고 기존에 받았던 패키지 캐시도 삭제해 줍니다. 

	npm uninstall -g angular-cli
	npm cache clean

여기서 [npm cache clean 명령어](https://docs.npmjs.com/cli/cache)는 캐시 폴더를 삭제해 주는 명령어입니다. 캐시 폴더는 % AppData%/Roaming/npm-cache 디렉터리에 위치해 있습니다. 캐시 폴더의 목적은 다음에 패키지를 설치해주게 될 때 네트워크가 필요 없이 npm 패키지를 설치해주게 해주는 용도입니다. 만약 새로운 패키지 설치 후 이전 버전의 패키지가 더 이상 필요 없기 때문에 npm cache clean 명령어를 이용해 오래된 버전의 캐시를 정리해 주는 것입니다. 

### angular-cli@webpack 설치 ###

정상적으로 angular-cli가 삭제되었다면 angular-cli@webpack을 설치해줍니다.

	npm install -g angular-cli@webpack

### angular-cli 버전확인 ###

설치 후 angular-cli 버전을 확인해 봅니다. angular-cli 버전 확인 명령어는 다음과 같습니다. 

	ng --version

위 명령어 실행 후 작성일 기준(16-08-22) 정상적으로 1.0.0-beta.11-webpack.2로 업데이트되었음을 확인할 수 있습니다. 

	angular-cli: 1.0.0-beta.11-webpack.2
	node: 4.4.3
	os: win32 x64

### angular-cli 프로젝트 만들고 실행 ###

위 과정까지 완료되었다면 프로젝트를 새로 만듭니다.

	ng new PROJECT_NAME
	cd PROJECT_NAME
	ng serve

### 기타 ###

그외 명령어는 공식 [도움말문서](https://github.com/angular/angular-cli)를 참고 합니다. 

작지만 도움이 되었으면 좋겠습니다. 감사합니다.