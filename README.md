# itis-k8s-demo-2026

Gli esercizi presenti in questa sezione hanno lo scopo di farti 
familiarizzare con i principali concetti di Kubernetes con 
difficolta' crescente. 

👉 Il consiglio e' di affrontarli in ordine: cerca di comprendere bene
ogni esercizio prima di passare al successivo.

---
## Prima di iniziare

Sul tuo terminale troverai gia' installato Minikube. 

👉 Minikube è uno strumento che crea un cluster Kubernetes locale (composto da un 
solo nodo), ideale per fare pratica e sperimentare.

Per poter iniziare ad usare Minikube, apri un terminale e lancia il seguente comando:
```
minikube start
```
Per verificare che il cluster Kubernetes sia effettivamente in esecuzione:
```
minikube kubectl get nodes
```

👉 `kubectl` è il comando con cui "parli" a Kubernets. Permette di creare, modificare e vedere le 
risorse all'interno di un cluster Kubernetes.

### Suggerimento

Per praticita', Minikube include `kubectl` al suo interno e lo usa per parlare con il cluster.
Per renderne meno verboso il suo utilizzo, definisci all'interno del tuo terminale Windows il
seguente alias:
```
function kubectl {
    minikube kubectl -- $args
}
```
In questo modo potrai utilizzare una versione piu' corta e comoda del comando da terminale,
ad esempio:
```
kubectl get nodes
```



