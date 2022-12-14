# *installing prometheus*
* First make a user and group:
```
sudo useradd -M -r -s /bin/false prometheus
```
* Then create the directories needed:
```
sudo mkdir /etc/prometheus /var/lib/prometheus
```
* Download the latest binary version of prometheus from its official github page:
```
sudo apt install wget -f
wget https://github.com/prometheus/prometheus/releases/tag/(latest release) 
```
* Extract the file and copy the binary files in the right path:
```
sudo cp (extracted directory from the zip file)/{prometheus,promtool} /usr/local/bin/
```
* Change the owner and the group of these binaries to the previously created user:
```
sudo chown prometheus:prometheus /usr/local/bin/{prometheus,promtool}
```
* Copy these directories plus the prometheuses config file in the configuration directory:
```
sudo cp -r (extracted directory)/{consoles,console_libraries} /etc/prometheus && \
sudo cp (extracted directory)/prometheus.yml /etc/prometheus/
```
* Set these files owners and groups to prometheus:
```
sudo chown -R prometheus:prometheus /etc/prometheus && \
sudo chown prometheus:prometheus /var/lib/prometheus
```
* After these steps, set the prometheuses configuration to read from etc/prometheus
```
prometheus --config.file=/etc/prometheus/prometheus.yml
```
Then prometheus starts as a service but in foreground , so we press `ctrl+c` to exit.
Its better to run it as a systemd service,so follow these steps:
* First we create the file in systemd directory:
```
sudo vim /etc/systemd/system/prometheus.service
```
* Then paste this content inside of it:
```
[Unit]
Description=Prometheus processing server
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
--config.file /etc/prometheus/prometheus.yml \
--storage.tsdb.path /var/lib/prometheus/ \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```
* You can `daemon-reload` the systemctl but if you dont want the services to be stopped and started again,
then just run and start the service.
```
sudo systemctl start prometheus
```
Its done, you can connect to the prometheus from port `9090`.
