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
