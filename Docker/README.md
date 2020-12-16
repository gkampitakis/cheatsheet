<h1 align="center">Docker</h1>

## Commands

- Build Docker Image

```bash
docker build -f <Dockerfile name> -t <tag> .
```

- Run image

```bash
docker run -p <host-port>:<container-port> <image-name>
```

- Get container's logs

```bash
docker logs <container id>
# You can use -f, --follow for following logs
```

- Run sh inside docker

```bash
docker exec -it <container id> sh
# You can also override entry point by replacing exec with run
```

- Docker compose stop/run/build

```bash
docker-compose stop/run/build <service name>
```

- Get number of CPUs

```bash
docker info | grep CPUs
```

- Display a live stream of container(s) resource usage statistics
```bash 
docker stats [OPTIONS] [CONTAINER...]
```

- Connect to container from `docker-compose`

```bash
docker-compose exec <service-name> sh
```

[Prune unused Docker Objects](https://docs.docker.com/config/pruning/)