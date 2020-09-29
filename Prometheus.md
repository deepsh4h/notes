# Prometheus 

## About 

1. Created by Matt T.Proud & Julius Volz at SoundCloud. 
2. Written in Golang. 
3. Open-source
4. More info on prometheus.io

## Use Cases

1. Metric Collection
2. Visualization
3. Alerting

## Prometheus Architecture

1. Prometheus Server - A central server that gathers metrics and makes them available.
2. Exporters - Agents that exposde that that is collected by the Prom server.
3. Alertmanager - Sends alerts triggered by metric data.

Note:- Prometheus works on a Pull model.

## Prometheus Server Installation Shell Script for Ubuntu 16.x,18.x,20.x

````
#!/bin/bash
sudo useradd -M -r -s /bin/false prometheus
sudo mkdir /etc/prometheus /var/lib/prometheus
cd /tmp & wget https://github.com/prometheus/prometheus/releases/download/v2.21.0/prometheus-2.21.0.linux-amd64.tar.gz
tar xzf prometheus-*.tar.gz
rm -rf prometheus-*.tar.gz
sudo cp prometheus-*/{prometheus,promtool} /usr/local/bin/
sudo chown prometheus:prometheus /usr/local/bin/{prometheus,promtool}
sudo cp -r prometheus-*/{consoles,console_libraries} /etc/prometheus/
sudo cp prometheus-*/prometheus.yml /etc/prometheus/prometheus.yml
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
````

## RUN Command

```
prometheus --config.file=/etc/prometheus/prometheus.yml 
```

## Systemd Service Script
```` /etc/systemd/system/prometheus.service ````
````
[Unit]
Description=Prometheus
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
````

## Systemd Service Commands

```
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
sudo systemctl status prometheus
```

## Prometheus Server WebUI Access URL

http://localhost:9090
or 
http://server-public-ip:9090


Reloading the configuration for Prometheus Server without restarting the service/process

````
sudo killall -HUP prometheus
````
