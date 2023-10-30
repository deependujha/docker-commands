## `Bind Mounts` vs `Copy`

- Though, we can ignore `copy` command in the Dockerfile and use `bind mounts` to share data between the host machine and the container, it is not recommended to do so.

- This is because, `bind mounts` are used in `development environments` and `copy` command is used in `production environments`.

- When we create an image and `bind mount` the working directory to the container, when we make changes, those changes are reflected in container. 

- But, our image hasn't changed. So, when we deploy our application, the changes won't be reflected in the container. Also, `bind mounts` are not used in `production environments`.

- So, when we create an image, we should use `copy` command in the Dockerfile to copy the files from the host machine to the container.

---

## `Dockerignore` file

- Similar to `.gitignore` file.
- we can create a `.dockerignore` file to `ignore files and directories` when copying to the container.

```.dockerignore
node_modules
temp/
logs/
.git
.gitignore
Dockerfile
```

---

## `ARG` and `ENV` in Docker

`ARG` and `ENV` are dockerfile instructions through which you can `apply the different configurations`. However, both look pretty similar when you look at the dockerfile. Some of the notable differences between these two are below:

- `ARG` is used to pass the arguments to the dockerfile at the build time of the image.

- `ENV` is used to set the environment variables inside the container.

- `ARG` parameters are applied only during the docker image building process; they are `unavailable once you have built the image`.

- The running containers cannot access ARG values. 

- **Some examples of ARG arguments include a version of your `Ubuntu or a version of a library`.**
<br/>
- **`You can specify a default value for ARG parameters in dockerfile, and you can modify them during the creation of the build of the image`**.

<br/>

- **`You can pass ENV variables not only during the image building but also at runtime when your containers are running`**.

- **ENV variables are usually your API keys, database URLs, secret keys, etc**. 

---

### Summary:

- `ARG`: To specify the arguments to the dockerfile at the build time of the image.

- `ENV`: To set the environment variables inside the container.

#### ~> BUILD TIME `ARGUMENTS` & RUN TIME `ENVIRONMENTS` 😎

---

## `ARG` and `ENV` in Dockerfile

### `ENV`:

- `ENV` is used to set the environment variables inside the container.

```dockerfile
ENV PORT 3000
ENV NODE_ENV production


EXPOSE $PORT 
# $PORT is replaced with 3000

# the above two lines are equivalent to:
# ENV PORT=3000 NODE_ENV=production
```

- To set environment variables in the container, we use the command with `--env <key>=<value>` flag.

```bash
docker run --env PORT=3000 --env NODE_ENV=production <image-name>
```

- To set multiple environment variables, we can use the `--env-file <file-name>` flag.

```bash
docker run --env-file ./.env <image-name>
```

---

### `ARG`:

- Used to pass the arguments to the dockerfile at the build time of the image.

- We don't include `secret keys` using `ARG` because they are `visible in the image history`.

```bash
docker image history <image-name>
```

- Using `ARG` in Dockerfile

```dockerfile
ARG PORT=3000

EXPOSE $PORT
# ARG can be used everywhere in Dockerfile, except in `CMD` because it is used at `runtime`.
```

- To pass arguments to the dockerfile, we use the `--build-arg <key>=<value>` flag.

```bash
docker build --build-arg PORT=3000 -t <image-name> .
```

- To pass multiple arguments, we can use the `--build-arg-file <file-name>` flag.
