# Installation eines Demo Service im Cluster

## Nützliche Befehle
etcd2 status prüfen
```
 sudo systemctl status -r etcd2
```

Cluster status prüfen 
```
sudo etcdctl cluster-health
```

alle Server anzeigen
```
sudo fleetctl list-machines
```


## Einen Container mit fleetctl starten
(1) Wir erstellen ein Application template
```
vi test-app@.service
[Unit]
Description=test-app%i
After=docker.service

[Service]
TimeoutStartSec=0
ExecStartPre=-/usr/bin/docker kill test-app%i
ExecStartPre=-/usr/bin/docker rm test-app%i
ExecStart=/usr/bin/docker run -e APPNAME=test-app%i --name test-app%i -p 80:80 -v /home/core/www/f1.samlinux.at/:/usr/share/nginx/html nginx:alpine
ExecStop=/usr/bin/docker stop test-app%i
```

(2) Wir sende das Application template zu fleetctl
```
fleetctl submit test-app@.service
```

(3) Wir starten die Application auf allen 3 hosts
```
fleetctl start test-app@1
fleetctl start test-app@2
fleetctl start test-app@3
```

(4) Wir prüfen ob Instanzen die Application ausführen
```
fleetctl list-units

UNIT            MACHINE             ACTIVE  SUB
test-app@1.service  00021ac0.../207.154.220.227 active  running
test-app@2.service  c27fbbf9.../207.154.209.30  active  running
test-app@3.service  ce9dcf07.../207.154.223.176 active  running
```

