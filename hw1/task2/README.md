# Task#2

## Task Requirements
- Create 2 services (backend, frontend). Any framework allowed
- Create 4-5 methods in backend and use it in frontend service. 
- Write deployment manifests in k8s for this services.
- Manifests should describe the  Deployment,  Service,  Ingress entities for both services.   
- Create ingress rule that will forward all requests from (http://sa.homework/):port to frontend service and (http://api.sa.homework/):port to backend service.  
- At the output, provide a link to github with manifests. Manifests must be in the same directory so that all of them can be applied with one command kubectl apply -f.

## How to test?
1. ```$ minikube start --driver=hyperkit```
2. ```$ eval $(minikube docker-env)```
3. ```$ minikube addons enable ingress```
4. ```$ kubectl apply -f .```
5.  Copy Ingress IP address from here  ```$ kubectl get ingress```
6.  Paste the Ingress IP address into /etc/hosts
```IP_ADDRESS sa.homework```
```IP_ADDRESS api.sa.homework```
8.  Open in browser http://sa.homework