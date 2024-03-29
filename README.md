# Docker-nginx-vhost
![image](https://github.com/ldh0308/docker-nginx-vhost/assets/142721325/b4a8245b-6560-4cda-987a-b0bf612f8443)

# Load-balancing
https://www.nginx.com/resources/glossary/load-balancing/

# Step #1
- docker rm * rmi
```
$ sudo docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
# Step #2
컨테이너 3개 이미지 1개 생성
```
$ sudo docker run -itd -p 8002:80 --name serv-a nginx
$ sudo docker run -itd -p 8003:80 --name serv-b nginx
$ sudo docker run -itd -p 8001:80 --name lb nginx:latest
```

# Step #3
```bash
$ mkdir config
$ cd config
$ vi default.conf

//nginx 문법
upstream serv {
    server serv-a:80;
    server serv-p:80;
}
server {
    listen 80;

    location /
    {
        proxy_pass http://serv;
    }
}
```

# Step #4
```
$ sudo docker exec -it lb bash // 서버접속
root@c625f68dae89:/# cd /etc/nginx/
root@c625f68dae89:/etc/nginx# ls
conf.d  fastcgi_params  mime.types  modules  nginx.conf  scgi_params  uwsgi_params
root@c625f68dae89:/etc/nginx# ls -l
total 32
drwxr-xr-x 1 root root 4096 Feb 14 02:17 conf.d
-rw-r--r-- 1 root root 1007 Oct 24 13:46 fastcgi_params
-rw-r--r-- 1 root root 5349 Oct 24 13:46 mime.types
lrwxrwxrwx 1 root root   22 Oct 24 16:10 modules -> /usr/lib/nginx/modules
-rw-r--r-- 1 root root  648 Oct 24 16:10 nginx.conf
-rw-r--r-- 1 root root  636 Oct 24 13:46 scgi_params
-rw-r--r-- 1 root root  664 Oct 24 13:46 uwsgi_params

$sudo docker cp config/default.conf lb:/etc/nginx/conf.d/

//serv-a, b 인덱스 파일 복사하기
$ sudo docker cp serv-a/index.html serv-a:usr/share/nginx/html/
$ sudo docker cp serv-b/index.html serv-b:usr/share/nginx/html/

```
$ sudo docker restart lb


# Step #5
```bash
$ sudo docker ps | rev | cut -d' ' -f1 | rev

$ sudo docker logs lb // 로그 보기 증상과 로그 기록(전체)

$ apt install telnet
```
# Step #6 (network)
```bash
$ sudo docker network ls

$ sudo docker network create ablb

$ sudo docker network inspect ablb

$ sudo docker network connect ablb serv-a
```
# Step #7 (8001 포트로만 접속하기)
```bash
$ sudo docker stop lb

$ sudo docker stop serv-a
$ sudo docker stop serv-b

$ sudo docker commit serv-a ldh0308/serv-a:0.1.0  (스냅샷)
$ sudo docker commit serv-b ldh0308/serv-b:0.1.0  (스냅샷)

$ sudo docker rm serv-a
$ sudo docker rm serv-b

$ sudo docker run --name serv-a -d ldh0308/serv-a:0.1.0 //-p 옵션을 제외한다.
$ sudo docker run --name serv-b -d ldh0308/serv-b:0.1.0

$ sudo docker network inspect ablb

$ sudo docker network connect ablb serv-a
$ sudo docker network connect ablb serv-b

$ sudo docker restart lb

실습
```

# Step #8 작동확인
![image](https://github.com/ldh0308/docker-nginx-vhost/assets/142721325/60d9ef17-0005-47a3-8d3f-1cab34c17b4e)
![image](https://github.com/ldh0308/docker-nginx-vhost/assets/142721325/3bd16cd5-3569-48ea-8ac6-ef45e28993b0)
![image](https://github.com/ldh0308/docker-nginx-vhost/assets/142721325/30d14a79-2fa4-4490-ad58-a28d35fba747)





### Ref
https://github.com/pySatellite/docker-nginx-vhost
- Dockfile
  https://github.com/pySatellite/docker-nginx-vhost
