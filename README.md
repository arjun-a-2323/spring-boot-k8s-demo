# Spring boot k8s demo

## Intro

This project is for demonstrating spring boot project which uses external configuration and deployable in different platform. It is built on Windows machine, so you will see powershell commands and usage of windows directory structure in commands and files.

Configuration file "application.properties" is kept outside of 'src' folder and maintained as separate files per environment. During runtime, these files are passed with command argument '--spring.config.location' and in container world, the files are mounted as volumes.

This application exposes REST endpoint GET /test is exposed which will return with property value present in application.properties fie which you decided to use.

### Pre-requisite

1. JDK 21
2. Apache Maven 3.9.6+
3. Docker Desktop
4. Minikube

## Build

mvn clean package

## Native Execution

java -jar .\target\spring-boot-k8s-demo-1.0.0.jar --spring.config.location=file:.\config\dev\


## Docker

docker build -f .\devops\docker\Dockerfile -t spring-boot-k8s-demo .

docker compose -f .\devops\docker\docker-compose.yml up

## minikube

NOTE:
1. Run the below commands in Powershell with Administrator privilege.
2. Docker desktop and minikube uses different docker daemon, so you need to build container image again after minikube is started and pointed to minikube's docker daemon.
3. Some issues are observed while running below commands please check issues section to resolve them.

Commands:

1. minikube start

2. minikube -p minikube docker-env | Invoke-Expression

3. docker build -f .\devops\docker\Dockerfile -t spring-boot-k8s-demo:latest .

4. kubectl apply -f .\devops\k8s\configmap-dev.yml

5. kubectl apply -f .\devops\k8s\deploy.yml 

6. kubectl apply -f .\devops\k8s\svc.yml

7. minikube service spring-boot-k8s-demo --url

### Issues Observed and Resolution

#### Container Image download failed within minikube with error - x509: certificate signed by unknown authority

1. minikube delete
2. minikube stop
3. close the powershell terminal
4. Open git bash and run the following command after replacing DNS_NAME with the one present in your error:  "openssl s_client -showcerts -verify 5 -connect <DNS_NAME>:443"
5. Copy each certificate contents from "-----BEGIN CERTIFICATE-----" till "-----END CERTIFICATE-----" in a separate file with extension of ".crt" under the location "USER_HOME\.minikube\certs"
6. Open a new powershell with Administrator privilege.
7. Repeat all the steps mentioned in commands section above.

#### error starting ssh tunnel: exec: "ssh": executable file not found in %PATH%

1. minikube stop
2. close the powershell terminal
3. This error because the ssh program not present under path env variable, so use the git provided ssh program which is present under "USER_HOME\AppData\Local\Programs\Git\usr\bin" and add it to system or user path env variable.
4. Repeat 1 and 2 commands from commands section and then run 7th command from above as this error is related to it. Other commands (3 to 6) will be already reflected from previous run within minikube k8s cluster.
