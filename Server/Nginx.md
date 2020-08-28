# Nginx

## How to install Nginx in Ubuntu 18.04
```
$ sudo apt-get install nginx -y
```

## Set port forwarding
```
$ vi /etc/nginx/nginx.conf

# nginx/1.14.0 (ubuntu)
stream{
    # port forwarding 3000 to another address port
    server{
        listen 3000;
        proxy_pass https://[address]:[port];
    }
    # port forwarding 3001 to another address port
    server{
        listen 3001;
        proxy_pass https://[address]:[port];
    }        
}
```


## Issue…
### the page you are looking for is temporarily unavailable. please try again later. 
proxy_pass http://localhost:3000; 으로 했을 때 forwarding이 되지 않고 에러만 나온다,, <br>
검색 해 보니, linux는 본인 서버로 리다이렉팅 하는 것을 허용하지 않는다고 한다 ,, <br>

I was able to find a solution after 2 days of searching. Somehow SELinux was not permitting Nginx to proxy to my server. <br>
Running the command below fixed the issue. <br>
아래의 설정으로 해결할 수 있다.
```
$ /usr/sbin/setsebool httpd_can_network_connect true 
```

### SSL: error:0200100D:system library:fopen:Permission denied
ssl 인증서가 user folder에 있을 때 발생하는 문제,, <br>
단순히 접근 권한의 문제가 아님,, <br>
인증서 경로를 /etc/nginx/ssl 안으로 옮김으로써 해결
```
$ sudo cp /home/ec2-user/.ssl/bundle.pem /etc/nginx/ssl/
```
