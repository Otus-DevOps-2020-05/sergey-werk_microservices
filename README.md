# sergey-werk_microservices
sergey-werk microservices repository

## Docker
'''
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
Ctrl+p, Ctrl+q
docker exec -it <u_container_id> top
docker commit <u_container_id> yourname/ubuntu-tmp-file
---
docker kill $(docker ps -q)
docker system df
docker rm $(docker ps -a -q) # удалитвсенезапущенныеконтейнеры
docker rmi $(docker images -q)
'''

'''
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
'''

'''
docker build -t reddit:latest .
docker run --name reddit -d --network=host reddit:latest
docker login
docker tag reddit:latest <your-login>/otus-reddit:1.0
docker push <your-login>/otus-reddit:1.0
*
docker run --name reddit -d -p 9292:9292 <your-login>/otus-reddit:1.0
'''

'''
docker logs reddit -f
docker diff reddit
'''

'''
docker inspect /otus-reddit:1.0
docker inspect /otus-reddit:1.0 -f '{{.ContainerConfig.Cmd}}'
docker run --name reddit -d -p 9292:9292 /otus-reddit:1.0
'''


## Docker 2

### Linter

'''
docker run --rm -i hadolint/hadolint < dockermonolith/Dockerfile
'''

### Run microservices (HW docker-3)
'''
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

'''


