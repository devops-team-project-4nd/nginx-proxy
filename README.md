# nginx-proxy


## STEP 1 
Nginx Pull </br>
https://hub.docker.com/_/nginx
```
$ docker pull nginx
$ docker images
REPOSITORY               TAG       IMAGE ID       CREATED       SIZE
nginx                    latest    e4720093a3c1   13 days ago   187MB
```
## STEP 2
### Run
```
$ docker run -itd --name nginx-proxy -p 9090:80 nginx
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
38f208106552   nginx     "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   0.0.0.0:9090->80/tcp, :::9090->80/tcp   nginx-proxy
```

## STEP 3
### Connecting NGINX proxy with the app server
```
$ docker exec -it nginx-proxy bash
root@38f208106552:/# cd /etc/nginx/conf.d
root@38f208106552:/etc/nginx/conf.d# apt update
root@38f208106552:/etc/nginx/conf.d# apt install vim -y
root@38f208106552:/etc/nginx/conf.d# vi default.conf
server {
    listen 80;
    server_name 172.18.143.34;

    location / {
        proxy_pass         http://192.168.0.50:8080/;
    }
}

root@38f208106552:/etc/nginx/conf.d# nginx -s reload
```

## STEP 4
### Nginx proxy Test
<img src="https://github.com/devops-team-project-4nd/nginx-proxy/assets/91647614/be637b2c-ff79-4be5-af7c-cef3be0dc3f3"  style="width:70%;">

## STEP 5
### Network Fowarding
https://github.com/beyond-sw-camp/beyond-sw-camp-be01_4nd_mini-project/issues/7
- cmd창 관리자 권한으로 실행
```
$ netsh interface portproxy add v4tov4 listenport=9090 listenaddress=0.0.0.0 connectport=9090 connectaddress=172.18.143.34
$ netsh interface portproxy show v4tov4
ipv4 수신 대기:             ipv4에 연결:

주소            포트        주소            포트
--------------- ----------  --------------- ----------
0.0.0.0         9090        172.18.143.34   9090
```

## Network Fowarding Test
<img src="https://github.com/devops-team-project-4nd/nginx-proxy/assets/91647614/c52c44e7-38cf-4d7b-ae41-14d9ac3820ab" style="width:70%;">
