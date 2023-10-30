# Docker Commands

## To see all the running containers
```
docker ps
```

## To see all the containers (running and stopped)
```
docker ps -a
```

## To see all the images
```
docker images
```

## To stop a container
```
docker stop <container_id>
```

## To start a container
```
docker start <container_id>
```

## To remove a container
```
docker rm <container_id> # container must be stopped
```

## To remove a running container
```
docker rm -f <container_id>
```

## To remove all the stopped containers
```
docker container prune
```

## To remove an image
```
docker rmi <image_id>
```

## To remove all the images
```
docker image prune # remove all the dangling images
```

## To remove all the images
```
docker image prune -a # remove all the dangling and unused images
```

## To remove all stopped containers, all dangling images, and all unused networks
```
docker system prune
```