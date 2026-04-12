# Esercizio 10 – Aggiornare la versione di nginx

## 🎯 Obiettivo

Aggiornare l’applicazione a una nuova versione e osservare cosa succede.

## ❓ Problema

Finora abbiamo creato un Deployment con una certa versione di nginx:
```
nginx:1.26
```
Ma nella realtà le applicazioni vengono aggiornate continuamente.

👉 Come facciamo ad aggiornare l’applicazione in Kubernetes?

Modificheremo il Deployment per usare una nuova versione dell’immagine.

---
## Step 1 – Aggiornare il Deployment
Modifica il file `deployment.yaml` cambiando `image: nginx:1.26` in  `image: nginx:1.27`.
Incrementa anche il numero di repliche a 5.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 5
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
        image: nginx:1.27
        ports:
        - containerPort: 80
        volumeMounts:
        - name: shared-data
          mountPath: /usr/share/nginx/html

      - name: writer
        image: busybox
        command: ["/bin/sh", "-c"]
        args:
        - |
          i=0
          while true; do
            i=$((i+1))
            echo "<h1>Counter: $i</h1><p>Ora: $(date)</p>" > /data/index.html
            sleep 5
          done
        volumeMounts:
        - name: shared-data
          mountPath: /data

      volumes:
      - name: shared-data
        emptyDir: {}
```
---
## Step 2 – Applicare la nuova versione ed osserva il rollout
Subito dopo aver applicato la nuova configurazione, osserva cosa succede ai pods:
```
kubectl apply -f deployment.yaml
kubectl get pods -w
```
👉 vedrai:
* nuovi Pod creati
* vecchi Pod eliminati

## ❓ Cosa sta succedendo?
Kubernetes sta sostituendo gradualmente i Pod con quelli nuovi.

👉 questo processo si chiama `rollout`

## Step 3 – Verificare
```
kubectl get pods
```
Poi:
```
kubectl describe pod <nome-pod>
```
👉 controlla che vi sia la nuova immagine
---
## Step 4 – Torniamo... indietro (rollback)!
E se la nuova versione non funzionasse?

👉 Possiamo tornare alla versione precedente?

Prova i seguenti comandi:
```
kubectl rollout undo deployment nginx-deployment
kubectl get pods -w
```
👉 vedrai:
* nuovi Pod creati
* quelli con la versione nuova eliminati

Quando aggiorni un Deployment, Kubernetes salva la versione precedente.
Questo permette di tornare indietro in caso di problemi.

---
### 💡 Cosa hai imparato
* Un Deployment permette di aggiornare un’applicazione modificando l’immagine.
* Kubernetes applica automaticamente le modifiche creando nuovi Pod e rimuovendo quelli vecchi (rollout).
* È possibile tornare facilmente alla versione precedente tramite un rollback.
