# Esercizio 9 – Condividere dati tra container

## 🎯 Obiettivo

Capire come più container nello stesso Pod possono collaborare condividendo dati.

## ❓ Problema

Finora il nostro Pod (gestito dal Deployment) ha un solo container `nginx` che 
serve una pagina HTML.

👉 Ma cosa succede se vogliamo che quella pagina venga generata dinamicamente?

## ❓ Idea

Aggiungiamo un secondo container `writer` al deployment, in modo che:

* `writer` scrive il contenuto
* `nginx` lo mostra

Useremo un volume temporaneo di tipo `emptyDir` condiviso tra i container
dello stesso Pod

---
## Step 1 – Modificare il Deployment
Modifica `deployment.yaml` nel modo seguente:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
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
## Step 5 – Applicare le modifiche
```
kubectl apply -f deployment.yaml
```
---
## Step 6 – Verificare nel browser
Apri la pagina nel browser al seguente indirizzo:
```
http://<NODE IP>:<NODEPORT>
```
👉 Ora la pagina mostra l’orario

Aggiorna più volte.

👉 L’orario cambia!

---
## ❓ Cosa sta succedendo?

* il container `writer` scrive continuamente il file `/data/index.html`
* il container `nginx` legge `/usr/share/nginx/html/index.html`

👉 ma è lo stesso file (volume condiviso)

---
## Step 6 – Rimuovere il pod
Adesso prova a cancellare il pod:
```
kubectl delete pod <nome-pod>
```
Appena pronto, ricarica la pagina del browser.

👉 Il contatore si e' azzerato?

---
## ❓ Cosa sta succedendo?

* Lo storage `emptyDir` viene cancellato quando il pod muore.

---
## 💡 Cosa hai imparato
* Un Pod può contenere più container che collaborano tra loro.
* I container possono condividere dati tramite un volume.
* I dati in `emptyDir` sono temporanei e vengono persi quando il Pod viene eliminato.