Instructions

sudo useradd -m -s /bin/bash admin
sudo passwd admin
sudo usermod -aG wheel admin
su - admin
sudo whoami


DOCKER
dnf update -y
dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
dnf install docker-ce
systemctl enable --now docker
sudo usermod -aG docker $USER && newgrp docker


Kubernetes
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
minikube start --driver=docker
kubectl get pod
kubectl get replicaset
kubectl edit deployment nginx-depl
kubectl exec -it nginx-depl-68c944fcbc-n88sb -- bin/bash

#Explanation:

#kubectl exec → Executes a command inside a running pod.

#-it → Interactive mode (useful for interactive shells).

#nginx-depl-68c944fcbc-n88sb → The pod name where the command runs.

#-- bin/bash → Runs /bin/bash inside the pod.

nano nginx-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
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
          image: nginx
          ports:
            - containerPort: 80

kubectl apply -f nginx-deployment.yaml

nano nginx-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080

kubectl apply -f nginx-service.yaml
kubectl get all



