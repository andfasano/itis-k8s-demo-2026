# Esercizio 3 – Deployment (self-healing)

## 🎯 Obiettivo

Creare un Deployment che mantiene i Pod attivi automaticamente.

## ❓ Cos’è un Deployment?

Un Deployment:

* crea Pod
* li mantiene in vita
* li ricrea se muoiono
* gestisce scaling

👉 È il modo standard per eseguire applicazioni

---
## Step 1 – Creare il file YAML
Crea un file chiamato `deployment.yaml` ed inserisci il seguente contenuto:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.26
        ports:
        - containerPort: 80
```
---
## Step 2 – Creare il Deployment
```
kubectl apply -f deployment.yaml
```
---
## Step 3 – Verificare
```
kubectl get pods
```
Dal momento che hai configurato il Deployment con `replicas: 2`, dovresti vedere due diversi Pod:
```
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-569f95f5cb-jpmm7   1/1     Running   0          9s
nginx-deployment-569f95f5cb-sjpwb   1/1     Running   0          9s
```

👉 Il nome del Pod viene generato automaticamente a partire dal nome del Deployment 
e contiene un suffisso casuale. 

---
## Step 4 – Test self-healing
Scegli uno qualunque dei pod precedentemente visualizzati, ed utilizza il suo nome
completo per cancellarlo:
```
kubectl delete pod <nome-pod>
```

Subito dopo:
```
kubectl get pods
```

👉 Il Pod viene ricreato automaticamente

---
## 💡 Cosa hai imparato
* Kubernetes lavora per stato desiderato
* Il Deployment garantisce che il numero di Pod sia mantenuto

