# Elasticsearch

## Ladda ner Elasticsearch (och Kibana)

Elastic: https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.6.0.tar.gz

Kibana, Linux: https://artifacts.elastic.co/downloads/kibana/kibana-6.6.0-linux-x86_64.tar.gz

Kibana, Mac: https://artifacts.elastic.co/downloads/kibana/kibana-6.6.0-darwin-x86_64.tar.gz

## Starta Elasticsearch

Öka minnet till något mer än default om du får problem:

`export ES_JAVA_OPTS="-Xms1g -Xmx1g"`

Starta Elasticsearch: `elasticsearch-6.6.0/bin/elasticsearch`

Elasticsearch kör på http://localhost:9200.

## Lägg in data

`curl -XPOST localhost:9200/vivill/_doc/_bulk -H "Content-Type: application/x-ndjson" --data-binary @vivill.json`

## Sök

Starta Kibana `kibana-6.6.0/bin/kibana`, gränssnittet finns tillgängligt på http://localhost:5601, klicka på Dev tools.

Testa en sökning:

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

Endast exakta träffar på "globalisering" visas i highlightningen.

### Automatisk mappning

`GET vivill/_mapping` visar nuvarande mappning.

### Lägg in en mappning

I Kibana, ta först bort indexet med: `DELETE vivill`

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

Gör om indexeringen:

`curl -XPOST localhost:9200/vivill/_doc/_bulk -H "Content-Type: application/x-ndjson" --data-binary @vivill.json`

Gör samma sökning som ovan, nu visas även träffar på t.ex. "globaliserings".

De fälten som man inte väljer att ge en explicit mappning får det automatiskt istället.

### Olika saker att testa

Tips: lägg in `"_source": {"exclude": ["text"]}` för att slippa se den långa texten i reslutatet, men ha kvar highlights
för att veta vad som träffar.

#### `boolean`-query

https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html

#### Aggregations

T.ex. terms aggregations på parti eller år

https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html

#### Lägga in datummappning på `datefrom`, `dateto`

https://www.elastic.co/guide/en/elasticsearch/reference/current/date.html

#### `multi_match`, sök i flera fält samtidigt

https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-multi-match-query.html

T.ex. på titel och text

#### `copy_to`, skapa ett nytt fält och indexera flera fält dit

https://www.elastic.co/guide/en/elasticsearch/reference/current/copy-to.html

Testa att söka i det nya fältet.

## cURL

Alla anrop går att göra via cURL istället, lägg på parametern `pretty` för att få finformaterat resultat

## Python

`elasticsearch-py` fungerar bra för att söka i Elasticsearch.

`pip install elasticsearch`. Se https://elasticsearch-py.readthedocs.io/en/master/








