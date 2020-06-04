#### Install pkgs
```
yum install  wget 
wget https://github.com/prometheus/prometheus/releases/download/v2.17.1/prometheus-2.17.1.linux-amd64.tar.gz
wget https://github.com/prometheus/alertmanager/releases/download/v0.20.0/alertmanager-0.20.0.linux-amd64.tar.gz
wget https://github.com/prometheus/node_exporter/releases/download/v1.0.0-rc.0/node_exporter-1.0.0-rc.0.linux-amd64.tar.gz
tar zxvf prometheus-2.17.1.linux-amd64.tar.gz 
tar zxvf node_exporter-1.0.0-rc.0.linux-amd64.tar.gz 
tar zxvf alertmanager-0.20.0.linux-amd64.tar.gz 
mv prometheus-2.17.1.linux-amd64 prometheus
mv node_exporter-1.0.0-rc.0.linux-amd64 node_exporter
mv alertmanager-0.20.0.linux-amd64 alertmanager
```

### Create users
```
systemctl daemon-reload  
useradd -s /sbin/nologin prometheus
mkdir -p /var/lib/prometheus
mkdir -p /var/lib/alertmanager
chown -R prometheus:prometheus /var/lib/prometheus
chown -R prometheus:prometheus /var/lib/alertmanager
chown -R prometheus:prometheus /opt/prometheus/
chown -R prometheus:prometheus /opt/node_exporter/
chown -R prometheus:prometheus /opt/alertmanager/
```

### prometheus.service

vim cat /etc/systemd/system/prometheus.service

```
[Unit]
Description=Prometheus Server
After=network.target

[Service]
Type=simple
PIDFile=/var/run/prometheus.pid
User=prometheus
Group=prometheus
LimitNOFILE=65535
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/opt/prometheus/prometheus \
  --log.level=debug \
  --config.file=/opt/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/data \
  --web.console.templates=/opt/prometheus/consoles \
  --web.console.libraries=/opt/prometheus/console_libraries \
  #for nginx reverce proxy
  --web.external-url=http://localhost:9090/prom \
  --web.route-prefix="/"
  
SyslogIdentifier=prometheus
Restart=always

[Install]
WantedBy=multi-user.target
```

### node_exporter.service
```
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=prometheus
ExecStart=/opt/node_exporter/node_exporter $OPTIONS

[Install]
WantedBy=multi-user.target
```

### alertmanager.service
```
[Unit]
Description=AlertManager
After=network-online.target

[Service]
User=prometheus
Type=simple

ExecStart=/opt/alertmanager/alertmanager \
  --config.file=/opt/alertmanager/alertmanager.yml \
  --storage.path=/var/lib/alertmanager \
  --cluster.listen-address=

[Install]
WantedBy=multi-user.target
```
