# *what is elasticsearch*
Elasticsearch is where we store , search and analyse our data. It allows you to store, search, and analyze huge volumes of data quickly and in near real-time and give back answers in milliseconds. It’s able to achieve fast search responses because instead of searching the text directly, it searches an index. It uses a structure based on documents instead of tables and schemas and comes with extensive REST APIs for storing and searching the data. At its core, you can think of Elasticsearch as a server that can process JSON requests and give you back JSON data.

---
# write access for elastic user
For elastic we need to grant an access to the directory which it wants to read and write data to, we need to specify the  
directory which is mounted for its container an access:  
```
mkdir esdatadir
chmod g+rwx esdatadir
chgrp 0 esdatadir
```
Because elastic user is a part of a group called "0" , we can just use that as its permission.  

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
Most Elasticsearch configuration can take place in the cluster settings API. Certain operations require you to modify elasticsearch.yml and restart the cluster.Whenever possible, use the cluster settings API instead, `elasticsearch.yml` is local to each node, whereas the API applies the setting to all nodes in the cluster.  
Three categories of setting exist in the cluster settings API: persistent, transient, and default. Persistent settings, well, persist after a cluster restart. After a restart, Elasticsearch clears transient settings.  
To change a setting, just specify the new one as either persistent or transient. This example shows the flat settings form:  
```
PUT /_cluster/settings
{
  "persistent" : {
    "action.auto_create_index" : false
  }
}
```
You can also use the expanded form, which lets you copy and paste from the GET response and change existing values:  
```
PUT /_cluster/settings
{
  "persistent": {
    "action": {
      "auto_create_index": false
    }
  }
}
```
---
# ES data architecture and how it organizes data
* Document:  
Documents are the basic unit of information that can be indexed in Elasticsearch expressed in JSON, which is the global internet data interchange format. You can think of a document like a row in a relational database, representing a given entity — the thing you’re searching for. In Elasticsearch, a document can be more than just text, it can be any structured data encoded in JSON. That data can be things like numbers, strings, and dates. Each document has a unique ID and a given data type, which describes what kind of entity the document is.  
* Index:  
An index is a collection of documents that have similar characteristics. An index is the highest level entity that you can query against in Elasticsearch. You can think of the index as being similar to a database in a relational database schema. Any documents in an index are typically logically related. In the context of an e-commerce website, for example, you can have an index for Customers, one for Products, one for Orders, and so on. An index is identified by a name that is used to refer to the index while performing indexing, search, update, and delete operations against the documents in it.  

---
# Node
A node is a single server that is a part of a cluster. A node stores data and participates in the cluster’s indexing and search capabilities. An Elasticsearch node can be configured in different ways:  
* Master Node — Controls the Elasticsearch cluster and is responsible for all cluster-wide operations like creating/deleting an index and adding/removing nodes.
* Data Node — Stores data and executes data-related operations such as search and aggregation.
We also always need a master node and a data node. Check out this [document](https://www.elastic.co/guide/en/elasticsearch/reference/8.5/modules-node.html) for more information.  

---
# shards in ES
Data in an Elasticsearch index can grow to massive proportions. In order to keep it manageable, it is split into a number of shards. Elasticsearch provides the ability to subdivide the index into multiple pieces called shards. Splitting indices in this way keeps resource usage under control.The number of shards is set when an index is created, and this number cannot be changed later without reindexing the data. When creating an index, you can set the number of shards and replicas as properties of the index using:  
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
Index sharding is a process that splits the documents in an index into smaller partitions. These smaller partitions are called shards. The result is that instead of all documents being in one large index, documents are distributed between shards.

---
# replicas in ES
There are thus two types of shards, the primary shard and a replica, or copy. Each replica is located on a different node, which ensures access to your data in the event of a node failure. In addition to providing redundancy and their role in preventing data loss and downtime, replicas can also help boost search performance by allowing queries to be processed in parallel with the primary shard, and therefore faster.  
If a primary shard becomes unavailable—for example, due to a node disconnection or hardware failure—a replica is promoted to take over its role.  
Also worth noting that a primary and its replica shard(s) can never be located on the same node.  

---
# restore another clusters snapshot to another cluster
When doing that, we first make a repository and then take a snapshot from cluster A with a specific name just to remember it, and then do the same thing with the same name as the cluster A's repo and snapshot on cluster B, and then move the repositories snapshot content somewhere else and then copy the cluster A's snapshot to cluster B's snapshot directory and then restart its cluster. After that you can check the repositories snapshot name with this command:  
```
GET _cat/snapshots/<repo name>
```
and then we might get an error about the snapshot being disabled , we need to recreate the repository with the same name and then we might need to recreate the snapshot name as well, and after that we can see the snapshot contents and we can restore it to our second cluster.  
You can check out these links for more information([how to restore snapshot](https://kifarunix.com/restore-elasticsearch-snapshot-to-another-cluster/) , [why theres an error about the repository](https://www.elastic.co/guide/en/elasticsearch/reference/8.5/add-repository.html))

---
# repository
When registering a repository first we need to config each elasticsearch nodes `path.repo` to a directory and then when creating a repository,  
we can specify a directory inside the path and then take snapshots on that path to be specific.  

---
*‌ You should avoid sending client requests to just one of your nodes. If you do and this node fails, such requests will not receive responses even if the remaining node is a healthy cluster on its own. Ideally, you should balance your client requests across both nodes. A good way to do this is to specify the addresses of both nodes when configuring the client to connect to your cluster. Alternatively, you can use a resilient load balancer to balance client requests across the nodes in your cluster.
