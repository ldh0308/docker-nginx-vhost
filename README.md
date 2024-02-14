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

# Setp #3
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
}%
```


### Ref
https://github.com/pySatellite/docker-nginx-vhost
