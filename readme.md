# Kubernetes setup

## Usage:

review settings in ansible/host_vars && Vagrantfile

vagrant up

## usefull commands

show etcd member list

``` 
docker exec -i `docker ps -q --filter label=io.kubernetes.container.name=etcd` etcdctl --endpoints=https://172.16.50.11:2379 --ca-file=/etc/kubernetes/ssl/ca.pem member list
```

show kubernetes component statuses
```
docker run --rm -it -v /etc/kubernetes/ssl:/etc/kubernetes/ssl:ro manager1:5000/gcr.io/google_containers/hyperkube-amd64:v1.5.1 /hyperkube kubectl --server https://172.16.50.11 --certificate-authority=/etc/kubernetes/ssl/ca.pem --client-certificate=/etc/kubernetes/ssl/host-manager1.pem --client-key=/etc/kubernetes/ssl/host-manager1-key.pem get componentstatuses
```

kubectl create -f /mount/whatever.yaml
```
docker run --rm -it -v /etc/kubernetes/ssl:/etc/kubernetes/ssl:ro -v ${PWD}:/mount manager1:5000/gcr.io/google_containers/hyperkube-amd64:v1.5.1 /hyperkube kubectl --server https://172.16.50.11 --certificate-authority=/etc/kubernetes/ssl/ca.pem --client-certificate=/etc/kubernetes/ssl/host-manager1.pem --client-key=/etc/kubernetes/ssl/host-manager1-key.pem create -f /mount/canal.yaml
```

## Todo:
* Getting everything back on the secure api ports
* Implement ServiceAccounts for nginx ingress controller
* Add some addons like logging (fluentd, etc)


## usefull stuff

* [https://storage.googleapis.com/kubernetes-release/release/stable.txt](kubernetes version number)
* [https://github.com/kelseyhightower/kubernetes-the-hard-way](Kubernetes the hard way by Kelsey Hightower)
* [https://coreos.com/kubernetes/docs/latest/](coreos install)
* [https://github.com/coreos/etcd/blob/master/Documentation/op-guide/configuration.md](etcd vars)