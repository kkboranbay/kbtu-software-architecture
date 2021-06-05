# Task#1

## Task Requirements
Create a minimal service: 
- that answers on port 8000
- has http method GET /health RESPONSE: {"status": "OK"}  
- Build a docker image of the application locally. Push the image to dockerhub. 
- Write deployment manifests in k8s for this service.  
- Manifests should describe the  Deployment, Service. 
- Liveness, Readiness should be specified in deployment.
- The number of replicas must be at least 2.
- The container image must be listed from Dockerhub.  
- Ingress host should be sa.homework.
- As a result, after applying the GET manifests, the request to http: //api.sa.homework:port/health should return {“status”: “OK”}.  
- At the output, provide a link to github with manifests. Manifests must be in the same directory so that all of them can be applied with one command kubectl apply -f .
- url, by which you can get a response from the service (or a test in postman).  
- The Ingress must have a rule that will forward all requests from / kbtu / {student name} / * to the service with a rewrite path. Where {student name} is the name of the student.

## Endpoints:
1. http://api.sa.homework/health
2. http://api.sa.homework/kbtu/student/{student_name}

## How to test?
### With Ingress
1. ```$ minikube start --driver=hyperkit```
2. ```$ eval $(minikube docker-env)```
3. ```$ minikube addons enable ingress```
4. ```$ kubectl apply -f .```
5.  Copy Ingress IP address from here  ```$ kubectl get ingress```
6.  Paste the Ingress IP address into /etc/hosts
```IP_ADDRESS api.sa.homework```
7.  Open in browser http://api.sa.homework/health
8.  For test **rewrite path** open in browser http://api.sa.homework/kbtu/student/{student name}. You should see "Student name is {student name}"

### Without Ingress
1. ```$ minikube start --driver=hyperkit```
2. ```$ eval $(minikube docker-env)```
3. ```$ kubectl apply -f deployment.yml -f service.yml```
4. ```$ minikube ssh```
5. ```$ curl http://SERVICE-IP:SERVICE-PORT/health```
6. Or you can test it from a browser outside of minikube: 
```$ kubectl port-forward service/flask-service 8000:8000```