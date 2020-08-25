
# AWS EC2 생성

## 1. 인스턴스 시작
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_0C3CFBECFD65F946C540A0776389D5237E67C249764DC95BAD7C19793094B044_1574914749223_image.png)

## 2. OS 선택
Ubuntu 18.04 버전으로 선택한다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_0C3CFBECFD65F946C540A0776389D5237E67C249764DC95BAD7C19793094B044_1574914816323_image.png)

## 3. 인스턴스 유형 선택
사용할 인스턴스의 스펙을 선택한다. 필요한 스펙을 확인하여 선택한다.
테스트를 위해 프리티어(가입 후 1년 무료)로 결정한다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_0C3CFBECFD65F946C540A0776389D5237E67C249764DC95BAD7C19793094B044_1574914863707_image.png)

## 4. 인스턴스 세부 정보 구성
같은 스펙의 2개의 인스턴스가 필요하기 때문에 인스턴스 개수를 2로 시작한다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_0C3CFBECFD65F946C540A0776389D5237E67C249764DC95BAD7C19793094B044_1574914948389_image.png)

## 5. 스토리지 추가
인스턴스 비용과 별개로 스토리지 비용이 과금된다. 필요한 크기와 종류를 선택하면 된다.
디폴트로 SSD 8GB를 사용할 수 있다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_0C3CFBECFD65F946C540A0776389D5237E67C249764DC95BAD7C19793094B044_1574915010340_image.png)

## 6. 태그 추가
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_0C3CFBECFD65F946C540A0776389D5237E67C249764DC95BAD7C19793094B044_1574915052802_image.png)

## 7. 방화벽 보안 그룹 설정
EC2는 보안 그룹으로 허용한 포트만 접근할 수 있다.
필요한 포트와 허용 IP를 설정하면 된다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_0C3CFBECFD65F946C540A0776389D5237E67C249764DC95BAD7C19793094B044_1574915259027_image.png)

## 8. 인스턴스 시작 검토
지금까지 설정한 인스턴스 정보 요약 정보를 확인한다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_0C3CFBECFD65F946C540A0776389D5237E67C249764DC95BAD7C19793094B044_1574915381966_image.png)

## 9. 키페어 생성
ssh 접근을 위한 프라이빗 키를 생성할 수 있다.
![](https://csy-image-uploader-bucket.s3.ap-northeast-2.amazonaws.com/image/s_0C3CFBECFD65F946C540A0776389D5237E67C249764DC95BAD7C19793094B044_1574915448018_image.png)
