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

```
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
```

## RUN Command

```
prometheus --config.file=/etc/prometheus/prometheus.yml 
```

## Prometheus Server Systemd Service
``` /etc/systemd/system/prometheus.service ```
```
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
```

## Prometheus Server Systemd Service Commands

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

```
sudo killall -HUP prometheus
```

Checking if Prometheus configuration was changed ?

```
Open the URL : http://localhost:9090/api/v1/status/config
```

## Node Exporter Install Script

```
sudo useradd -M -r -s /bin/false node_exporter
cd /tmp & wget https://github.com/prometheus/node_exporter/releases/download/v1.0.1/node_exporter-1.0.1.linux-amd64.tar.gz
tar xzvf node_exporter-*.tar.gz
sudo rm -rf node_exporter-*.tar.gz
sudo cp node_exporter-*/node_exporter /usr/local/bin/
sudo rm -rf /tmp/node_exporter-*
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

## Node Exporter Systemd Service
``` /etc/systemd/system/node_exporter.service ```
```
[Unit]
Description=Prometheus Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

## Node Exporter Systemd Service Commands

```
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
sudo systemctl status node_exporter
```

## Prometheus Node Exporter WebUI Access URL

http://localhost:9100/metrics
or 
http://server-public-ip:9090/metrics


## Prometheus Data Model

1. Timeseries Data

Prometheus fundamentally stores all data as time series: streams of timestamped values belonging to the same metric and the same set of labeled dimensions. Besides stored time series, Prometheus may generate temporary derived time series as the result of queries.

2. Metrics and Labels

Every time series is uniquely identified by its metric name and optional key-value pairs called labels.
Example:
![Example](https://i.imgur.com/Anh6FXF.jpg)

Where:
**node_cpu_seconds_total** is the name of the metric
Following are the labels which help to query the data:
* cpu="0"
* instance="127.0.0.1:9100"
* job="Linux Server"
* mode="idle"


3. Metric Types

