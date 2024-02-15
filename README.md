# Build

mvn clean package

# Native Execution

java -jar .\target\spring-boot-k8s-demo-1.0.0.jar --spring.config.location=file:.\config\dev\


# Docker

docker build -f .\devops\docker\Dockerfile -t spring-boot-k8s-demo .

docker compose -f .\devops\docker\docker-compose.yml up

# minikube

minikube start 

minikube -p minikube docker-env | Invoke-Expression

docker build -f .\devops\docker\Dockerfile -t spring-boot-k8s-demo:latest .

kubectl apply -f .\devops\k8s\configmap.yml

kubectl apply -f .\devops\k8s\deploy.yml 

kubectl apply -f .\devops\k8s\svc.yml

minikube service spring-boot-k8s-demo --url

