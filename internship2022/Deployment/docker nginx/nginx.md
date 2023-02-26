1. Start a docker container with nginx image
2. Copy source files into src directive
3. Start server

## Build an image from Dockerfile

### Remove container
```shell
Docker rm container
```

### Remove image
```
Docker rmi image
```

### Build image
```
docker build --tag name .
```

### Run container
#### Nginx server
```shell
docker run --rm -d --name znginx -p 5050:80 front
```

#### Akka-http server
```shell
docker run --rm -t -d --name back_server -p 8080:8080 akka_server
```
