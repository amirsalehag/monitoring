# Starting the EFK compose  
* First we make its directories:
```
mkdir /data/{certs,eslog}
```
Remember to always set the elastics data directory to user 1000 permission:  
```
sudo chown 1000:1000 /data/eslog
```
* Then run the compose file services
* And after that remember to set the certs directory permission also readable for fluentd user:
```
sudo setfacl -Rm u:999:rwx /data/certs/
```
* If the fluentd image user is set to other users then the uid should be something else.
---
* Beacause the stack has a single node elastic.. remeber to make a index template that sets the indexes created by fluends replica number to 0.
