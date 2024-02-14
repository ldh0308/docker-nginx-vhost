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
```
$ docker run -itd -p 8002:80 --name serv-a nginx
$ docker run -itd -p 8003:80 --name serv-b nginx
$ docker run -itd -p 8001:80 --name lb nginx:latest
```

### Ref
https://github.com/pySatellite/docker-nginx-vhost
