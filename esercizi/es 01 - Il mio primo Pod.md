# Esercizio 1 - Il mio primo Pod

## 🎯 Obiettivo

Creare il tuo primo Pod Kubernetes e vedere come eseguire un container all’interno del cluster.

### ❓ Cos’è un Pod?
Un **Pod** è l’unità più piccola in Kubernetes.

Contiene:
* uno o più container
* rete condivisa
* storage condiviso (eventuale)

👉 Per ora useremo **un solo container** (nginx).

### ❓ Cos’è nginx

nginx è un web server, cioè un programma che:

* riceve richieste HTTP (dal browser)
* restituisce pagine web (HTML)

👉 Si pronuncia: _"engine-x"_

Utilizzeremo nginx perché è un modo semplice per avere subito 
un'applicazione funzionante su cui testare Kubernetes.

---
## Step 1 – Creare il file YAML
Crea un file chiamato `pod-nginx.yaml` ed inserisci il seguente contenuto:
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.26
    ports:
    - containerPort: 80
```
---
## Step 2 – Creare il Pod
Applica la configurazione al cluster:
```
kubectl apply -f pod-nginx.yaml
```
---
## Step 3 – Verificare che sia partito
```
kubectl get pods
```

Dovresti vedere qualcosa del tipo:
```
nginx-pod   1/1   Running
```

👉 Se vedi `ContainerCreating`, aspetta qualche secondo.

---
## Step 4 – Ispezionare il Pod
Puoi ottenere più dettagli con:
```
kubectl describe pod nginx-pod
```

👉 Qui puoi vedere:
* immagine usata
* eventi
* stato
---
## Step 5 – Entrare nel container
Ora entriamo dentro il container:
```
kubectl exec -it nginx-pod -- sh
```
Una volta dentro:
```
curl localhost
```

👉 Dovresti vedere la pagina HTML di default di nginx.

Esci con:
```
exit
```
---
## 💡 Cosa hai imparato
* Un Pod esegue container
* Kubernetes può creare risorse da file YAML
* Puoi interagire con i container tramite kubectl exec