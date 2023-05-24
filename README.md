1  yum localinstall /vagrant/upload/grafana-7.5.15-4.el8.x86_64.rpm 
    2  sudo sed -i -e "s|mirrorlist=|#mirrorlist=|g" /etc/yum.repos.d/CentOS-*
    3  sudo sed -i -e "s|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g" /etc/yum.repos.d/CentOS-*
    4  yum localinstall /vagrant/upload/grafana-7.5.15-4.el8.x86_64.rpm 
    5  systemctl daemon-reload
    6  systemctl start grafana-server
    7  systemctl status grafana-server
    8  ss -tlnp
    9  wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
   10  yum install vget
   11  yum install wget
   12  tar xzfv node_exporter-1.5.0.linux-amd64.tar.gz 
   13  yum install wget vim -y
   14  wget https://github.com/prometheus/prometheus/releases/download/v2.44.0/prometheus-2.44.0.linux-amd64.tar.gz
   15  useradd --no-create-home --shell /bin/false prometheus
   16  mkdir /etc/prometheus
   17  mkdir /var/lib/prometheus
   18  chown prometheus:prometheus /etc/prometheus
   19  chown prometheus:prometheus /var/lib/prometheus
   20   tar -xvzf prometheus-2.44.0.linux-amd64.tar.gz 
   21  mv prometheus-2.44.0.linux-amd64 prometheuspackage
   22  cp prometheuspackage/prometheus /usr/local/bin/
   23  cp prometheuspackage/promtool /usr/local/bin/
   24  chown prometheus:prometheus /usr/local/bin/prometheus
   25   chown prometheus:prometheus /usr/local/bin/promtool
   26  cp -r prometheuspackage/consoles /etc/prometheus
   27  cp -r prometheuspackage/console_libraries /etc/prometheus
   28  chown -R prometheus:prometheus /etc/prometheus/consoles
   29  chown -R prometheus:prometheus /etc/prometheus/console_libraries
   30  vim /etc/prometheus/prometheus.yml
   31  chown prometheus:prometheus /etc/prometheus/prometheus.yml
   32  vim /etc/systemd/system/prometheus.service
   33  systemctl daemon-reload
   34  systemctl start prometheus
   35  systemctl status prometheus
   36   wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
   37  tar xzfv node_exporter-1.5.0.linux-amd64.tar.gz
   38  useradd -rs /bin/false nodeusr
   39  mv node_exporter-1.5.0.linux-amd64/node_exporter /usr/local/bin/
   40  vim /etc/systemd/system/node_exporter.service
   41  systemctl daemon-reload
   42  systemctl start node_exporter
   43  systemctl enable node_exporter
   44  systemctl restart prometheus
   45  systemctl status prometheus
   46  systemctl status node_exporter
   47  :q
   48  systemctl restart node_exporter
   49  systemctl status node_exporter
   50  tail -fn 100 /var/log/messages 
   51  history 
   52  ll /usr/local/bin/node_exporter
   53  ls -la /usr/local/bin/node_exporter 
   54  ls -la /usr/local/bin | grep node_exporter 
   55  useradd -rs /bin/false nodeusr
   56  systemctl daemon-reload
   57  systemctl restart node_exporter
   58  systemctl status node_exporter
   59  tail -fn 100 /var/log/messages 
   60  ls -la /usr/local/bin/node_exporter
   61  chmod 777 /usr/local/bin/node_exporter
   62  systemctl restart node_exporter
   63  systemctl status node_exporter
   64  tail -fn 100 /var/log/messages 
   65  systemctl status node_exporter
   66  systemctl status node_exporter
   67  sed -i /etc/sysconfig/selinux -r -e ‘s/^SELINUX=.*/SELINUX=disabled/g’
   68  sed -i /etc/sysconfig/selinux -r -e ‘s/^SELINUX=.*/SELINUX=disabled/g’
   69  sed -i /etc/sysconfig/selinux -r -e 's/^SELINUX=.*/SELINUX=disabled/g'
   70  systemctl restart node_exporter
   71  systemctl status node_exporter
   72  setenforce 0
   73  reboot
   74  systemctl status node_exporter
   75  systemctl restart node_exporter
   76  systemctl status node_exporter
   77  vi /etc/selinux/config
   78  vi /etc/selinux/config
   79  reboot
   80  systemctl status node_exporter
   81  systemctl status prometheus.service 
   82  systemctl start prometheus.service 
   83  systemctl status prometheus.service 
   84  systemctl enable prometheus.service 
   85  systemctl status graphical.target 
   86  systemctl start graphical.target 
   87  systemctl status graphical.target 
   88  systemctl status grafana-server.service 
   89  systemctl start grafana-server.service 
   90  systemctl status grafana-server.service 
   91  systemctl enable grafana-server.service 
   92  ip ad
