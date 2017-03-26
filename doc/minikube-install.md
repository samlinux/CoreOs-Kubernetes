# Minikube Installation
Minikube auf dem Mac installieren. Es wird eine virtualbox VM installiert, welche als cluster verwendet wird. Es werden zwei google tools installiert:

- minikube 
- kubectl 

Folgende Schritte in einem Verzeichnis ausführen:

- mkdir minikube 
- cd minikube 
- curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.17.1/minikube-darwin-amd64 && chmod +x minikube 
- minikube start (dauert etwas)
- sudo mv minikube /usr/local/bin
- minikube ip (liefter die IP der virtualbox auf welcher minikube läuft)
- curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl
- chmod +x ./kubectl
- sudo mv ./kubectl /usr/local/bin/kubectl
- Demoprojekt installieren kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080
- kubectl get pod
- minikube dashboard

