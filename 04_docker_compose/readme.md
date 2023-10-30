# Docker-Compose 🚢🛞

- Create a `Dockerfile` for each service.
- Create a `docker-compose.yaml` file.

- `Services` are the containers that will run in the docker-compose network.
- Name of the container will be different from the name of the service. The name of the container will be the name of the service with the project name as a prefix.

- To specify the container name, we can use `container_name` key in the docker-compose file.

```yaml
# This is a dummy Docker Compose file with comments
version: '3.8'  # Specify the Docker Compose version

services:  # Define the services that will run in containers

  # Service 1: Web Application
  web_app:
    image: nginx:latest  # Docker image to use for this service
    ports:
      - "8080:80"  # Map host port 8080 to container port 80
    volumes:
      - ./web_app:/usr/share/nginx/html  # Mount a host directory to a container directory
      - web_app_data:/app  # Named volume
      - /app/node_modules  # anonymous volume
    environment:
      - NODE_ENV=development  # Set environment variables
      - PORT=8080
    container_name: web_app_container  # Specify the container name
    args:
      - arg1=value1  # Specify command-line arguments here
      - arg2=value2
    
    depends_on:  # Specify dependencies (services that must be running first)
      - db
      - redis

  # Service 2: Database
  db:
    image: mysql:5.7  # Docker image for the database
    environment:
      MYSQL_ROOT_PASSWORD: root_password  # Set environment variables
      MYSQL_DATABASE: my_database
      MYSQL_USER: my_user
      MYSQL_PASSWORD: my_password
    volumes:
      - ./db_data:/var/lib/mysql  # Mount a host directory to the database data directory

  # Service 3: Redis Cache
  frontend:
    build:  # Build the image from the Dockerfile in the frontend directory
        context: ./frontend  # Specify the build context
        dockerfile: Dockerfile.dev  # Specify the Dockerfile name. Default is Dockerfile

    ports:
      - "6379:6379"


# Define volumes for persistent data
volumes:
  web_app_data: 
  db_data:
  # the above is the syntax for creating named volumes. We need to specify them explicitly. After `:`, we don't need to specify the path. Docker will automatically create a volume with the name we specified.

```

- We don't necessarily need to write about `networks` in docker-compose file, because docker-compose automatically creates a network for us. And, all the services in the docker-compose file are in that network.

- `depends_on` key is used to specify the dependencies of a service. It is used to specify the services that must be running before the current service can start.

- To run the docker-compose file, we can use the following command:

```bash
docker-compose up
```

- To run the docker-compose file in the background, we can use the following command:

```bash
docker-compose up -d
```

- By default, all the containers are run with `--rm` flag. So, when we stop the containers, they are automatically removed. To prevent this, we can use the following command:

- To tear down the docker-compose network, we can use the following command:

```bash
docker-compose down
```

- But, this will not remove the volumes. To remove the volumes, we can use the following command:

```bash
docker-compose down -v
```

- To see the logs of the containers, we can use the following command:

```bash
docker-compose logs
```

- To see the logs of a specific container, we can use the following command:

```bash
docker-compose logs <container_name>
```

- To see the logs of a specific container in real-time, we can use the following command:

```bash
docker-compose logs -f <container_name>
```

- To run a single container in the docker-compose network, we can use the following command:

```bash
docker-compose run -it --vm <service_name> # --rm flag will be required if we want to remove the container after exiting. Bcoz, this is not `up` command.
```

- To force rebuild the images, we can use the following command:

```bash
docker-compose up --build
```

---

### `ENTRYPOINT` AND `WORKDIR` IN docker-compose file:

- We can specify the `ENTRYPOINT` and `WORKDIR` in the docker-compose file as well.

```yaml
version: '3.8'

service:
  web_app:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./web_app:/usr/share/nginx/html
    environment:
      - NODE_ENV=development
      - PORT=8080
    container_name: web_app_container
    args:
      - arg1=value1
      - arg2=value2
    entrypoint: ["/bin/sh", "-c"]  # Specify the entrypoint
    working_dir: /usr/share/nginx/html  # Specify the working directory

```

- It's generally not a good idea to make docker-compose file too complex. So, it's better to specify the `ENTRYPOINT` and `WORKDIR` in the Dockerfile itself. 

- But, if we want to specify them in the docker-compose file, we can do that. (For smaller images, it's better to specify them in the docker-compose file itself.)
