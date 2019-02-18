https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.6.0.tar.gz

https://artifacts.elastic.co/downloads/kibana/kibana-6.6.0-linux-x86_64.tar.gz
https://artifacts.elastic.co/downloads/kibana/kibana-6.6.0-darwin-x86_64.tar.gz


Ändra heapstorlek
export ES_JAVA_OPTS="-Xms4g -Xmx4g"

elasticsearch-6.6.0/bin/elasticsearch
kibana-6.6.0/bin/kibana

1. Indexera data python script
2. Exempelmappningar via kibana
3. Exempelqueries
4. Aggregations
5. Highlighting
6. GET _cat/health?v
7. Starta två instanser för att göra ett kluster
8. GET _cat/health?v

### cURL

Lägg på parametern `pretty` för att få finformaterat resultat

### Python

`pip install elasticsearch`. Se https://elasticsearch-py.readthedocs.io/en/master/
