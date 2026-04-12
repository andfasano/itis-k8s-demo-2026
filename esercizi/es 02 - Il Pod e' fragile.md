# Esercizio 2 – Il Pod è fragile

## 🎯 Obiettivo

Capire cosa succede quando un pod viene eliminato.

---
## Step 1 – Eliminare il Pod
```
kubectl delete pod nginx-pod
```
---
## Step 2 – Controllare lo stato
```
kubectl get pods
```

👉 Il Pod non viene ricreato automaticamente dopo la sua distruzione.

## ❓ Perché succede?

Il Pod è una risorsa “semplice”.

Kubernetes:

* lo crea
* lo esegue
* ma **non lo mantiene vivo**

👉 Serve qualcosa di più intelligente → lo vedremo nel prossimo esercizio


