# Esercizio 6 – Esporre l’applicazione all’esterno

## 🎯 Obiettivo

Accedere all’applicazione direttamente dal tuo computer, senza usare port-forward.

## ❓ Problema

Finora abbiamo usato:
```
kubectl port-forward svc/nginx-service 8080:80
```
Funziona, ma solo finché il comando resta attivo.
Una soluzione realistica e' quella di utilizzare
un Service di tipo `NodePort`:

* apre una porta sul nodo
* permette accesso dall’esterno

---
## Step 1 – Modificare il Service

Modifica il file `service.yaml` creato nell'esercizio 5, cambiando `type: ClusterIP` in `type: NodePort`.
Ecco il nuovo file:
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
  type: NodePort
```

---
## Step 2 – Applicare il nuovo Service
Per rendere effettive le modifiche nel cluster, applica la nuova
configurazione:
```
kubectl apply -f service.yaml
```
---
## Step 3 – Trovare la porta
Controlla la nuova configurazione del servizio:
```
kubectl get svc
```
Vedrai qualcosa tipo:
```
NAME            TYPE        ...   PORT(S)        AGE
nginx-service   NodePort    ...   80:30855/TCP   3h4m
```

👉 `30855` è la porta esterna

---
## Step 4 – Trovare l'IP del nodo
Il cluster Minikube e' composto da un solo nodo. Per trovare 
il suo IP, osserva la risorsa `node`:
```
kubectl get node -o wide
```
Vedrai qualcosa del tipo:
```
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION           CONTAINER-RUNTIME
minikube   Ready    control-plane   23h   v1.35.1   192.168.49.2   <none>        Debian GNU/Linux 12 (bookworm)   6.19.8-100.fc42.x86_64   docker://29.2.1
```

👉 L'IP riportato nella colonna `INTERNAL-IP` è l'IP del nodo.

---
## Step 5 – Accedere al servizio
Per accedere al servizio utilizza il seguente indirizzo all'interno del browser,
utilizzando i dati recuperati negli step precedenti:
```
http://<NODE IP>:<NODEPORT>
```
Esempio:
```
http://192.168.49.2:30855
```

---
## ❓ Cosa è successo?
* Il nodo espone una porta
* Quella porta inoltra al Service
* Il Service inoltra ai Pod

## 💡 Cosa hai imparato
* Un Service può esporre un'applicazione anche all'esterno del cluster.