* if the elasticsearch containers are working with security enabled and certificated.. we should give the ca/ca.cert path to  
fluentd too. and specify it in its config file:  
```
      ca_file /fluentd/certs/ca/ca.crt
      user elastic
      password <password>
```
and also give the cert directory permissions for fluentd user (uid = 999).  

# healthcheck
```
curl --fail -s http://localhost:24225 || exit 1
```
but remember that it have to have the curl command installed on the image.  
