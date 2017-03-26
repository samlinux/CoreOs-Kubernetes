# Installation von CoreOs auf virtualbox basierend auf einem ISO-File
Download CoreOs Iso von folgendem Link: https://coreos.com/os/docs/latest/booting-with-iso.html 

Im Setup-Assistenten der virtualbox gibt man folgende Daten an:

### (1) Name 
- name: CoreOs_01
- typ: Linux
- version: Linux 2.6 64 bit

### (2) RAM
- min. 2048 MB (wichtig, da der Installationsprozess im RAM abläuft und bei 1GB zuwenig RAM vorhanden ist!)

### (3) Storage
- min. 8 GB (für unsere Tests sollte dies einmal reichen)
- Festplatte erzeugen, VDI, dynamisch assoziert, alles wie im Assistenten angeben durchgehen

### nach dem booten
CoreOs booted mit dem Benutzer core ohne Password, man ist automatisch an der Console angemeldet.

Wir geben dem Benutzer core ein password, damit wir uns via SSH anmelden könenn.
```
sudo passwd core 
```

Darauf achten, dass die Netzwerkkonfiguration der VM auf Netzwerk-Brücke steht.

IP-Adresse abrufen mit ipfconfig

Mit SSH auf die Maschine verbinden, da wir dann keine Probleme mit dem Tastertur-Layout haben oder Texte ohne Probleme in die VM kopieren können. Alternativ könnte man hier auch eine public ssh-key verwenden.
```
ssh core@192.168.51.144
```

## cloud-config erstellen
Zu diesem Zeitpunkt befindet man sich in der CoreOs live Version. D.h. es ist noch nichts auf die Festplatte installiert. Der Installationsprozess erfolgt nun.

Zuerst erzeugen ein Kennwort für den Benutzer core, welches wir für den Installationsprozess hinterlegen werden.
```
openssl passwd -1
```
Danach erstellen wir ein cloud_config.yml File. Dieses dient als Basis für die Installation. 
core = der Benutzer mit welchem man sich anmelden möchten
passwd = jenes Password was wir für den Benutzer core erstellt haben

Für ein einfaches Beispiel reicht die folgende Konfiguration von cloud_config.yml:

```
$ cat cloud_config.yml
#cloud-config
users:
- name: core
  passwd: $1$uu01fbEV$kvMg70kI.5uBAd8WmMWAX1
  groups:
  - sudo
  - docker
```


## Finale Installation von CoreOs
Die finale Installation erfolgt über folgenden Befehl. Dies kann dann etwas dauern. CoreOs wird auf der sda installiert aus dem stable branche von CoreOs.
```
sudo coreos-install -d /dev/sda -C stable -c cloud_config.yml 
```

Nach der Installation muss man noch die live cd entfernen. Dazu geht man wie folgt vor:

- VM stoppen
- Tab Massenspeicher von der VM
- coreos_Production_iso_image.iso entfernen (Controller IDE)
- VM neu starten 
- mit User core am Server anmelden
- CoreOs als single Host ist nun installiert und einsatzbereit


