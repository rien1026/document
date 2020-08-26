# jenkins 설치 및 master-slave 구성

## jenkins 설치
EC2 master 서버에 ssh로 접속하여 아래의 순서대로 Jenkins를 설치한다. 
```
$ sudo apt-get update
$ sudo apt-get install java-1.8.0-openjdk-devel
$ sudo apt-get install wget
$ sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
$ sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
$ sudo apt-get install jenkins -y
```

## jenkins 서비스 시작
```
$ sudo systemctl start jenkins
```

## jenkins 접속 후 추천 plugin 설치
### 1. 웹브라우저에서 Jenkins 페이지 접근
Jenkins는 기본적으로 8080 포트를 사용하므로 웹 브라우저에서 아래의 주소로 접근할 수 있다.
```
http://[jenkins server ip]:8080
```

### 2. initial password 입력 후 기본 플로그인 설치
initial password가 있는 파일을 확인하여 아래의 administrator password에 입력한다.
```
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574917890141_image.png)

### 3. 추천 플러그인을 설치한다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574918045415_image.png)

### 4. jenkins 관리 페이지 접근을 위한 계정을 생성한다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574918219231_image.png)

### 5. jenkins 서버 주소를 입력한다.
jenkins가 설치되어있는 서버의 URL주소가 입력되어 있으므로 그냥 Save and Finish 할 수 있다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574918317096_image.png)


## master-slave 구성
jenkins 관리 페이지 접속 후, 아래의 순서대로 설정을 진행한다.
### 1. agent tcp port 설정
Configure Global Security 에서 접근 가능한 agent tcp port random 으로 설정
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574919090641_image.png)
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574919311048_image.png)



### 2. slave 노드 생성
jenkins 관리 → 노드 관리
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574919513999_image.png)
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574919569059_image.png)
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574919632362_image.png)

launch method를 Launch agent by connecting it to the master로 설정 <br>
Custom WorkDir path는 jenkins workspace 경로이며 추 후 github 연동 후 소스 코드를 다운 받는 경로
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574919764458_image.png)
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574919801832_image.png)

### 3. 준비한 EC2 slave 서버에서 java 명령어를 통해 master에 연결
아래의 agent.jar를 다운받아 slave 서버에 복사 한다. <br>
파일 복사의 경우 scp 명령어로 ftp 서버 연결 없이 간단하게 할 수 있다.<br>
예) scp -i [private key 이름].pem ./agent.jar ubuntu@[master 서버 주소]:~/agent.jar <br>
agent.jar가 있는 경로에서 아래의 java 명령어를 실행 <br>
```
$ java -jar agent.jar …. 
```
master 서버의 방화벽에 slave 서버 ip 허용 작업을 꼭 해야 한다. <br>
앞선 agent tcp port 설정에서 특정 포트로 지정하였다면 해당 포트만 허용하면 되지만, <br>
이번 경우에는 random으로 설정을 해 주었기 때문에 모든 포트를 허용해야 한다. 
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574919897512_image.png)
정상적으로 연결이 되면, 연결된 ip address와 port를 알 수 있다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574920073804_image.png)

### 4. slave 정상 연결 확인
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_615B4E053CCE60E9EFCACB14527C666DD98999949346BFCEEB55884AC5DA0600_1574920109494_image.png)
