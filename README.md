# Installation de Prometheus Debian 12

```bash
$ sudo groupadd --system prometheus
$ sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```
```bash
$ wget https://github.com/prometheus/prometheus/releases/download/v3.4.1/prometheus-3.4.1.linux-amd64.tar.gz
$ tar -xvf prometheus-3.4.1.linux-amd64.tar.gz
$ mv prometheus-3.4.1.linux-amd64 /etc/prometheus
$ sudo mkdir /var/lib/prometheus
```
```bash
$ sudo chown prometheus:prometheus /etc/prometheus
$ sudo chown prometheus:prometheus /var/lib/prometheus
$ sudo chown -R prometheus:prometheus /etc/prometheus/consoles
```
```bash
$ sudo cp /etc/prometheus/prometheus /usr/local/bin/
$ sudo nano /etc/systemd/system/prometheus.service
```
```bash
[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
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
--web.console.libraries=/etc/prometheus/console_librairies

[Install]
WantedBy=multi-user.target
```
```bash
$ sudo systemctl daemon-reload
$ sudo systemctl enable --now prometheus
$ sudo systemctl status prometheus
```