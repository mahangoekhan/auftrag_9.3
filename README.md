
# Kubernetes Cheatsheet (oc, Docker, Git & Deployments)

## 🔧 OpenShift (oc) Befehle

```bash
# Login
oc login https://api.<cluster> --token=<your-token>

# Aktuelles Projekt anzeigen
oc project

# Alle Projekte anzeigen
oc get projects

# Pods anzeigen
oc get pods

# Logs eines Pods
oc logs pod/<pod-name>

# Deployment neu starten
oc rollout restart deployment/<deployment-name>

# Pods nach Label löschen
oc delete pod -l app=<app-label>

# Secret anzeigen (decoded)
oc get secret <secret-name> -o yaml | yq '.data' | base64 -d
```

---

## 🐳 Docker

```bash
# Image bauen
docker build -t <image-name>:<tag> .

# Image taggen für GitHub oder OpenShift Registry
docker tag <image-name>:<tag> ghcr.io/<user>/<repo>:<tag>

# In Registry einloggen (GitHub / Appuio)
echo <TOKEN> | docker login ghcr.io -u <USERNAME> --password-stdin
oc whoami -t | docker login -u $(oc whoami) --password-stdin registry.$(oc whoami --show-server | cut -d "." -f 2).appuio.cloud

# Image pushen
docker push ghcr.io/<user>/<repo>:<tag>
```

---

## 🧬 Git

```bash
# Projekt klonen
git clone <repo-url>

# Änderungen zum Commit vormerken
git add .

# Commit erstellen
git commit -m "Beschreibung"

# Pushen auf Remote
git push origin main
```

---

## 📦 Kubernetes Deployment YAML Beispiel

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: petclinic-app-mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: petclinic-app-mysql
  template:
    metadata:
      labels:
        app: petclinic-app-mysql
    spec:
      containers:
        - name: mysql
          image: mysql:8.0
          ports:
            - containerPort: 3306
          envFrom:
            - secretRef:
                name: petclinic-app-mysql
          volumeMounts:
            - name: mysql-pv
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-pv
          persistentVolumeClaim:
            claimName: petclinic-app-mysql
```

---

## 📁 Nützliche Pfade

- `~/.kube/config`: kubeconfig Datei
- `~/.docker/config.json`: Docker Login-Daten

---

## 📚 Weitere Tipps

- Nutze `oc explain <resource>` für Hilfe zu YAML Feldern.
- Nutze `kubectl` für Kubernetes ohne OpenShift.

---

Happy Deploying 🚀
