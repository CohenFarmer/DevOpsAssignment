# Basic MERN App


A full-stack [MERN](https://www.mongodb.com/mern-stack) application for managing information of employees.

## About the project

This is a full-stack MERN application that manages the basic information of employees.  
The app uses an employee database from the MongoDB Atlas database and then displays it using React.

## Tech Stack
- **Client:** React, Bootstrap  
- **Server:** NodeJS, ExpressJS  
- **Database:** MongoDB  

## Setting up Instance
- Instance should be a t3.medium
- Inbound Security rules are 22, 80, 443, 8081, 8080
- Size is 20 gib (not sure if smaller would work, went large to be safe)

## In Instance
- Install docker, docker compose, minikubem kubectl. Just install the files needed up to lab F
- Git clone https://github.com/CohenFarmer/Group50TaskOne.git
- cd Group50TaskOne
- Then copy and paste steps below, one group at a time

#Start with docker compose: 
# Pull base images
docker pull node:18-alpine
docker pull nginx:1.25-alpine
docker pull mongo:6.0
docker pull mongo-express:1.0.2

# Build images
docker build -t mern-app-backend:latest ./server
docker build -t mern-app-frontend:latest ./client

#generate certs
mkdir reverse-proxy/certs
openssl req -x509 -nodes -days 365 -newkey rsa:2048   -keyout reverse-proxy/certs/key.pem   -out reverse-proxy/certs/cert.pem   -subj "/CN=localhost"

#build
docker compose up -d

#visualise
docker compose ps

#Test -> http://Public_ip/ and http://public_ip/db

docker compose down -v

#RUNNING KUBERNETES
#start minikube
minikube start --driver=docker --cpus=2 --memory=3g

#build images in minikube
minikube image build -t mern-app-backend:latest ./server
minikube image build -t mern-app-frontend:latest ./client

kubectl apply -f mongo-deployment.yml \
              -f backend-deployment.yml \
              -f frontend-deployment.yml \
              -f mongo-express-deployment.yml

kubectl get pods -w

kubectl port-forward svc/frontend 8081:80 --address 0.0.0.0 &
kubectl port-forward svc/mongo-express 8080:8081 --address 0.0.0.0 &

#then access at http://PUBLIC_IP:8081 and http://PUBLIC_IP:8080