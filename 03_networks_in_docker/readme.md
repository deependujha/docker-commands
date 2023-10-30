# Networks in Docker 🛳️

## There can be 3 different types of networks-communication in Docker:

1. **Container to WWW** => `Works out of the box. We don't have to do anything.`

<br/>

2. **Container to Host Machine** => We just have to replace `localhost:PORT` with **`host.docker.internal:PORT`**. It is a special DNS name which resolves to the internal IP address of the host machine.

<br/>

3. **Container to Container**

- Create a network => `docker network create my-network`
- Run a container in that network => `docker run --network my-network --name my-nginx nginx`
- Run another container in that network => `docker run --network my-network --name my-ubuntu ubuntu`

- Now, **`container names can be used as their IP address`**. For IP address of `my-nginx` container, we can use `my-nginx` as the hostname. Similarly, for IP address of `my-ubuntu` container, we can use `my-ubuntu` as the hostname.