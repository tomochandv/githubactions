# NODEJS Github Actions 사용 설명서

Github actions를 이용 하여 빌드하고 배포하는 법을 담았습니다.
실제로 검색을 해도 아직 나온지 얼마 되지않은 서비스이기에 문서가 많이 있지않습니다.(특히 한글은 찾아보기가 힘드네요)
Github의 도큐먼트를 봐도 실제로 상용에 적용 하기에는 많은 시간과 스택보어플로어를 봐야 하는 단점이 있기에
혹시나 구글링을 하시는 분들을 위해 올립니다.


## 소개

아시다 싶이 개발자는 손이가는걸 귀찮아 합니다. 또한 나의 손은 실수를 합니다.
그러기에 자동화된 배포를 지향합니다. 이전까지는 jenkins를 설치하여 사용하거나 travis를 이용 하셨을 겁니다.
**Jenkins**
엄청나게 좋은 툴임은 틀림없습니다. 하지만, 설치부터 세팅까지가 너무너무 손이 많이 갑니다.
또한 서버가 존재해야 합니다. 이게 참 걸리죠.
**Travis**
이거또한 엄청나게 좋은 서비스입니다. 많은 개발자들이 사용하니.... 그러나 Docker를 이용해야 합니다.
Docker 가 굳이 필요 없는 서비스인데 Docker 구성을 해야하니 이또한 귀찮습니다.

근대 베타버전이던 배포 서비스를 Github에서 정식 릴리즈 하여 서비스합니다.

저는 기본적으로 .NET을 약 15년간 개발오다 약 5년 전부터 nodejs 를 다루게 되어, 요즘은 nodejs를 이용합니다.
요번에 주니어 개발자와 합작을 하며 reactjs 까지 만지게 되네요. 
그러면서 actions를 이용하여 배포를 해보자 하게 되어 만지면서 성공한걸 문서화하여 조금이나마 도움이 되게 할려 합니다.


## 구성

### stack 

 - Backend - Nodejs 
 - Front - Reacjs

### 서비스

 - Backend - EC2 
 - Front - S3

### 구성도

 - Backend - koa, babel
	 - babel build -> EC2 copy -> pm2 start
 - front - reactjs
	 - reactjs 기본 build -> S3복사

## 사용법

여러분이 작성하시는 코드는 1도 고치실 필요가 없습니다.
레포지토리의 루트에 **.github/workflows** 폴더를 생성하시거나 
Actions 라는 메뉴로 가셔서 생성을 하셔도 됩니다.
이폴더에 yml 파일을 만들면됩니다.

이 프로젝트에는 backend 기준으로 작성을 하였고, **deploy** 라는 폴더에 각각의 yml을 넣었습니다.

> ${{ secrets.REMOTE_HOST }}: 변수입니다.  
> 메뉴에 settings -> secrets 안에 key와 value를 넣으시면 됩니다.
> 이렇게 해서 commit을 하게 되면 보안에 좋겠죠.

**주의사항**
package.json.lock 파일이 있어야 합니다. 보통 ignore에 빼게 되는데 이놈을 보고 디펜더시 cache를 써서 속도를 높여줍니다.
없으면 에러가 납니다.

## 요약

 1. 레포지토리 생성
 2. EC2, S3 구성 : S3는 업로드 권한 IAM 생성, EC2는 접속할 ssh 계정및 비번 생성
 3. Settings -> secrets 안에 서버 정보 저장
 4. .github/workflows 폴더생성
 5. 4번 폴더에 yml 파일 작성
 6. master에 push
 7. 잘 돌아가는지 확인.
 8. 궁금하신 사항은 aka.tomochan@gmail.com 으로 메일 주세요.