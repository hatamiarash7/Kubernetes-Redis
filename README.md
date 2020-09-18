# Kubernetes Redis Cluster

 Deploy Redis cluster in Kubernetes

![DockerImage-Github](https://github.com/hatamiarash7/Kubernetes-Redis/workflows/DockerImage-Github/badge.svg) ![DockerImage-Dockerhub](https://github.com/hatamiarash7/Kubernetes-Redis/workflows/DockerImage-Dockerhub/badge.svg)

## Install

<div dir="rtl">
کاربران فارسی زبان جهت اطلاعات بیشتر به این پست مراجعه کنند :

[راه اندازی کلاستر Redis](https://arash-hatami.ir/redis-cluster-in-kubernetes/)
</div>

Redis Cluster requires at least 3 master nodes, so we have 6 replicas here ( 3 master / 3 slave )

```bash
kubectl apply -f statefulset.yml
kubectl apply -f service.yml
```

## Make cluster

You should scale the cluster up or down manually

### Get cluster nodes

```bash
kubectl get pods -l app=redis-cluster -o jsonpath='{range.items[*]}{.status.podIP}:6379 '
```

Now we are going to create the cluster ( assign master/slave roles, distribute slot maps) using the `redis-cli --cluster create` script.  

Since we are going create 3 masters cluster with 3 dedicated slaves, the `--cluster-replicas 1` flag is passed.

```bash
kubectl exec -it redis-cluster-0 -- redis-cli --cluster create --cluster-replicas 1 <<< node list from previous command >>>
```

Or you can merge command like this :

```bash
kubectl exec -it redis-cluster-0 -- redis-cli --cluster create --cluster-replicas 1 $(kubectl get pods -l app=redis-cluster -o jsonpath='{range.items[*]}{.status.podIP}:6379 ')
```

---

## Support

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/D1D1WGU9)

<div><a href="https://payping.ir/@hatamiarash7"><img src="https://cdn.payping.ir/statics/Payping-logo/Trust/blue.svg" height="128" width="128"></a></div>

## Contributing

1. Fork it !
2. Create your feature branch : `git checkout -b my-new-feature`
3. Commit your changes : `git commit -am 'Add some feature'`
4. Push to the branch : `git push origin my-new-feature`
5. Submit a pull request :D

## Issues

Each project may have many problems. Contributing to the better development of this project by reporting them.
