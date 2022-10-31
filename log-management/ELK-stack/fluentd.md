# What is fluentd
* Fluentd is an open source log collector, processor, and aggregator.Its a one-stop component that can aggregate data  
from multiple sources, unify the differently formatted data into JSON objects ( and route it to different output destinations.  
* A vanilla Fluentd deployment will run on ~40MB of memory and is capable of processing above 10,000 events per second.  
Adding new inputs or outputs is relatively simple and has little effect on performance. Fluentd uses disk or memory for buffering  
and queuing to handle transmission failures or data overload and supports multiple configuration options to ensure a more resilient data pipeline.  
* The Fluentd Docker image includes tags `debian`,`armhf` for ARM base images, `onbuild` to build, and `edge` for testing.  
---
# What is fluentbit
* Fluent Bit is an open source log collector and processor and it was created with a specific use case in mind;  
highly distributed environments where limited capacity and reduced overhead (memory and CPU) are a huge consideration.  
* Fluent Bit is also extensible, but has a smaller eco-system compared to Fluentd.You can see the bellow table for more information:  
![image](https://logz.io/wp-content/uploads/2018/06/compare-chart.png)  
### performance
To see the difference in performance, take a look at the recommended default specs for running the two tools in Kubernetes.  
You can do the math yourselves:  

Fluentd:
```
resources:
  limits:
    memory: 500Mi
  requests:
    cpu: 100m
    memory: 200Mi
```
Fluent Bit:
```
resources:
  requests:
    cpu: 5m
    memory: 10Mi
  limits:
    cpu: 50m
    memory: 60Mi
```
### aggregation
Fluent Bit acts as a collector and forwarder and was designed with performance in mind, as described above. Fluentd was designed  
to handle heavy throughput — aggregating from multiple inputs, processing data and routing to different outputs.  
Fluent Bit is not as pluggable and flexible as Fluentd, which can be integrated with a much larger amount of input and output sources.  
### using fluentd and fluent bit
We can use both at the same time, in Kubernetes for example, Fluent Bit would be deployed per node as a daemonset, collecting and  
forwarding data to a Fluentd instance deployed per cluster and acting as an aggregator — processing the data and routing it to different  
sources based on tags:  
![image ](https://dytvr9ot2sszz.cloudfront.net/wp-content/uploads/2018/06/kuberbetes-monitoring-arch-1.jpg)  
Fluent Bit can be used on it own of course but has far less to offer in terms of aggregation capabilities and with a much smaller amount  
of plugins for integrating with other solutions.  

---
* Fluentd has 9 types of plugins; `Input`,`Parser`,`Filter`,`Output`,`Formatter`,`Storage`,`Service Discovery`,`Buffer`,`Metrics`.  
* The configuration file is the fundamental piece to connect all things together, as it allows to define which Inputs or listeners Fluentd  
will have and set up common matching rules to route the Event data to a specific Output.  
Its also important to put the config sections in the right order that we want. Each section will be considered before proceeding.  
