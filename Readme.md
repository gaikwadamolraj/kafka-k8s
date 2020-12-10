## Start minikube 
```
minikube start --memory=5384 --cpus=4 --kubernetes-version=v1.18.10
```

## set up helm3 on your machine


## Add chart repo 
```
    helm repo add incubator https://charts.helm.sh/incubator
```

## Create namespace and pv
```
   kubectl apply -f yaml/setup-kafka.yaml
```


## Create kafa cluster using helm
```
helm install  my-kafka --namespace kafka incubator/kafka
```

## Check everything running 
```
kubectl get all -n kafka
```

## Create a test pod to connect kafka producer or broker
```
kubectl apply -f kafka.yaml
```

## Once testclient pod is ready then create topic
```
kubectl -n kafka exec testclient -- ./bin/kafka-topics.sh --zookeeper my-kafka-zookeeper:2181 --topic test1 --create --partitions 1 --replication-factor 1
```



## Start consumer

```
kubectl -n kafka exec -ti testclient -- ./bin/kafka-console-consumer.sh --bootstrap-server my-kafka:9092 --topic test1 --from-beginning
```


## start producer
```
kubectl -n kafka exec -ti testclient -- ./bin/kafka-console-producer.sh --broker-list my-kafka-headless:9092 --topic test1
```

## you can push message in producer 