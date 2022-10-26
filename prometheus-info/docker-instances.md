# monitoring docker
* We can monitor the hosts docker engine without any exporters just by these steps.  
first to configure the Docker daemon as a Prometheus target, you need to specify the metrics-address.  
we add this values to `/etc/docker/daemon.json`:  
```
{
  "metrics-addr" : "127.0.0.1:9323",
  "experimental" : true
}
```
and then we restart docker service.  
* after that, we configure the prometheus config file:  
```
- job_name: 'docker'
         # metrics_path defaults to '/metrics'
         # scheme defaults to 'http'.
  static_configs:
     - targets: ['localhost:9323']
```
restart prometheus service.  

---
* we can use the `container exporter` for prometheus which will monitor the aspects of our docker containers.  
