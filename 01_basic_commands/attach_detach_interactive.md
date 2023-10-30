## Attach & Detach mode in Docker

- Both modes are related to the output of the container. Nothing to do with the `input`.

### Attach mode

- The output of the container is attached to the terminal where the docker run command was executed.

- The container is running in the foreground.

### Detach mode

- The output of the container is not attached to the terminal where the docker run command was executed.

- The container is running in the background.

### By default, `docker run` command runs in `attach mode`.
### By default, `docker start` command runs in `detach mode`. 

## ~> To run a container in `detach mode` using `docker run` command, use `-d` option.

```bash
$ docker run -d ubuntu:latest sleep 1000
```

## ~> To run a container in `attach mode` using `docker start` command, use `-a` option.

```bash
$ docker start -a <container-id> # start command is used to start a stopped container.
```

## ~> To run a container in `detach mode` using `docker start` command, use `-d` option.

```bash
$ docker start -d <container-id>
```

---

## Interactive mode in Docker

- Interactive mode is related to the `input` of the container.

- By default, the container runs in `non-interactive mode`.

- In `non-interactive mode`, the container does not accept any input from the user.

- In `interactive mode`, the container accepts input from the user.

- To run a container in `interactive mode`, use `-i` option.

- Typically, `interactive mode` is used with `tty` mode (pseudo terminal).

```bash
$ docker run -it ubuntu
```


---

## Attach to a running container

- To attach to a running container, use `docker attach` command.

```bash
$ docker attach <container-id> # useful when the container is running in the detach mode.
```

---

## See the logs of a container

- To see the logs of a container, use `docker logs` command.

```bash
$ docker logs <container-id>
```

## See the logs of a container in real time

- To see the logs of a container in real time, use `docker logs -f` command.

```bash
$ docker logs -f <container-id>
```