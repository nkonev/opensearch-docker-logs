# Opensearch Log Collector for Docker Containers

Opensearch based container log collector sample with fluentbit.

## Running

`docker-compose.yml` starts the [Spring PetClinic Sample Application](https://github.com/spring-projects/spring-petclinic) container and collect logs via [*fluentbit*](https://fluentbit.io/) and [*opensearch*](https://opensearch.org/).

```sh
docker compose up -d
docker compose logs -f
```

Open in browser http://localhost:8080/vets to produce logs

Queries
```sh
curl -Ss -XGET "http://localhost:9200/_list/indices"

# https://opster.com/guides/elasticsearch/data-architecture/elasticsearch-index-pattern/
curl -Ss -X GET 'http://localhost:9200/logstash-*/_search' -H 'Content-Type: application/json' -d'
{
  "size": 1000,
  "query": {
    "match_all": {}
  },
  "sort" : [
    { 
        "@timestamp" : "asc"
    }
  ]
}
' | jq
```

Dashboards http://localhost:5601/opensearch

Saved objects https://forum.opensearch.org/t/is-there-a-saved-objects-api-in-opensearch/20625

```sh
curl -Ss -X POST "http://localhost:5601/opensearch/api/saved_objects/_export" -H "osd-xsrf: true" -H "Content-Type: application/json" -d'
{
  "type": ["index-pattern", "search", "visualization", "dashboard"]
}' > saved.ndjson
```

Importing https://github.com/opensearch-project/OpenSearch-Dashboards/issues/1723#issuecomment-1154803199
```sh
curl -X POST "http://localhost:5601/opensearch/api/saved_objects/_import?overwrite=true" -H "osd-xsrf: true" --form file=@saved.ndjson
```

SO - Kibana Search within text for string https://stackoverflow.com/questions/42514737/kibana-search-within-text-for-string

Observability -> Logs -> PPL
```
source=logstash-*
```

Get all the objects
```sh
curl -Ss -X GET 'http://localhost:9200/_cat'
```

Batching
* https://github.com/fluent/fluent-bit/issues/551#issuecomment-376945772
* https://docs.fluentbit.io/manual/administration/buffering-and-storage

