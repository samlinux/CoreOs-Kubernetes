# Docker - nginx 

## Install nginx docker container
Im Homeverzeichnis des user core /home/core legen wir ein Verzeichnis www an. Darin erstellen wir eine HTML Datei mit einem beliebigen Inhalt.

```
mkdir www
cd www
echo "my first nginx docker container running on CoreOs" > index.html 
```

Da wir in diesem Beispiel noch kein nginx image haben, wird dies beim ersten Aufruf mit installiert.
Als Basis verwenden wir nginx auch Basis von alpine Linux, damit bleint das image sehr klein.
```
docker run -d -p 80:80 -v ~/www/:/usr/share/nginx/html nginx:alpine
```

Vorhandene docker image anzeigen
```
docker images 
```

Aktuell laufende container anzeigen 
```
docker ps
core@localhost ~ $ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
e2febaf73056        nginx:alpine        "nginx -g 'daemon off"   22 minutes ago      Up 22 minutes       0.0.0.0:80->80/tcp, 443/tcp   silly_mcnulty

```

CoreOs legt beim starten auch entsprechende ipTables an, sodass ein Routing auf den Container sofort möglich ist. Da jeder docker container eine eigene IP Adresse hat, kann diese auch in den ipTables abgelesen werden. Eine andere Möglichkeit ist
```
docker inspect e2f
```

ipTables anzeigen mit 
```
iptables -L 

core@localhost ~ $ sudo iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         
DOCKER-ISOLATION  all  --  anywhere             anywhere            
DOCKER     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere            

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         

Chain DOCKER (1 references)
target     prot opt source               destination         
ACCEPT     tcp  --  anywhere             172.17.0.2           tcp dpt:http

Chain DOCKER-ISOLATION (1 references)
target     prot opt source               destination         
RETURN     all  --  anywhere             anywhere  
```
