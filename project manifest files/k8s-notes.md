# Kubernetes and Docker Quick Reference for Jenkins

## Docker Registry Secret Creation

```bash
kubectl create secret docker-registry regcred \
  --docker-server=[https://index.docker.io/v1/](https://index.docker.io/v1/) \
  --docker-username=YOUR_DOCKER_USERNAME \
  --docker-password=YOUR_DOCKER_PASSWORD \
  --docker-email=YOUR_DOCKER_EMAIL \
  -n YOUR_NAMESPACE
```

## Kubernetes Manifest Application

```bash
kubectl apply -n vq8-jenkins -f configmap.yml
kubectl apply -n vq8-jenkins -f redis-service.yml
kubectl apply -n vq8-jenkins -f redis-statefulset.yml
kubectl apply -n vq8-jenkins -f app-service.yml
kubectl apply -n vq8-jenkins -f app-deployment.yml
kubectl apply -n vq8-jenkins -f ingress.yml
```

## Dockerfile for Jenkins with Docker and kubectl
#### Dockerfile

```bash
FROM jenkins/jenkins:lts-jdk17
RUN apt-get update && \
    apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release && \
    curl -fsSL [https://download.docker.com/linux/debian/gpg](https://download.docker.com/linux/debian/gpg) | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] [https://download.docker.com/linux/debian](https://download.docker.com/linux/debian) <span class="math-inline">\(lsb\_release \-cs\) stable" \| tee /etc/apt/sources\.list\.d/docker\.list \> /dev/null && \\
apt\-get update && \\
apt\-get install</3\> \-y docker\-ce\-cli && \\
curl \-LO "\[https\://dl\.k8s\.io/release/</span>(curl](https://dl.k8s.io/release/$(curl) -L -s [https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl](https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl)" && \
    install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

RUN echo "127.0.0.1 visit-counter" >> /etc/hosts

EXPOSE 8080
EXPOSE 50000
```

```bash
docker build -t jenkins-with-docker .
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 jenkins-with-docker
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

## Ingress Nginx Controller Installation
```bash
kubectl apply -f [https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/cloud/deploy.yaml](https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/cloud/deploy.yaml)
```
