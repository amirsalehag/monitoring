# *what is elasticsearch*
elasticsearch is where we store , search and analyse our data.

---
# write access for elastic user
For elastic we need to grant an access to the directory which it wants to read and write data to, we need to specify the  
directory which is mounted for its container an access:  
```
mkdir esdatadir
chmod g+rwx esdatadir
chgrp 0 esdatadir
```
because elastic user is a part of a group called "0" , we can just use that as its permission.  

---
# docker container logs
For redirecting docker container logs into the elasticsearch instance, we need to use its log driver for the containers  
as labels or write a `daemon.json` config for it to be considered for all the containers:  
```
{
  "log-driver" : "elastic/elastic-logging-plugin:7.6.2",
  "log-opts" : {
    "output.elasticsearch.hosts" : "myhost:9200",
    "output.elasticsearch.protocol" : "https",
    "output.elasticsearch.username" : "myusername",
    "output.elasticsearch.password" : "mypassword",
    "output.elasticsearch.index" : "elastic-log-driver-%{+yyyy.MM.dd}"
  }
}
```
check out this [document](https://www.elastic.co/guide/en/beats/loggingplugin/7.6/log-driver-usage-examples.html) for more detail.  

---
# ES not stay's up as container
When setting up ES as container for the first time, if the node goes down the second it comes up, it might be because of this:  
```
sudo sysctl -w vm.max_map_count=262144
```
it might be the answer to the issue.  

---
# configuring ES
We can use its configuration file for configuring ES, or we can dynamically change its configuration through its API,

---
# shards in ES
Data in an Elasticsearch index can grow to massive proportions. In order to keep it manageable, it is split into a number of shards. Each Elasticsearch shard is an Apache Lucene index, with each individual Lucene index containing a subset of the documents in the Elasticsearch index. Splitting indices in this way keeps resource usage under control.The number of shards is set when an index is created, and this number cannot be changed later without reindexing the data. When creating an index, you can set the number of shards and replicas as properties of the index using:  
```
PUT /sensor
{
    "settings" : {
        "index" : {
            "number_of_shards" : 6,
            "number_of_replicas" : 2
        }
    }
}
```
Generally, an optimal shard should hold 30-50GB of data. For example, if you expect to accumulate around 300GB of application logs in a day, having around 10 shards in that index would be reasonable.  

---
# replicas in ES
There are thus two types of shards, the primary shard and a replica, or copy. Each replica is located on a different node, which ensures access to your data in the event of a node failure. In addition to providing redundancy and their role in preventing data loss and downtime, replicas can also help boost search performance by allowing queries to be processed in parallel with the primary shard, and therefore faster.  
If a primary shard becomes unavailable—for example, due to a node disconnection or hardware failure—a replica is promoted to take over its role.  

---
