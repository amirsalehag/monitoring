# ELK stack installation
* First we need to download java runtime on our system:  
```
sudo apt install default-jre
```
We can check its version with `java -version` commands.

---
(its better to use them as container but we can also download them via repositories.)  
## Installing Elasticsearch
* Download and install the public signing key:  
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```
* You may need to install the `apt-transport-https` package on Debian before proceeding:  
```
sudo apt-get install apt-transport-https
```
* Save the repository definition to `/etc/apt/sources.list.d/elastic-8.x.list`:  
```
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```
* You can install the Elasticsearch Debian package with:  
```
sudo apt-get update && sudo apt-get install elasticsearch
```
(visit this [page](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/deb.html) for more info.)  

---
## installing Kibana
* Download and install the public signing key:  
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```
* You may need to install the `apt-transport-https` package on Debian before proceeding:  
```
sudo apt-get install apt-transport-https
```
* Save the repository definition to `/etc/apt/sources.list.d/elastic-8.x.list`:  
```
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```
* You can install the Kibana Debian package with:  
```
sudo apt-get update && sudo apt-get install kibana
```
(visit this [page](https://www.elastic.co/guide/en/kibana/8.4/deb.html) for more info.)  

---
## installing logstash
* Download and install the Public Signing Key:  
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
* You may need to install the `apt-transport-https` package on Debian before proceeding:  
```
sudo apt-get install apt-transport-https
```
* Save the repository definition to `/etc/apt/sources.list.d/elastic-8.x.list`:  
```
echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
```
* Run `sudo apt-get update` and the repository is ready for use. You can install it with:  
```
sudo apt-get update && sudo apt-get install logstash
```
---
