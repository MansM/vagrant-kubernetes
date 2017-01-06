# Kubernetes setup

## Usage:

review settings in ansible/host_vars && Vagrantfile

vagrant up

## usefull commands
 
* docker exec -i `docker ps -q --filter label=io.kubernetes.container.name=etcd` etcdctl --endpoints=https://172.16.50.11:2379 --ca-file=/etc/kubernetes/ssl/ca.pem member list

* docker run --rm -it -v /etc/kubernetes/ssl:/etc/kubernetes/ssl:ro manager1:5000/gcr.io/google_containers/hyperkube-amd64:v1.5.1 /hyperkube kubectl --server https://172.16.50.11 --certificate-authority=/etc/kubernetes/ssl/ca.pem --client-certificate=/etc/kubernetes/ssl/host-manager1.pem --client-key=/etc/kubernetes/ssl/host-manager1-key.pem get componentstatuses

docker run --rm -it -v /etc/certs:/etc/certs:ro -v ${PWD}:/mount manager1:5000/gcr.io/google_containers/hyperkube-amd64:v1.5.1 /hyperkube kubectl --server https://172.16.50.11 --certificate-authority=/etc/certs/ca.pem --client-certificate=/etc/certs/host-manager1.pem --client-key=/etc/certs/host-manager1-key.pem create -f /mount/canal.yaml

## Todo:
* TLS with generated certs on etcd client authentication
* create join to generate: --etcd-servers=http://manager1:2379 (kube-apiserver.yaml)
curl -X PUT -d "value={\"Network\":\"10.2.0.0/16\",\"Backend\":{\"Type\":\"vxlan\"}}" "https://manager1:2379/v2/keys/coreos.com/network/config"


## usefull stuff

* [https://storage.googleapis.com/kubernetes-release/release/stable.txt](kubernetes version number)
* [https://github.com/kelseyhightower/kubernetes-the-hard-way](Kubernetes the hard way by Kelsey Hightower)
* [https://coreos.com/kubernetes/docs/latest/](coreos install)
* [https://github.com/coreos/etcd/blob/master/Documentation/op-guide/configuration.md](etcd vars)