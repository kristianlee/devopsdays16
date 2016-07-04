#Going Elastic


This workshop gave an introduction to Elastic's products, and then included a practical to go through and set up our own ES instance. 

The slides are [here](ElasticStack.pdf), and the repo containing the vagrant image for the interactive workshop is [here](https://github.com/xeraa/vagrant-elastic-stack)

##My Notes

###Elastic Products
**ELK stack**
Elastic - search
Kibana - visualisations + easy way to explore the data in ES. 
Logstash - ingest other data. 
 
**Beats** 
Logstash is heavy, started as ruby, but then jruby. Beats is written in go. Includes a new slimline forwarder, and enrichment for logstash = you have IP address in logs, then enrichment looks up the ip address location with city etc. 

As a whole, the suite is now called **Elastic Stack** 

**Xpack** = commercial plugins for elastic stack. 
Security, Alerting, Monitoring, Graph

Versioning - 
Right now, it's a bit screwed up, with each product on its own version tree (since some have been acquired etc). Next version will be 5.0 for all the components. 

###Beats
Beats are split up (+ community versions), we went through:
Filebeat - shipper for log file data
Metricbeat - metrics shipper
Packetbeat - similar to an aggregated wireshark. 

###Kibana
V3 - dashboards were in their own format.
V4 - rewritten to node, angular etc, all dashboards have to be re-written
V5 - interface different, but same format. 



###ES
ES 5 == lucene 6. Lucene = the engine of ES. ES does all the distribution of the data + replication + query url + REST API, to actually store the data is Apache Lucene. 

In ES5 Sense has been renamed to 'console' and it installed by default now. 

`GET /_cat/health?v`
Healthcheck for the cluster. Status - yellow = data isn't replicated (maybe because only 1 node). 
Red = ES can't read all the data you've stored. 
`GET /_cat/indices?v`
Find out about the indexes
`GET /_cat/shards?v`
Find out about the shards

Cluster always has a name (by default = elasticsearch), previously all nodes on the network used to look for elasticsearch. 
In V2, now you have to provide the DNS entries / IP addressses for another node in the cluster. 

Index is created, and stretches over the 2 nodes. By default you have 5 shards, with replication factor of 1 (so all data will be replicated). If only 1 node, we don't replicate the data. 

Adding a node, will result in primary shards being redistributed + replica shards. 

On node loss secondaries would be promoted to primaries, and new replicas would be created. 

```
PUT /movies 
PUT /movies/movie/1
{
  "title": "The Godfather",  "director": "Francis Ford Coppola",  "year": 1972
}GET /movies/movie/1GET /movies/_mapping
```

Movie = a type, an equivalent of that sort of information you have (not quite a table). 

_mapping = similar to a schema

####Querying:

`/_search` = search all indices in ES cluster
`/movies/_search` = only search the movies type

Searches are case insensitive, singular and plural insensitive (different to relational databases, which are about absolute information). 

Fuzzy search add a ~ to the end of the query

All queries etc can be done in JSON (and received back in JSON too)

Score = how ES finds the most relevant documents for you. In the forumlae :
Tf = term frequency. If 1 document contains a word multiple times, it's probably more relevant than a doc that contains a word only once. 
Idf = searching multiple terms - searching for less common terms, matching those will have higher relevancy. 

Filters (hard constraints, no scoring, just "give me all the movies from the year 1972") - cheaper searches. 

'must' = query must have these
'should' = should ranked higher if included

Aggregation - counts for a specific option

Timelion = for time series. Resaerch project at the moment, intention to make it a full product

