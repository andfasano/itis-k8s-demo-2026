# Esercizio 4 – Scaling

## 🎯 Obiettivo

Aumentare e diminuire il numero di Pod.

---

## Step 1 – Aumentare le repliche
Esegui il seguente comando per aumentare il numero di pod:
```
kubectl scale deployment nginx-deployment --replicas=5
```
---
## Step 2 – Verificare
Ricontrolla il numero di pod:
```
kubectl get pods
```

👉 Ora dovresti vedere 5 Pod
---
## Step 3 – Ridurre il numero di pod
```
kubectl scale deployment nginx-deployment --replicas=1
```

---
## 💡 Cosa hai imparato
* Kubernetes gestisce automaticamente il numero di istanze
* Lo scaling è una operazione semplice e immediata