# Esercizio 7 – ConfigMap: personalizzare l’applicazione

## 🎯 Obiettivo

Modificare il contenuto della pagina web senza cambiare l’immagine Docker.

## ❓ Problema

Finora nginx mostra sempre la sua pagina di default.

Se volessimo cambiarla, dovremmo:

* modificare l’immagine Docker
* ricostruirla
* ridistribuirla

Sarebbe poco pratico, e richiederebbe molto lavoro. La soluzione
consiste nell'utilizzare una ConfigMap, al fine di:

* salvare configurazioni o file
* montarli dentro un container

👉 senza cambiare l’immagine

Creeremo quindi una pagina HTML personalizzata e la faremo usare a nginx.

---
## Step 1 – Creare la ConfigMap
Crea un file chiamato `configmap.yaml` ed inserisci il seguente contenuto:
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-html
data:
  index.html: |
    <html>
      <body>
        <h1>Ciao Kubernetes!</h1>
        <p>Questa pagina arriva da una ConfigMap</p>
      </body>
    </html>
```
---
## Step 2 – Applicare la ConfigMap
Utilizza il solito comando per creare una nuova configmap:
```
kubectl apply -f configmap.yaml
```
Verifica che sia presente con:
```
kubectl get configmap
```
---
## Step 3 – Modificare il Deployment
E' necessario che il deployment consumi la configmap per poter vedere la 
nuova pagina web. Modifica quindi il file `deployment.yaml` nella 
seguente maniera:
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
        - name: html-volume
          mountPath: /usr/share/nginx/html

      volumes:
      - name: html-volume
        configMap:
          name: nginx-html
```

Come abbiamo modificato il deployment?

* E' stato aggiunto un nuovo volume `html-volume`, il cui contenuto deriva dalla configMap `nginx-html`.
* Il volume `html-volume` e' stato montato all'interno del container `nginx` nel path `/usr/share/nginx/html`.
* Le chiavi della ConfigMap vengono trasformate in file all’interno di questa directory.

👉 In particolare, la chiave `index.html` diventa il file `/usr/share/nginx/html/index.html`

---
## Step 4 – Applicare il Deployment aggiornato
Come al solito, per poter rendere effettive le modifiche e' necessario
riapplicare il nuovo deployment:
```
kubectl apply -f deployment.yaml
```

---
## Step 5 – Verificare

Prova adesso ad aggiornare la pagina web nel tuo browser,
precedentemente visitata all'indirizzo:
```
http://<NODE IP>:<NODEPORT>
```

👉 Dovresti vedere la nuova pagina aggiornata

---
## ❓ Cosa è successo?
* nginx legge `index.html`
* quel file ora arriva dalla ConfigMap
* quindi il contenuto è cambiato

👉 senza modificare l’immagine Docker

---
## 💡 Cosa hai imparato
* La ConfigMap permette di modificare configurazione o file
senza ricreare l'immagine del container.