# Intro

This project is for demonstrating spring boot project which uses external configuration and deployable in different platform. It is built on Windows machine, so you will see powershell commands and usage of windows directory structure in commands and files. 

Configuration file "application.properties" is kept oustide of 'src' folder and maintained as separate files per environment. During runtime, these files are passed with command argument '--spring.config.location' and in container world, the files are mounted as volumes.

## Pre-requisite

1. JDK 21
2. Apache Maven 3.9.6+
3. Docker Desktop
4. Minikube

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

