# Elasticsearch

## Ladda ner Elasticsearch (och Kibana)

Elastic: https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.6.0.tar.gz

Kibana, Linux: https://artifacts.elastic.co/downloads/kibana/kibana-6.6.0-linux-x86_64.tar.gz
Kibana, Mac: https://artifacts.elastic.co/downloads/kibana/kibana-6.6.0-darwin-x86_64.tar.gz

## Starta Elasticsearch

Öka gärna minnet till något mer än default:

`export ES_JAVA_OPTS="-Xms1g -Xmx1g"`

Starta Elasticsearch: `elasticsearch-6.6.0/bin/elasticsearch`

## Lägg in data

curl -XPOST localhost:9200/vivill/_doc/_bulk -H "Content-Type: application/x-ndjson" --data-binary @vivill.json

## Sök

Starta Kibana `kibana-6.6.0/bin/kibana`, gränssnittet finns tillgängligt på http://localhost:5601/app/kibana#/dev_tools/

Testa en sökning

```
GET vivill/_search
{
  "query": {
    "match": {
      "text": "globalisering"
    }
  },
  "highlight": {
    "fields": {
      "text": {}
    }
  }
}
```

Endast exakta träffar på "globalisering" visas i highlightningen

### Automatisk mappning

`GET vivill/_mapping` visar nuvarande mappning

### Lägg in en mappning

I Kibana, ta först bort indexet med `DELETE vivill`

Skapa indexet: `PUT vivill`

Och lägg in en mappning:
```
PUT vivill/_mapping/_doc 
{
  "properties": {
    "text": {
      "type": "text",
      "analyzer": "swedish"
    }
  }
}
```

Gör samma sökning som ovan, nu visas även träffar på t.ex. "globaliserings"


## cURL

Alla anrop går att göra via cURL istället, lägg på parametern `pretty` för att få finformaterat resultat

## Python

`elasticsearch-py` fungerar bra för att söka i Elasticsearch.

`pip install elasticsearch`. Se https://elasticsearch-py.readthedocs.io/en/master/








