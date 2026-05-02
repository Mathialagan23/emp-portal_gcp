# 🐳 Kubernetes (Minikube) Quick Start Guide – Windows

## 📌 Prerequisites

* Docker installed & running
* Minikube installed
* kubectl installed

---

## 🚀 1. Start Kubernetes Cluster

```bash
minikube start
```

Check status:

```bash
minikube status
```

---

## 🧭 2. Verify Cluster

```bash
kubectl get nodes
```

Expected: `Ready`

---

## 📦 3. Deploy Your Docker Image

```bash
kubectl create deployment my-app --image=<your-dockerhub-username>/<image>:<tag>
```

Example:

```bash
kubectl create deployment my-app --image=starboy/app:latest
```

---

## 🔍 4. Check Pods

```bash
kubectl get pods
```

Wait until:

```
Running
```

---

## 🌐 5. Expose Deployment (Create Service)

👉 IMPORTANT: Use the correct port (your app’s port)

```bash
kubectl expose deployment my-app --type=NodePort --port=8080
```

Or explicitly:

```bash
kubectl expose deployment my-app --type=NodePort --port=8080 --target-port=8080
```

---

## 🔗 6. Access Your App

```bash
minikube service my-app --url
```

⚠️ Keep terminal open (required on Windows with Docker driver)

---

## 🔁 Alternative (Better for stability)

```bash
kubectl port-forward svc/my-app 8080:8080
```

Open in browser:

```
http://localhost:8080
```

---

## 🧪 7. Test Using Curl

```bash
curl http://127.0.0.1:<port>
```

---

## ⚙️ 8. Set Environment Variables

```bash
kubectl set env deployment/my-app NAME=Starboy
```

---

## 📋 9. Useful Debug Commands

### Get all resources

```bash
kubectl get all
```

### Describe pod

```bash
kubectl describe pod <pod-name>
```

### View logs

```bash
kubectl logs <pod-name>
```

---

## ❌ 10. Delete Resources

### Delete service

```bash
kubectl delete svc my-app
```

### Delete deployment

```bash
kubectl delete deployment my-app
```

---

## 🔄 11. Restart Cluster

```bash
minikube stop
minikube start
```

---

## ⚠️ Common Issues & Fixes

### ❌ ImagePullBackOff

* Wrong image name
* Private repo (needs secret)

---

### ❌ App not accessible

* Wrong port in service
* App not listening on `0.0.0.0`

---

### ❌ Browser error / no response

* Terminal closed (minikube service)
* Use port-forward instead

---

## 🧠 Best Practices

* Always match:

  * Container port
  * Service port
* Use `0.0.0.0` inside containers
* Prefer YAML for real projects

---

## 📄 12. (Optional) YAML Deployment Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: <your-image>
          ports:
            - containerPort: 8080
          env:
            - name: NAME
              value: "Starboy"
---
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - port: 8080
      targetPort: 8080
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

---

## 🎉 Done!

You now know how to:

* Start Kubernetes
* Deploy an image
* Expose it
* Debug issues

---
