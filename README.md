# sergey-werk_microservices

## Docker
```
docker version
docker info
docker images
docker run hello-world
docker ps
docker ps -a
docker run -it ubuntu:18.04 /bin/bash
docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.CreatedAt}}\t{{.Names}}"
docker start <container_id>
docker attach <container_id>

Ctrl+p,Ctrl+q  — escape attached container.
docker run = docker create + docker start + docker attach*
*при наличии опции -i

docker exec -it <u_container_id> top
docker commit <u_container_id> yourname/ubuntu-tmp-file
---
docker kill $(docker ps -q)
docker system df
docker rm $(docker ps -a -q) # удалит все незапущенные контейнеры
docker rmi $(docker images -q)
```

## Docker-machine
```
Команда создания - docker-machine create <имя>. Имен может бытьмного, переключение между ними через eval $(docker-machine env<имя>). Переключение на локальный докер - eval $(docker-machineenv --unset). Удаление - docker-machine rm <имя>

docker-machine -v
docker-machine create <имя>.
eval $(docker-machine env <имя>)
eval $(docker-machine env --unset)
docker-machine rm <имя>

docker-machine create \
  --driver generic \
  --generic-ip-address=1.2.3.4 \
  --generic-ssh-user yc-user \
  --generic-ssh-key ~/.ssh/id_rsa \
  docker-host

docker-machine ls
eval $(docker-machine env docker-host)
```

```
docker build -t reddit:latest .
docker run --name reddit -d --network=host reddit:latest
docker login
docker tag reddit:latest <your-login>/otus-reddit:1.0
docker push <your-login>/otus-reddit:1.0
*
docker run --name reddit -d -p 9292:9292 <your-login>/otus-reddit:1.0
```

```
docker logs reddit -f
docker diff reddit
```

```
docker inspect /otus-reddit:1.0
docker inspect /otus-reddit:1.0 -f '{{.ContainerConfig.Cmd}}'
docker run --name reddit -d -p 9292:9292 /otus-reddit:1.0
```

### Network

Port formats:
* ip_addr:hostPort:containerPort/proto
* ip_addr:hostPort:containerPort
* hostPort:containerPort
* containerPort (будет работать как -P)

```
$ docker run -d --network=reddit  -p 9292:9292 --name=ui_reddit express42/ui:1.0
$ docker run --network reddit tutum/dnsutils nslookup ui_reddit 127.0.0.11

docker network create -d bridge --subnet 192.168.70.0/24 my_bridge
docker network ls
 
docker run --rm -dt --name c_4 --network my_bridge alpine

```
Запускаем в двух сетях
```
docker network create back_net --subnet=10.0.2.0/24
docker network create front_net --subnet=10.0.1.0/24

docker run -d --network=back_net --network-alias=post_db --network-alias=comment_db  --name mongo_db --mount source=reddit_db,target=/data/db mongo:latest
docker run -d --network=back_net --name post sergeyryzh/post:2.1 
docker run -d --network=back_net --name comment sergeyryzh/comment:2.1 
docker run -d --network=front_net --name ui -p 9292:9292 sergeyryzh/ui:2.1

docker network connect front_net post
docker network connect front_net comment
```

```
docker network ls
sudo iptables -nL -t nat
ps ax | grep docker-proxy
```

### Linter

```
docker run --rm -i hadolint/hadolint < dockermonolith/Dockerfile
```

### Run microservices (HW docker-3)
```
docker pull mongo:latest


docker build -t sergeyryzh/post:2.1 ./post-py
docker build -t sergeyryzh/comment:2.1 ./comment
docker build -t sergeyryzh/ui:2.1 ./ui


docker network create reddit
docker volume create reddit_db
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db --mount source=reddit_db,target=/data/db mongo:latest
docker run -d --network=reddit --network-alias=post sergeyryzh/post:2.1 
docker run -d --network=reddit --network-alias=comment sergeyryzh/comment:2.1 
docker run -d --network=reddit -p 9292:9292 sergeyryzh/ui:2.1

```


### Docker compose

```


```

### Docker + Gitlab CI
```
web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'gitlab.example.com'
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'http://130.193.50.206'
  ports:
    - '80:80'
    - '443:443'
    - '2222:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab


stages:
  - build
  - test
  - deploy

build_job:
  stage: build
  script:
    - echo 'Building'

test_unit_job:
  stage: test
  script:
    - echo 'Testing 1'

docker exec -it gitlab-runner gitlab-runner register \
    --url http://130.193.50.206/ \
    --non-interactive \
    --locked=false \
    --name DockerRunner \
    --executor docker \
    --docker-image alpine:latest \
    --registration-token XXX-XXX-XXX \
    --tag-list "linux,xenial,ubuntu,docker" \
    --run-untagged

docker exec -it gitlab-runner gitlab-runner register --help

```
