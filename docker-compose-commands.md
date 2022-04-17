# Docker Compose commands
  * [Start containers](#start-containers)
  * [Stop containers](#stop-containers)

For the sequence, refer to lecture *Install & Run Kafka using Docker*

----

## Start containers

```bash
docker-compose -f docker-compose-core.yml -p core up -d
docker-compose -f docker-compose-connect.yml -p connect up -d
docker-compose -f docker-compose-connect-sample.yml -p connect-sample up -d
docker-compose -f docker-compose-full.yml -p full up -d
docker-compose -f docker-compose-full-sample.yml -p full-sample up -d
```

## Stop containers

```bash
docker-compose -f docker-compose-core.yml -p core down
docker-compose -f docker-compose-connect.yml -p connect down
docker-compose -f docker-compose-connect-sample.yml -p connect-sample down
docker-compose -f docker-compose-full.yml -p full down
docker-compose -f docker-compose-full-sample.yml -p full-sample down
```

*Note : If you want to start fresh, also delete the `data` subfolder, which exists on same folder with the docker compose scripts.*

----

[Back to index](/spring-kafka-bootcamp)
