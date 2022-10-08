# monitoring nginx vts metrics
* For using nginx vts status metrics on prometheus and even grafana, we just need to configure  
prometheuses config file (`/etc/prometheus/prometheus.yml`):
```
- job_name: "nginx-vts"
    metrics_path: /status/format/prometheus
    static_configs:
      - targets: ["localhost:9999"]
```
(port number is dependent on the open port specified on nginx's configuration file)
(the `metrics_path` also depends on what directive we set for vts module in nginx config file)
* And then we restart prometheus service.
* We then can create dashboards in grafana with its metrics on prometheus.
