# HW3 - Monitoring, Alerting, Prometheus, Grafana, Kubernetes

## Task Requirements
- Supplement the service from the previous task (HW2) with metrics in the Prometheus format using a library for your framework and programming language.
- Make a dashboard in Grafana, in which there would be metrics broken down by API methods:
-- RPS
-- Error Rate - number of 500 responses
- Try to set up alert in grapha for Error Rate.
- Add folowing metrics:
--Pod memory consumption
--Pod CPU consumption
- The output should be: screenshots of the dashboard with charts at the time of stress testing the service. 


## Image source code:
Below is the repository I created for this homework assignment. This image uploaded to https://hub.docker.com/.
- https://github.com/kkboranbay/user-crud-flask


## How to run?
- ```$ minikube start --cpus 4 --memory 8192  --driver=virtualbox```
- ```$ eval $(minikube docker-env)```
- ```$ kubectl create namespace monitoring```
- ```$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts```
- ```$ helm repo update```
- ```$ helm install [RELEASE_NAME] prometheus-community/kube-prometheus-stack -f prometheus.yaml -n monitoring```
- ```$ kubectl port-forward service/prom-kube-prometheus-stack-prometheus 9090```
- ```$ kubectl create namespace app```
- ```$ helm install myapp myhelm -n app -f myhelm/values.yaml```
- ```$ minikube ip```
- ```$ curl minikube_ip:port/metrics```
- ```$ kubectl port-forward deployment/promo-grafana 3000 -n monitoring```

## Metrics

I used *prometheus-client==0.7.1* for Python. 

#### Requests per second (RPS):

Number of successful requests per second. 

```sh
rate(app_request_count_total{exported_endpoint='/config',method="GET"}[30s])
```

#### Errors per second:

Number of failed (non HTTP 200) responses per second.

```sh
sum(rate(app_request_count_total{exported_endpoint='/oops',method="GET",http_status='500'}[30s]))
```

#### CPU usage:

Rate returns a per second value, so multiplying by 100 will give a percentage:

```sh
rate(process_cpu_seconds_total{job="myapp-myhelm"}[30s]) * 100
```
```sh
rate(container_cpu_user_seconds_total{job="myapp-myhelm"}[30s]) * 100
```

#### Memory usage:

```sh
process_resident_memory_bytes{job="myapp-myhelm"}
```


## Alerting
- create Telegram Bot with BotFather
- get chat_id from @userinfobot
- paste this info to channel setting in Grafana

## Make fake multiple requests:
```sh
while true; do ab -n 50 -c 5 http://192.168.99.107:31897/config; sleep 3; done
```

## Screenshots

![Alt text](1.jpg?raw=true "postman")
![Alt text](2.jpg?raw=true "postman")
![Alt text](3.jpg?raw=true "postman")
![Alt text](4.jpg?raw=true "postman")
![Alt text](5.jpg?raw=true "postman")
![Alt text](6.jpg?raw=true "postman")
