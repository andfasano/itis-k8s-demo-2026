#  Esercizio 8 – ConfigMap: modificare il comportamento “live”

## 🎯 Obiettivo

Aggiornare la configurazione senza cambiare il Deployment.

## ❓ Problema

Nel precedente esercizio abbiamo creato una ConfigMap per creare una nuova pagina web.
Come possiamo aggiornarla in modo semplice e diretto? 

---
## Step 1 – Modificare la ConfigMap
Cambia il contenuto di `configmap.yaml`:
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-html
data:
  index.html: |
    <html>
      <body>
        <h1>Versione aggiornata!</h1>
        <p>La ConfigMap è stata modificata</p>
      </body>
    </html>
```
---
## Step 2 – Applicare la nuova configmap
```
kubectl apply -f configmap.yaml
```
---
## Step 3 – Controllare il browser

Prova ad aggiornare la pagina fintanto che non compare la versione aggiornata.

👉 La modifica alla ConfigMap non viene propagata immediatamente:
Kubernetes aggiorna i file nel container dopo qualche secondo (fino a circa un minuto).

Puoi anche verificare manualmente l'aggiornamento controllando direttamente il file dentro il Pod:
```
kubectl exec -it <nome pod> -- cat /usr/share/nginx/html/index.html

```
---
## 💡 Cosa hai imparato
* Una ConfigMap può essere usata per fornire file a un container.
* Se la ConfigMap cambia, il contenuto nel container viene aggiornato (non immediatamente).
