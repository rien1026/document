

# jenkins, git, docker를 이용한 자동 배포 구현

## docker 설치
```
sudo apt-get install -y docker.io
```

## Github SSH 연결
### 1. Jenkins User로 SSH 생성
Jenkins User는 서버에 등록된 사용자가 아니기 때문에, <br>
sudo su - u jenkins등으로 전환할 수 없다.
```
$ sudo -u jenkins /bin/bash
bash-4.2$ mkdir /var/lib/jenkins/.ssh
bash-4.2$ cd /var/lib/jenkins/.ssh
bash-4.2$ ssh-keygen -t rsa -f github
```


### 2. Jenkins Credential 설정
```
/var/lib/jenkins/.ssh/github
```
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1582249075628_image.png)


### 3. Github Key 추가
```
/var/lib/jenkins/.ssh/github.pub 
```
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1582249232882_image.png)


### 4. github webhook 설정
```
payload URL = http://[jenkins 서버 주소]:8080/github-webhook/
```
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1574922695236_image.png)
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1574924431964_image.png)


## Jenkins 자동 배포 태스크 추가
### 1. jenkins user 에 docker 권한 추가
```
$ sudo usermod -aG docker $USER
```


### 2. jenkins 태스크 생성
Github Project에 깃허브 주소 추가. <br>
Restrict where this project can be run 에 해당 배포 대상이 되는 노드 추가.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1574926392209_image.png)
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1574927053248_image.png)


Repositories에 깃허브 주소 추가 <br>
빌드 유발의 Github hook trigger for GitScm polling 추가 <br>
Credentials 설정 후, 제대로 연결이 되었다면 에러메세지가 없어진다. <br>
Github Deploy Keys에서 key가 초록불이 된 걸 알 수 있다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1582249443911_image.png)
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1582249494886_image.png)


git 코드 동기화 이 후 실행될 docker image build 및 run 명령어 추가.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1582518891870_image.png)
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1598488314612_image.png)


### 3. 배포 테스트
Build Now를 눌러서 태스크 실행. <br>
정상적으로 동작했을 시 Build History에 파란 구슬이 생성된다. <Br>
에러가 발생했을 시 붉은 구슬이 생성된다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1574931636349_image.png)


에러 발 생 시, 붉은 구슬을 클릭하면 아래와 같은 에러 메세지를 확인 할 수 있다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1574931686756_image.png)


### 4. Dockerfile 보기.
```
FROM: 이미지 생성의 기반이 되는 이미지
MAINTAINER: 이미지 생성자 정보
RUN: FROM에서 설정한 이미지 위에서 스크립트 혹은 명령을 실행
여기서 RUN으로 실행한 결과가 새 이미지로 생성되고, 실행 내역은 이미지의 히스토리에 기록 됨
WORKDIR: RUN, CMD에서 설정한 실행 파일이 실행될 디렉터리
ADD: 파일을 이미지에 추가
EXPOSE: 호스트와 연결할 포트 번호 설정
호스트와 연결할 뿐 외부에 노출되지는 않음
CMD: 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행
```
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1574931726007_image.png)
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_8B7330F65FED23BA6CEE06973502BB13EEF7384C94AB525F437B6843960D4CF9_1598488649307_image.png)


### 5. node image를 사용할 때 linux 용량을 줄이는 방법
full 버전은 기본적으로 개발에 필요한 패키지들을 포함하고 있다. <br>
필요한 패키지만 추가 설치 하는 방향으로 용량이 더 적은 버전을 쓰기로 한다.
```
FROM node:lts # 900MB
FROM node:lts-slim # 140MB
FROM node:lts-alpine # 88MB
```
