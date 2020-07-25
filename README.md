# Kubernetes Redis Cluster

 Deploy Redis cluster in Kubernetes

![DockerImage-Github](https://github.com/hatamiarash7/Kubernetes-Redis/workflows/DockerImage-Github/badge.svg) ![DockerImage-Dockerhub](https://github.com/hatamiarash7/Kubernetes-Redis/workflows/DockerImage-Dockerhub/badge.svg)

## Install

Redis Cluster requires at least 3 master nodes, so we have 6 replicas here ( 3 master / 3 slave )

```bash
kubectl apply -f redis.yml
kubectl apply -f service.yml
```

## Make cluster

You should scale the cluster up or down manually

### Get cluster nodes

```bash
kubectl get pods -l app=redis-cluster -o jsonpath='{range.items[*]}{.status.podIP}:6379 '
```

Now we are going to create the cluster ( assign master/slave roles, distribute slot maps) using the `redis-trib` script.  

Since we are going create 3 masters cluster with 3 dedicated slaves, the --replicas 1 flag is passed.

```bash
kubectl exec -it redis-cluster-0 -- redis-trib create --replicas 1 <<< node list from previous command >>>
```
