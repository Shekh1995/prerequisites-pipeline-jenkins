Set Up Horizontal Pod Autoscaler (HPA) in GKE 🚀

This guide will help you deploy an Nginx Deployment, configure resource limits, and apply HPA (Horizontal Pod Autoscaler) to scale pods based on CPU usage.

✅ Step 1: Create an Nginx Deployment with Resource Limits

Create a file named nginx-deployment.yaml and add the following content:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2  # Initial number of replicas
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
        image: nginx
        resources:
          requests:
            cpu: "250m"
          limits:
            cpu: "500m"
        ports:
        - containerPort: 80


kubectl apply -f nginx-deployment.yaml

Verify the deployment:

kubectl get pods

kubectl describe deployment nginx-deployment

✅ Step 2: Expose Nginx Service (Optional for Testing)

Expose the deployment as a service:

kubectl expose deployment nginx-deployment --type=LoadBalancer --port=80 --name=nginx-service

Verify the service:

kubectl get svc

✅ Step 3: Enable Metrics Server (If Not Already Installed)
GKE usually comes with the Metrics Server pre-installed, but you can install it manually if needed:

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

Verify the metrics server:

kubectl get apiservices | grep metrics

✅ Step 4: Create HPA for Nginx Deployment

Run the following command to create an HPA that scales pods between 2 and 5 replicas based on CPU usage:

kubectl autoscale deployment nginx-deployment --cpu-percent=2 --min=2 --max=5
Verify the HPA configuration:
kubectl get hpa
Expected output:
NAME                REFERENCE                   TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
nginx-deployment   Deployment/nginx-deployment  0%/50%     2         5         2          1m

✅ Step 5: Simulate High CPU Load to Test Scaling

1️⃣ Deploy a Load Generator Pod (to stress the CPU and trigger HPA):
kubectl run -it --rm --image=busybox load-generator -- /bin/sh

2️⃣ Inside the container, run a loop to generate CPU load:

while true; do wget -q -O- http://nginx-service; done

3️⃣ Observe HPA scaling the pods:

kubectl get hpa
kubectl get pods -w


✅ Step 6: Cleanup (Optional)
After testing, clean up your resources:

kubectl delete hpa nginx-deployment
kubectl delete deployment nginx-deployment
kubectl delete svc nginx-service


🎯 Final Output & Result
Initially, there are 2 Nginx pods.
When CPU usage exceeds 50%, HPA scales up the pods dynamically (up to 5).
If CPU load reduces, HPA scales down the pods.
This ensures efficient resource utilization and cost management.
