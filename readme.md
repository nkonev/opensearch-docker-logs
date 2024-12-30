# Opensearch Log Collector for Docker Containers

Opensearch based container log collector sample with fluentd and fluentbit.

## Running

`docker-compose.yml` starts the [Spring PetClinic Sample Application](https://github.com/spring-projects/spring-petclinic) container and collect logs via [*fluentbit*](https://fluentbit.io/) and [*opensearch*](https://opensearch.org/).

```sh
docker compose up -d
docker compose logs -f
```

### Fluentd

`docker-compose.fluentd.yml` includes [*fluentd*](https://www.fluentd.org/) instead of *fluentbit*.

```sh
docker-compose -f docker-compose.fluentd.yml up -d
```

Open in browser http://localhost:8080/vets to produce logs

Queries
```sh
curl -Ss -XGET "http://localhost:9200/_list/indices"


curl -Ss -X GET "localhost:9200/logstash-2024.12.30/_search" -H 'Content-Type: application/json' -d'
{
  "size": 1000,
  "query": {
    "match_all": {}
  }
}
' | jq
```
