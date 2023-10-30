# Exec command in docker

- A running container will have a running process. We can run a new process inside the container using `exec` command without stopping the container.

- To run a new process inside the container, we can use the following command:

```bash
docker exec -it <container_name> <command> /bin/bash # enter into a running container
```

- Even after exiting the container, the container will be running.

---


## Attach command in docker

- Similar to `exec` command, we can use `attach` command to enter into the primary running process of container.

- To enter into the primary running process of container, we can use the following command:

```bash
docker attach <container_name> # enter into a running container
```

- If we exit the container, the container will be stopped. (If the primary process is stopped, the container will be stopped.)

---

## `RUN` Vs `CMD` Vs `ENTRYPOINT` in docker

- `RUN is used to run commands while building the image`. It is used to install packages and dependencies.

- `CMD is used to run commands when the container is started`. It is used to start the application.

- `ENTRYPOINT` is similar to `CMD`. Keywords passed while running the container will be appended to the entrypoint command.

```dockerfile
# ...
# ...

ENTRYPOINT ["npm"]
```

- When we run the container, we can pass the keywords to the entrypoint command.

```bash
docker run <image_name> install # npm install
```
