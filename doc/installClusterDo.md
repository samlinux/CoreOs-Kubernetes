# Installation CoreOs Cluster mit 3 hosts auf DigitalOcean
- image auswählen: CoreOs (stable)
- size auswählen: 1 GB
- datacenter Region auswählen: Frankfurt
- zusätzliche Optionen auswählen
    + private networking, check
    + IPv6, check
    + User data, check

Für den cluster benötgen wir die Anzahl der Hosts, dies wird dann beim <token> ersetzt z.B.
```
$ curl -w "\n" 'https://discovery.etcd.io/new?size=3'
https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
```

Unter "user data" fügen wir dann die #cloud-config ein, wobei wir vorab das token austauschen:

```
#cloud-config

coreos:
  etcd2:
    # generate a new token for each unique cluster from https://discovery.etcd.io/new?size=3
    # specify the initial size of your cluster with ?size=X
    discovery: https://discovery.etcd.io/<token>
    # multi-region and multi-cloud deployments need to use $public_ipv4
    advertise-client-urls: http://$private_ipv4:2379,http://$private_ipv4:4001
    initial-advertise-peer-urls: http://$private_ipv4:2380
    # listen on both the official ports and the legacy ports
    # legacy ports can be omitted if your application doesn't depend on them
    listen-client-urls: http://0.0.0.0:2379,http://0.0.0.0:4001
    listen-peer-urls: http://$private_ipv4:2380
  units:
    - name: etcd2.service
      command: start
    - name: fleet.service
      command: start
```

- den persönlichen SSH Key bei DigitalOcean hinzufügen
- die Anzahl der Droplets festlegen: 3
- Hostname kontrollieren bzw. neu festlegen
- Button create clicken

Danach werden drei Droplets erzeugt.

