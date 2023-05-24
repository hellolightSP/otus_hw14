**Домашнее задание**

Настройка мониторинга

**Что нужно сделать?**

Настроить дашборд с 4-мя графиками
память;
процессор;
диск;
сеть.
Настроить на одной из систем:
zabbix (использовать screen (комплексный экран);
prometheus - grafana.

**Выполнение**

Для создания VM использовал vagrant с [Vagrantfile](https://github.com/hellolightSP/otus_hw14/blob/main/Vagrantfile)
Пробросил порты для доступа к prometheus node_exporter и grafana с локального компьютера.
```
config.vm.network "forwarded_port", guest: 9090, host: 9090
config.vm.network "forwarded_port", guest: 9100, host: 9100
config.vm.network "forwarded_port", guest: 9093, host: 9093
config.vm.network "forwarded_port", guest: 3000, host: 3000
```

- Устанавливаем grafana
```
sudo sed -i -e "s|mirrorlist=|#mirrorlist=|g" /etc/yum.repos.d/CentOS-*

sudo sed -i -e "s|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g" /etc/yum.repos.d/CentOS-*

yum localinstall /vagrant/upload/grafana-7.5.15-4.el8.x86_64.rpm 
systemctl daemon-reload
systemctl start grafana-server
systemctl enable grafana-server.service 
```

- Устанавливаем prometheus

```
yum install wget vim -y
wget https://github.com/prometheus/prometheus/releases/download/v2.44.0/prometheus-2.44.0.linux-amd64.tar.gz
useradd --no-create-home --shell /bin/false prometheus
mkdir /etc/prometheus
mkdir /var/lib/prometheus
chown prometheus:prometheus /etc/prometheus
chown prometheus:prometheus /var/lib/prometheus
 tar -xvzf prometheus-2.44.0.linux-amd64.tar.gz 
mv prometheus-2.44.0.linux-amd64 prometheuspackage
cp prometheuspackage/prometheus /usr/local/bin/
cp prometheuspackage/promtool /usr/local/bin/
chown prometheus:prometheus /usr/local/bin/prometheus
 chown prometheus:prometheus /usr/local/bin/promtool
cp -r prometheuspackage/consoles /etc/prometheus
cp -r prometheuspackage/console_libraries /etc/prometheus
chown -R prometheus:prometheus /etc/prometheus/consoles
chown -R prometheus:prometheus /etc/prometheus/console_libraries
```
```
vim /etc/prometheus/prometheus.yml
```
```
global:
  scrape_interval: 10s
scrape_configs:
  - job_name: 'prometheus_master'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'node_exporter_centos'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9100']
```
```
chown prometheus:prometheus /etc/prometheus/prometheus.yml
vim /etc/systemd/system/prometheus.service
```
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
```
systemctl daemon-reload
systemctl start prometheus
systemctl status prometheus
systemctl enable prometheus.service 
```

- Устанавливаем node_exporter

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
tar xzfv node_exporter-1.5.0.linux-amd64.tar.gz
useradd -rs /bin/false nodeusr
mv node_exporter-1.5.0.linux-amd64/node_exporter /usr/local/bin/
```
```
vim /etc/systemd/system/node_exporter.service
```
```
[Unit]
Description=Node Exporter
After=network.target
[Service]
User=nodeusr
Group=nodeusr
Type=simple
ExecStart=/usr/local/bin/node_exporter
[Install]
WantedBy=multi-user.target
```
- В Centos 8 "из коробки" нужно отключить Selinux, или  node_exporter ругается на отсутствие прав и не стартует сервис:

```
systemctl enable node_exporter
sed -i /etc/sysconfig/selinux -r -e 's/^SELINUX=.*/SELINUX=disabled/g'

reboot 
```
- Проверяем работу [node_exporter](https://github.com/hellolightSP/otus_hw14/blob/main/Screenshot%20from%202023-05-24%2017-18-55.png)
- Проверяем работу [prometheus](https://github.com/hellolightSP/otus_hw14/blob/main/Screenshot%20from%202023-05-24%2017-19-29.png)
- Создаем в graphana [data sources](https://github.com/hellolightSP/otus_hw14/blob/main/Screenshot%20from%202023-05-24%2018-52-00.png)
- Создаем дашборд из готовых шаблонов по [ID](https://github.com/hellolightSP/otus_hw14/blob/main/Screenshot%20from%202023-05-24%2018-52-19.png)
- Проверяем работу дашборда в [grafana](https://github.com/hellolightSP/otus_hw14/blob/main/Screenshot%20from%202023-05-24%2017-55-21.png)
