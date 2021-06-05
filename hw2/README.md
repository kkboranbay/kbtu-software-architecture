# HW2 (Kubernetes StatefulSet, ConfigMap, Secret, Helm)

## Task Requirements
- Make the simplest RESTful CRUD to create, delete, view and update users (any framework is allowed).
- Add a database for the application (statefulset). 
- Application configuration should be stored in Configmaps. Database accesses should be stored in Secrets.
- Link to the directory in github with kubernetes yaml files.   
- The command to install the database from helm, along with the values.yaml file.  
- Initial migrations command (sql)
- kubectl apply -f command, which runs Kubernetes manifests in the correct order
- Postman collection, which presents examples of requests to the service (create, get, change and delete a user).
- (Optional) Additional points for templating the application in helm for dev, test, prod envs.


## Image source code: 
Below is the repository I created for this homework assignment. This image uploaded to https://hub.docker.com/.
- https://github.com/kkboranbay/user-crud-flask


## How to run without Helm? 
1. ```$ minikube start --driver=virtualbox```
2. ```$ eval $(minikube docker-env)```
3. ```$ kubectl apply -f withoutHelm/postgres.yaml -f withoutHelm/app-config.yaml -f withoutHelm/deployment-conf.yml -f withoutHelm/service.yml ```
4. Run ```minikube service list``` and get IP address of service.
5. And test it in Postman (How to check it, see below)


## How to run with Helm for dev, test, prod envs.
1. ```$ minikube start --driver=virtualbox```
2. ```$ eval $(minikube docker-env)```

### For dev environment: 
1. ```$ kubectl create namespace dev```
2. ```$ kubectl apply -f postgres-dev.yml -n dev```
3. Migration:
    1. ```psql -h {MINIKUBE IP} -p {PORT} -d myapp -U myuser``` By the way: (Password: "passwd")
    2. ```create table clients(id SERIAL PRIMARY KEY, name VARCHAR(255));```
    3. ```insert into clients(name) values('Sultan Dev');```
4. ```$ helm install myapp-dev myhelm -n dev -f myhelm/values-dev.yaml```
5. Run ```minikube service list``` and get IP address of service.
6. And test it in Postman (How do that, you can see below in ScreenShots)


### For prod environment: 
1. ```$ kubectl create namespace prod```
2. ```$ kubectl apply -f postgres-prod.yml -n prod```
3. Migration:
    1. ```psql -h {MINIKUBE IP} -p {PORT} -d myapp -U myuser``` By the way: (Password: "passwd")
    2. ```create table clients(id SERIAL PRIMARY KEY, name VARCHAR(255));```
    3. ```insert into clients(name) values('Sultan Prod');```
4. ```$ helm install myapp-prod myhelm -n prod -f myhelm/values-prod.yaml```
5. Run ```minikube service list``` and get IP address of service.
6. And test it in Postman (How do that, you can see below in ScreenShots)

### For test environment: 
1. ```$ kubectl create namespace test```
2. ```$ kubectl apply -f postgres-test.yml -n test```
3. Migration:
    1. ```psql -h {MINIKUBE IP} -p {PORT} -d myapp -U myuser``` By the way: (Password: "passwd")
    2. ```create table clients(id SERIAL PRIMARY KEY, name VARCHAR(255));```
    3. ```insert into clients(name) values('Sultan Test');```
4. ```$ helm install myapp-test myhelm -n test -f myhelm/values-test.yaml```
5. Run ```minikube service list``` and get IP address of service.
6. And test it in Postman (How do that, you can see below in ScreenShots)

## Endpoints
* **GET */*** - return "Greeting message"
* **GET */config*** - return all env variables
* **GET */all*** - return all users
* **GET */get/{user_id}*** - return user based on id
* **POST */store* params: name** - save new user
* **PUT */update/{user_id}* params: name** - update user
* **DELETE */delete/{user_id}*** - delete user

## How to test in Postman?
1. Import **sa2021-hw-helm.postman_collection.json** file into Postman
2. In request I used **{{ url }}** variable. So you should paste SERVICE IP to this variable. You can see screenshots below

![Alt text](postman_variable.jpeg?raw=true "postman")
![Alt text](postman-var.jpeg?raw=true "postman")
