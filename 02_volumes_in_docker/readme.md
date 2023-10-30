# Docker Volumes 🛳️

- To persist data in docker containers even after containers are removed, we use volumes.

- Volumes are stored in the host machine and are mounted to the containers.

- Volumes are used to share data between containers.

- It is of two types:
  - Named Volumes (Recommended) - To store data in the host machine.
  - Anonymous Volumes (Not Recommended)

- There is another way to share data between the host machine and the container, called **`Bind Mounts`**. Bind mounts are used in `development environments`.


#### `Anonymous Volumes` & `Named Volumes` are managed by `Docker`. 
#### But, `Bind Mounts` are not managed by `Docker`. They are just mapped to a path in the host machine.

---

## Attaching a volume to a container

- To attach a volume to a container, we use the `-v` flag.

- The `-v` flag takes two arguments, the first is the `path to the volume in the host machine` and the second is the `path to the volume in the container`.

```bash
# named volume - we don't specify the path to the volume in the host machine. Docker creates the volume in the host machine.
docker run -v myVolumeName:/var/lib/mysql mysql  # Attaching a volume to a container
```

- The name that we give to the volume is the name of the volume in the host machine. If volume with that name already exists, then docker will use that volume. Otherwise, docker will create a new volume with that name.

- To create a named volume, we use the `docker volume create <volume-name>` command.

```bash
# named volume
docker volume create myVolumeName  # Creating a named volume
```

---

## Docker Volume Commands

- To list all the volumes in the host machine, we use the `docker volume ls` command.

- To create a volume, we use the `docker volume create <volume-name>` command.

- To inspect a volume, we use the `docker volume inspect <volume-name>` command.

- To remove a volume, we use the `docker volume rm <volume-name>` command.

- To remove all the unused volumes, we use the `docker volume prune` command.

---

## Bind Mounts

- Bind mounts are used to share data between the host machine and the container.

- Mostly used in **`development environments`**.

- To attach a bind mount to a container, we use the `-v` flag with the `absolute path to the volume in the host machine` and the `absolute path to the volume in the container`.

```bash
# bind mount
docker run -v /home/username/myVolumeName:/var/lib/mysql mysql  # Attaching a bind mount to a container

# we can also use: `$(pwd)` to get the current working directory
```

## Anonymous Volumes

- Anonymous volumes are created by docker and are not named.

- They survive container restarts but not container removals.

- They are not used for storing data. But, you can use them to prioritize the container's internal paths higher than external paths.

- To create an anonymous volume, we use the `-v` flag with the `absolute path to the volume in the container`.

```bash
# anonymous volume
docker run -v /var/lib/mysql mysql  # Attaching an anonymous volume to a container
```

- To create an anonymous volume in Dockerfile, we use the `VOLUME` instruction.

```dockerfile
VOLUME ["/var/lib/mysql"]
```

- A great use case for anonymous volumes is to store `node_modules` in a container. If we're using `bind mount`, then as soon as container starts, the files are copied from host machine to container and the `node_modules` directory is replaced with the one in the host machine. This is not what we want. We want to use the `node_modules` directory in the container. So, we use anonymous volumes.

```bash
# anonymous volume
docker run -v /app/node_modules -v $(pwd):/app -v myNamedVolume:/app/db <image-name> 

# -v /app/node_modules => anonymous volume
# -v $(pwd):/app => bind mount
# -v myNamedVolume:/app/db => named volume
```

- Anonymous volumes used in above case to survive `node_modules`. When container is started, the `bind mount` will try to copy all the files and contents from the host machine to the container. But, the anonymous volume will prevent the `node_modules` directory from being replaced with the one in the host machine.

--- 

## `Read Only` `Bind Mounts`

- To create a read only volume, we use the `:ro` after the path to the volume in the container.

```bash
# read only bind mount
docker run -v /app/node_modules:/app:ro <image-name> 
```

- This will prevent the container from writing to the volume.

### But, what if we want to allow writing to some directories in the volume and left the others read only?

- To do this, we use the `:ro` after the path to the volume in the container. And, then we create anonymous volumes for the directories we want to write to.

```bash
# read only bind mount with anonymous volumes for writing
docker run -v /app/node_modules -v /app/db -v /app/public -v $(pwd):/app:ro <image-name> 
```

