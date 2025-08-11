# Kubernetes Minikube Deployment

## Objective
Deploy and manage a simple application on a Kubernetes cluster running locally on Minikube.

## Tools Used
- **Minikube** (local Kubernetes cluster)
- **kubectl** (Kubernetes CLI)
- **Docker** (container runtime)

---
## Steps Performed

### 1. Install and Start Minikube

minikube start --driver=docker


### 2. Create Deployment

**deployment.yaml**

yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
  labels:
    app: my-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app-container
          image: nginx:latest
          ports:
            - containerPort: 80


Apply the deployment:


kubectl apply -f deployment.yaml


### 3. Create Service

**service.yaml**

yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  type: NodePort
  ports:
    - port: 80
      targetPort: 80


Apply the service:

kubectl apply -f service.yaml


---

### 4. Verify Deployment

kubectl get pods
kubectl get svc

---

### 5. Access the Application from Browser (EC2 Environment)

Since Minikube is running inside an EC2 instance, direct `minikube service` access is not possible.
We used **port-forwarding** to expose the service:


kubectl port-forward svc/my-app-service 8080:80 --address=0.0.0.0

**AWS Security Group Rule:**

* Add inbound rule to allow TCP traffic on port `8080` from your IP or `0.0.0.0/0`.

**Browser Access:**

http://<EC2-public-ip>:8080

---

### 6. Scale the Deployment

kubectl scale deployment my-app-deployment --replicas=4
kubectl get pods

---

### 7. Describe Pods and View Logs

kubectl describe pod <pod-name>
kubectl logs <pod-name>


---

## Deliverables

* `deployment.yaml`
* `service.yaml`
* Screenshots:

  1. Output of `kubectl get pods` and `kubectl get svc`
  2. Application in browser via port-forward
  3. Scaled pods after `kubectl scale deployment <deployment-name> --replicas=<number>`
  4. Output of `kubectl describe pod`


