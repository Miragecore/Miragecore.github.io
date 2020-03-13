---
title : Docker 관련 명령어 정리
tags : docker NAS
---
### Docker 이미지 관련(PowerShell)
파워쉘에서 진행
#### docker 이미지 빌드 
```
docker build -t <iamge Name> <Dockerfile PATH>
ex) docker build -t node_devel .
```
이미지 빌드는 다음 섹션의 dockerfile을 기준으로 진행됨.

#### docker 이미지 파일로 저장
```
docker save <imagename> -o <targetname>.tar
ex) docker save node_devel -o node_devel.tar
```

#### 전체 컨테이너 삭제
```
docker rm $(docker ps -a -q)
```
#### 이미지 전체 삭제
```
docker rmi $(docker images -q)
```
[참고 사이트](https://pandora.tistory.com/202)

### DockerFile Example
Docker 빌드중 해야할 작업 정의
```
# Docker 이미지의 기본은 nodejs 이미지에서 시작 
FROM        node:12.16
# 이 이미지의 관리자는 나?
MAINTAINER  miragekimys@gmail.com

# vi editor 설치(필수는 아니지만 필요할지도)
RUN         apt-get -y update
RUN         apt-get -y install vim
 
# 소스 복사(현재 디렉토리의 파일들을 /usr/src/app으로 복사)
COPY . /usr/src/app  
 
# Docker 실행후 추후 명령들이 실행될 기본 디렉토리 설정
WORKDIR     /usr/src/app

# 의존성 패키지 설치
# package.json에 정의된 패키지를 설치하는 과정 
# /usr/src/app에 복사된 파일중에 npm init로 생성되고
# npm install로 정의된 package.json이 있어야함
RUN         npm install
 
# 외부 노출 포트 지정
# -p 옵션이나 NAS 사용시 해당 포트와 동일하게 설정되어야 함. 
EXPOSE      33080

# 기본 실행 명령
CMD         node server.js
```
뭔가 더 있을지도 모르지만 일단 여기까지가 내가 필요에 충분했음.

### Sysnology NAS
[Docker 지원 모델 확인](https://www.synology.com/ko-kr/dsm/packages/Docker)
* Docker Iamge 빌드 & 파일로 저장
* NAS Docker에서 이미지 추가(파일에서 추가)
* NAS Image 탭에서 컨테이너 실행
실행 버튼 클릭시 설정 마법사가 실행됨
    * 컨테이너 이름 설정
설정 마법사 상의 첫화면에서 컨테이너 이름 설정
        * 고급 설정
설정 마법사의 고급설정에서 아래의 볼륨, 포트, 환경변수등을 설정해야 함.
            * 볼륨 설정 
폴더 추가시 docker run -v 옵션의 역할
파일도 별도 추가 가능
            * 포트 설정
docker run -p 옵션 역할
            * 환경변수 등록
docker run -e 옵션에 설정하는 환경 변수
혹은 node에서 사용할 변수등을 등록(ex:NODE_ENV)
    * 설정 마법사 완료시 컨테이너 실행 
* 실행된 컨테이너는 NAS의 Docker의 컨테이너 탭에서 확인 

실행시 위의 변수등을 GUI에서 설정하고 나서 별도로 수정하는 방법을 찾지 못함.

실행된 컨테이너의 터미널탭에서 추가 버튼을 이용하여 내부 bash 사용가능

### Docker 실행 관련(PowerShell)
#### Docker 실행
-d : detached mode(background 실행)
-p <호스트 포트> : <컨테이너 포트>
-v <호스트 폴더 경로> : <컨테이너 마운트 경로>
-rm : 컨테이너 정지시 자동 삭제
```
ex) docker run -d -p 8080:4567 <image Name>
ex) docker run -p 81:80 -v c:/Projects/dockerTest/src/:/var/www/html/ mariadb
```
#### 실행중인 Docker 컨테이너내의 프로그램 실행
-it : 터미널 
```
docker exec -it [Container Name] <실행할 명령>
docker exec -it app /bin/bash
docker exec -it app /bin/sh
docker exec -it app /bin/ash
```
[참고 사이트](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter20/28)

### 참고
[Docker CP 명령어](https://www.leafcats.com/163)

[Docker Command 정리](https://jungwoon.github.io/docker/2019/01/11/Docker-1/)

[Docekr Command 정리](http://pyrasis.com/Docker/Docker-HOWTO#stop)

[Docker -v 에서 Port 매칭 관련](https://stackoverflow.com/questions/48629001/trying-to-run-a-simple-express-server-on-a-docker-but-cant-access-any-routes)

[Docker Volume 마운트](https://stackoverflow.com/questions/47162825/docker-volumes-on-windows-10)

[Docker Build](https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html#sinatra-%EC%9B%B9-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EC%83%98%ED%94%8C)