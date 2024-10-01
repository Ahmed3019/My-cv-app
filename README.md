
# Flask Web Application Deployment using Docker and Kubernetes

This project demonstrates how to containerize a **Flask** web application using **Docker** and deploy it on a **Kubernetes** cluster. The project follows the DevOps best practices by automating the process of building, pushing the Docker image to a container registry, and deploying it on Kubernetes.

## Project Overview

The project consists of the following steps:
1. **Building the Flask web application**.
2. **Creating a Dockerfile** to containerize the application.
3. **Building and pushing the Docker image** to Docker Hub.
4. **Deploying the Docker image** on a Kubernetes cluster using YAML manifests.
5. **Accessing the Flask app** via a service exposed on the Kubernetes cluster.

## Prerequisites

To deploy this project, you will need:
- **Docker** installed on your local machine.
- **Kubernetes** cluster (Minikube or any cloud provider like GKE, EKS).
- **kubectl** command-line tool installed.
- A **Docker Hub** account (or any other container registry).
  
## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/flask-kubernetes-deployment.git
cd flask-kubernetes-deployment
```

### 2. Build the Docker Image

Use the following command to build the Docker image:

```bash
docker build -t <your-dockerhub-username>/flask-app:latest .
```

### 3. Push the Image to Docker Hub

Once the image is built, push it to your Docker Hub repository:

```bash
docker push <your-dockerhub-username>/flask-app:latest
```

### 4. Deploy to Kubernetes

Apply the Kubernetes deployment and service configuration files:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### 5. Verify the Deployment

Check the status of the Pods and Services:

```bash
kubectl get pods
kubectl get svc
```

Once the service is running, you can access the Flask app through the exposed service IP address.

## Dockerfile

The **Dockerfile** for the Flask web application is structured as follows:

```dockerfile
FROM python:alpine

WORKDIR /usr/src/app

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

## Kubernetes Deployment

The **deployment.yaml** contains the Kubernetes deployment configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-app
        image: <your-dockerhub-username>/flask-app:latest
        ports:
        - containerPort: 5000
```

## Kubernetes Service

The **service.yaml** file defines the service that exposes the Flask application:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-app-service
spec:
  type: NodePort
  selector:
    app: flask-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
      nodePort: 30007
```

## Accessing the Flask Application

Once deployed, access the Flask app by using the external IP address or NodePort:

```bash
minikube service flask-app-service
```

This will open the Flask app in your browser.

## Conclusion

This project provides a step-by-step guide to containerize and deploy a Flask web application on a Kubernetes cluster. It uses Docker to build the container and Kubernetes to manage the deployment, scaling, and service exposure.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
