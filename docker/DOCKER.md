# Docker
The story as goes: Practice what your preach.
So, everything should run in a container.... unless

(Don't forget to copy and edit the config files.)

#
# Uninstall
#

for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

#
# Update
#

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update




## RANDOM pAssword
```
openssl rand -base64 32
 
date | md5sum
 
openssl rand -hex 10
```

echo " cgroup_memory=1 cgroup_enable=memory" >> /boot/cmdline.txt

## Docker
```
Edit daemon.json (fix ip adres)
cp daemon.json /etc/docker

systemctl restart docker
```

## CoreDNS
```
mkdir -p /infra/coredns/data

docker run --restart always -d -v /infra/coredns/data:/root -p 53:53 -p 53:53/udp -p 9253:9253 --name coredns docker.io/coredns/coredns -conf /root/Corefile

https://github.com/maxkratz/coredns-dyndns.git

```

## LetsEncrypt

```
SSL CERT

config  data

docker run -it --rm --name certbot -v "/infra/letsencrypt/config:/etc/letsencrypt" -v "/data/letsencrypt/data:/var/lib/letsencrypt" docker.io/certbot/certbot:arm64v8-v1.32.2 certonly --manual --preferred-challenges dns

Please enter the domain name(s) you would like on your certificate (comma and/or
space separated) (Enter 'c' to cancel): *.cwier.nl
Requesting a certificate for *.cwier.nl

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please deploy a DNS TXT record under the name:

_acme-challenge.cwier.nl.

with the following value:

ozlg6wbuNlqfyVh-PL-IlCFWlSnZiQ5xPGLgOPEnGxY

Before continuing, verify the TXT record has been deployed. Depending on the DNS
provider, this may take some time, from a few seconds to multiple minutes. You can
check if it has finished deploying with aid of online tools, such as the Google
Look for one or more bolded line(s) below the line ';ANSWER'. It should show the
value(s) you've just added.

Admin Toolbox: https://toolbox.googleapps.com/apps/dig/#TXT/_acme-challenge.cwier.nl.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Press Enter to Continue

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/cwier.nl/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/cwier.nl/privkey.pem
This certificate expires on 2025-08-15.
These files will be updated when the certificate renews.

NEXT STEPS:
- This certificate will not be renewed automatically. Autorenewal of --manual certificates requires the use of an authentication hook script (--manual-auth-hook) but one was not provided. To renew this certificate, repeat this same certbot command before the certificate's expiry date.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -


root@router:/data/letsencrypt/config/live/cwier.nl#

cat fullchain.pem privkey.pem > k3s_belni_nl.pem
```
```

root@router:/infra/letsencrypt/config/live# tar xvf /root/.kube/cert_key.tar
cert.pem
key.pem
root@router:/infra/letsencrypt/config/live# mv cert.pem key.pem moki6.asuscomm.com/
root@router:/infra/letsencrypt/config/live# ls
belni.nl  cloud.belni.nl  cwier.nl  moki6.asuscomm.com  README
root@router:/infra/letsencrypt/config/live# cd moki6.asuscomm.com/
root@router:/infra/letsencrypt/config/live/moki6.asuscomm.com# ls
cert.pem  key.pem
root@router:/infra/letsencrypt/config/live/moki6.asuscomm.com# cat cert.pem key.pem > moki6.asuscomm.com.pem


```


## NodeJS
```

https://github.com/nodejs/docker-node/blob/main/README.md#how-to-use-this-image

docker run -p 8080:8080 -it --rm --name nodejs -v /infra/nodejs/data:/usr/src/app -w /usr/src/app node:18 npm start

root@router:/data# mkdir -p nodejs/data
root@router:/data# mkdir -p nodejs/config
```

## NGinx
```
docker run -d --restart always --publish 8080:80 --publish 8443:443  --name nginx --volume /infra/nginx:/usr/share/nginx --volume /infra/nginx/log:/var/log/nginx  --volume /infra/nginx/config:/etc/nginx --volume /infra/letsencrypt/config:/etc/letsencrypt docker.io/nginx
```


##ISC KEA (DHCP Server)
```
cd /root/kea/kea-docker

docker login
docker build -t moki38/kea-dhcp4:latest -t moki38/kea-dhcp4:2.6 .

docker push moki38/kea-dhcp4

docker run --restart always --name kea-dhcp4 -d --network host  -v /data/kea/data:/var/lib/kea -v /data/kea/config:/etc/kea moki38/kea-dhcp4

docker exec -it kea-dhcp4 /bin/sh
# kea-admin db-version pgsql -h 192.168.1.40 -P 5432 -n kea_dhcp -u kea -p 3b190d65aeac9d4c94ce
22.1


bash-5.1# kea-admin db-upgrade pgsql -h 192.168.1.40 -n kea_dhcp -u kea -p 3b190d65aeac9d4c94ce
  534  vi Dockerfile
root@ROUTER-A:~/kea# cat Dockerfile
FROM alpine:3.14

RUN apk add --no-cache bash curl sudo

RUN curl -1sLf "https://dl.cloudsmith.io/public/isc/kea-2-6/rsa.72C2476D9C1A0ADE.key" > "/etc/apk/keys/kea-2-6@isc-72C2476D9C1A0ADE.rsa.pub"

RUN apk add --no-cache kea-admin postgresql-client

RUN apk update

RUN /bin/bash



  535  docker build -t kea-admin:2.6.2 .
  536  docker run -ti kea-admin:2.6.2 /bin/bash

    "hosts-database": {
        "type": "postgresql",
        "name": "kea_dhcp",
        "user": "kea",
        "password": "3b190d65aeac9d4c94ce",
        "host": "192.168.90.40",
        "port": 5432



 TFTP

```
root@ROUTER-A:~/infra-cwier# docker run -d --restart always    --name tftpd-hpa     -e TZ="Europe/Amsterdam"     -v /infra/tftp/data:/tftpboot     -p 69:69/udp     3x3cut0r/tftpd-hpa:latest     -c --secure --verbose /tftpboot
aebc3ad15b5208b3147e6ddd2ed9fa2bb438aacf31237c80cf76c6f915635309
```


OpenCert SSL

root@router-cwier:/data/letsencrypt/config/live/cwier.belni.nl# cat fullchain.pem privkey.pem > cwier_belni_nl.pem
root@router-cwier:/data/letsencrypt/config/live/cwier.belni.nl# docker restart haproxy


#
# PostgreSQL
#
```
mkdir -p /infra/postgresql/data
docker run --restart always --name postgresql -d -p 5432:5432 -v /infra/postgresql/data:/var/lib/postgresql/data -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=d29f90eb10309b8577e4 docker.io/postgres:latest
```

#
# PGadmin
#
docker run --restart always --name pgadmin -p 5050:5050 -e 'PGADMIN_DEFAULT_EMAIL=pgadmin@belni.nl' -e 'PGADMIN_DEFAULT_PASSWORD=d29f90eb10309b8577e4' -d docker.io/dpage/pgadmin4

#
# HAproxy
#
docker run -d --restart always --name haproxy -p80:80 -p443:443 -p8404:8404 -p8405:8405 -v "/infra/letsencrypt/config:/etc/letsencrypt/config" -v /infra/haproxy/www:/var/www -v /infra/haproxy/errors:/etc/haproxy/errors -v /infra/haproxy/config:/usr/local/etc/haproxy:ro --sysctl net.ipv4.ip_unprivileged_port_start=0 docker.io/haproxy

#
# CoreDNS
#
docker run -d -v /data/coredns/data:/root -p 53:53/udp --name coredns docker.io/coredns/coredns -conf /root/Corefile

#
# TFTP
#
https://github.com/3x3cut0r/docker/tree/main/tftpd-hpa

docker run -d --restart always    --name tftpd-hpa     -e TZ="Europe/Amsterdam"     -v /data/tftp/data:/tftpboot     -p 69:69/udp     3x3cut0r/tftpd-hpa:latest     -c --secure --verbose /tftpboot
  292  docker logs tftpd-hpa
  294  cd /data/tftp/data/
  298  docker restart tftpd-hpa
  299  docker logs -f  tftpd-hpa

#
# Kea-DHCP4
#

docker stop kea-dhcp4
docker rm kea-dhcp4

#
# Original //github.com/JonasAlfredsson/docker-kea.git
# Only works with Stable isc-kea releases (2.6.2)
#

docker login
docker build moki38/kea-dhcp4:2.6.2 --build-arg KEA_VERSION=2.6.2 .
docker image push moki38/kea-dhcp4:2.6.2

#
# KEA SOURCE
#
./configure --with-openssl --with-mysql --with-pgsql --with-gssapi --enable-static=no


 apt-get update && apt-get install -y \
        apt-transport-https \
        gnupg2 \
    && apt-get install -y \
        build-essential \
        curl \
        libboost-system-dev \
        libkrb5-dev \
        liblog4cplus-dev \
        libmariadb-dev \
        libmariadb-dev-compat \
        libssl-dev=1.1.1* \
        postgresql-server-dev-all

libssl-dev
log4cplus 
apt -y install krb5-config

`docker run -d --restart always --name kea-dhcp4 --network host -v /infra/kea/config:/etc/kea -v /infra/kea/data:/var/lib/kea jonasal/kea-dhcp4:2.6.2-alpine  -c /etc/kea/kea-dhcp4.conf

docker logs kea-dhcp4

docker exec -ti kea-dhcp4 /bin/sh
kea-admin db-init pgsql -h 192.168.90.40 -P 5432 -n kea_dhcp -u kea -p 3b190d65aeac9d4c94ce


docker exec -ti postgresql /bin/bash
su - postgres
psql
CREATE DATABASE kea_dhcp;
CREATE USER kea WITH PASSWORD '3b190d65aeac9d4c94ce';
GRANT ALL PRIVILEGES ON DATABASE kea_dhcp TO kea;
ALTER DATABASE kea_dhcp OWNER TO kea;
\q

docker exec -ti kea-dhcp4 /bin/sh
#
# kea-admin
#

kea-admin db-init pgsql -h 192.168.90.40 -P 5432 -n kea_dhcp -u kea -p 3b190d65aeac9d4c94ce
Schema version reported after initialization: 29.0
# kea-admin db-version pgsql -h 192.168.90.40 -P 5432 -n kea_dhcp -u kea -p 3b190d65aeac9d4c94ce
29.0
# kea-admin db-upgrade pgsql -h 192.168.90.40 -P 5432 -n kea_dhcp -u kea -p 3b190d65aeac9d4c94ce


#
# GITEA
#

```
docker pull docker.gitea.com/gitea:1.23.8
mkdir -p /infra/gitea/data

docker exec -ti postgresql /bin/bash
su - postgres
psql

CREATE DATABASE gitea;
CREATE USER gitea WITH PASSWORD '087f3ebe46e11b03d75f';
GRANT ALL PRIVILEGES ON DATABASE gitea TO gitea;
ALTER DATABASE gitea OWNER TO gitea
\q

docker run -d --restart always --name gitea -p 3000:3000 -p 222:22 -v /infra/gitea/data:/data -v /etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime:ro -e POSTGRES_USER=gitea -e GITEA__database__DB_TYPE=postgres -e GITEA__database__HOST=192.168.1.40:5432 -e GITEA__database__NAME=gitea -e GITEA__database__USER=gitea -e GITEA__database__PASSWD=087f3ebe46e11b03d75f  docker.gitea.com/gitea:1.23.8
```


#
# Registry Mirror
#


talosctl image default

ghcr.io/siderolabs/flannel:v0.26.7
registry.k8s.io/coredns/coredns:v1.12.1
gcr.io/etcd-development/etcd:v3.5.21
registry.k8s.io/kube-apiserver:v1.33.0
registry.k8s.io/kube-controller-manager:v1.33.0
registry.k8s.io/kube-scheduler:v1.33.0
registry.k8s.io/kube-proxy:v1.33.0
ghcr.io/siderolabs/kubelet:v1.33.0
ghcr.io/siderolabs/installer:v1.10.2
registry.k8s.io/pause:3.10


# 
# Confluence
#

git clone --recurse-submodule https://bitbucket.org/atlassian-docker/docker-atlassian-confluence-server.git
cd docker-atlassian-confluence-server
 
docker build -t moki38/confluence:latest -t moki38/confluence:v7.18.1 --build-arg CONFLUENCE_VERSION=7.18.1 .  
 
docker image push moki38/confluence:latest ; docker image push moki38/confluence:v7.18.1
```
#
#MINIO
#


```

git clone https://github.com/minio/minio.git
```
docker run -d --restart always --name minio  -p 9000:9000 -p 9001:9001 -v /s3:/data -e "MINIO_ROOT_USER=admin" -e "MINIO_ROOT_PASSWORD=7df775c0366007d02a5c" docker.io/minio server /data --console-address ":9001"


sudo curl -Lo /usr/local/bin/mc https://dl.min.io/client/mc/release/linux-arm64/mc
sudo chmod +x /usr/local/bin/mc
 
#
# mc autocompletion
#
mc --autocompletion
 
mc alias set local http://192.168.1.40:9000 admin 7df775c0366007d02a5c
mc admin info local

#
# Backup
#
mc mirror /data/confluence/backup/ local/backup
 
#
# Show files
#
mc ls
[2023-03-27 10:06:08 BST] 4.0KiB backup-2023_03_26.zip/
[2023-03-27 10:06:10 BST] 4.0KiB backup-2023_03_27.zip/
 
#
# Restore
#
mc cp local/backup/backup-2023_03_26.zip .



366007d02a5c
mc admin info local
mc: Configuration written to `/root/.mc/config.json`. Please update your access credentials.
mc: Successfully created `/root/.mc/share`.
mc: Initialized share uploads `/root/.mc/share/uploads.json` file.
mc: Initialized share downloads `/root/.mc/share/downloads.json` file.
Added `local` successfully.
●  192.168.1.40:9000
   Uptime: 3 minutes
   Version: 2025-04-22T22:12:26Z
   Network: 1/1 OK
   Drives: 1/1 OK
   Pool: 1

┌──────┬──────────────────────┬─────────────────────┬──────────────┐
│ Pool │ Drives Usage         │ Erasure stripe size │ Erasure sets │
│ 1st  │ 0.0% (total: 46 GiB) │ 1                   │ 1            │
└──────┴──────────────────────┴─────────────────────┴──────────────┘

1 drive online, 0 drives offline, EC:0
root@ROUTER-A:/data# mc mirror /infra local/infra
 0 B / ? ┃░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░▓┃mc: <ERROR> Failed to perform mirroring The specified bucket does not exist
 0 B / ? ┃░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░▓┃ 0sroot@ROUTER-A:/data# mc mirror /infra local/infra
...u-boot.bin: 6.23 GiB / 6.23 GiB ┃▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓┃ 50.75 MiB/s 2m5s



#
# ELASTIC
# https://github.com/elastic/start-local
#

# Logstash

```
mkdir -p /infra/logstash/config
mkdir -p /infra/logstash/data
mkdir -p /infra/logstash/logs
mkdir -p /infra/logstash/pipeline

docker.elastic.co/elasticsearch/elasticsearch:8.1.2/infra/logstash

docker run -d --restart always --name logstash -p 514:5514/udp -p 514:5514 -p 1520:1520 -p 6514:6514 -p 5044:5044 -x LS_OPTS="--config.reload.automatic --config.reload.interval 20" -v /infra/logstash/logs:/usr/share/logstash/logs -v /infra/logstash/data:/usr/share/logstash/data -v /infra/logstash/config:/usr/share/logstash/config -v /infra/logstash/pipeline:/usr/share/logstash/pipeline docker.elastic.co/logstash/logstash:8.1.2 

# Grok debugger
https://grokdebugger.com/

# Elasticsearch

mkdir -p /infra/elasticsearch/config
mkdir -p /infra/elasticsearch/data
mkdir -p /infra/elasticsearch/logs

docker run  -p 9200:9200 -p 9300:9300 --detach  --restart always  --name elasticsearch -v /infra/elasticsearch/data:/usr/share/elasticsearch/data -v /infra/elasticsearch/config:/usr/share/elasticsearch/config -v /infra/elasticsearch/logs:/usr/share/elasticsearch/logs -e ES_JAVA_OPTS="-Xms512m -Xmx512m" --ulimit memlock=-1:-1 docker.elastic.co/elasticsearch/elasticsearch:8.1.2

# Kibana
mkdir -p /infra/kibana/data
mkdir -p /infra/kibana/config
mkdir -p /infra/kibana/logs

docker run --name kibana -p 5601:5601 -d --restart always --hostname kibana.belni.local -v /infra/kibana/data:/usr/share/kibana/data -v /infra/kibana/log/usr/share/kibana/logs -v /infra/kibana/config:/usr/share/kibana/config docker.elastic.co/kibana/kibana:8.1.2

#keystore

podman exec -it  kibana /bin/bash
 
kibana-keystore create
A Kibana keystore already exists. Overwrite? [y/N] Y
Created Kibana keystore in /usr/share/kibana/config/kibana.keystore
 
kibana-keystore add elasticsearch.password
Enter value for elasticsearch.password: ********************   (kibana.system)

# Node Exporter

docker run -d --restart always --name node-exporter --net="host" --pid="host" -v "/:/host:ro,rslave" quay.io/prometheus/node-exporter:latest --path.rootfs=/host
docker run -d --restart always --name node-exporter -p 9100:9100 -v "/:/host:ro,rslave" quay.io/prometheus/node-exporter:latest --path.rootfs=/host

#
# Prometheus
#

docker run --restart always -d -p 9090:9090 --name prometheus -v /infra/prometheus/config:/etc/prometheus -v /infra/prometheus/data:/prometheus -v /infra/alertmanager/config:/etc/alertmanager docker.io/prom/prometheus
#
# Grafana
#
mkdir -p /infra/alertmanager/data
chown -R 472:472 /infra/grafana

docker run -d -p 3000:3000 --name grafana -v /infra/grafana/config:/etc/grafana -v /infra/grafana/data:/var/lib/grafana docker.io/grafana/grafana-oss

Add datasource: Prometheus->http://192.168.90.40:9090

#
# Alertmanager
#

mkdir -p /infra/alertmanager/data

docker run -d -p 9093:9093 --name alertmanager -v /infra/alertmanager/config:/etc/alertmanager -v /infra/alertmanager/data:/alertmanager docker.io/prom/alertmanager

#
# CAdvisor
#

docker run --restart always --volume=/:/rootfs:ro   --volume=/var/run:/var/run:ro   --volume=/sys:/sys:ro   --volume=/var/lib/docker/:/var/lib/docker:ro   --volume=/dev/disk/:/dev/disk:ro  --volume=/etc/machine-id:/etc/machine-id:ro  --publish=8081:8080   --detach=true   --name=cadvisor   --privileged   --device=/dev/kmsg   gcr.io/cadvisor/cadvisor:v0.49.1

wget https://raw.githubusercontent.com/samber/awesome-prometheus-alerts/master/dist/rules/docker-containers/google-cadvisor.yml


#
# Grafana Alloy
#

mkdir -p /infra/alloy/data
mkdir -p /infra/alloy/config

docker run -d --restart always --name alloy -v /infra/alloy/config:/etc/alloy/ -v /infra/alloy/data:/var/lib/alloy -p 12345:12345 -p 51893:51893 -p 51898:51898 grafana/alloy:latest run --server.http.listen-addr=0.0.0.0:12345 --storage.path=/var/lib/alloy/data /etc/alloy/config.alloy

#
# opentelemetry-collector
#

docker run otel/opentelemetry-collector-contrib:0.127.0
docker run -p 4317:4317-v /infra/otel/config.yaml:/etc/otelcol-contrib/config.yaml otel/opentelemetry-collector-contrib:0.127.0

##       scrape_configs:
        - job_name: 'otel-collector'
          scrape_interval: 30s
          static_configs:
            - targets: ['0.0.0.0:8888']

Grafana Dashboard 15983

#
# Loki
#

mkdir -p /infra/loki/data
mkdir -p /infra/loki/config


docker run -d --restart always --name loki -p 3100:3100 -v /infra/loki/config:/etc/loki/ -v /infra/loki/data:/tmp/loki -v /var/run/docker.sock:/var/run/docker.sock grafana/loki:latest
