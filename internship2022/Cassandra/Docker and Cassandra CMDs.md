**Execute cqlsh in a docker container**
- docker exec -it cassandra cqlsh

**Basic cassandra cmds**
- describe keyspaces;
- choose your keyspace with `USE KEYSPACE`

**Execute bash commands in a DOCKER container**
```bash
docker exec -t -i mycontainer /bin/bash
```

**Copy data from a csv file in a table**
```shell
docker cp 'video.csv' docker_id:/
```

**Check ip @ and port for every application**
```shell
netstat -anpe
```

**To connect your scala application to cassandra that is in a docker container**
You need to publish the ports of the container when creating the cassandra container, it is not published by default + maybe some ports mapping is required.
https://docs.docker.com/config/containers/container-networking/
https://stackoverflow.com/questions/34645846/cannot-connect-to-cassandra-docker-with-cqlsh
