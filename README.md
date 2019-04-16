# how this project was created
log on to [katacoda k8s course site](https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster) or [https://labs.play-with-k8s.com](https://labs.play-with-k8s.com/)
## install go
- curl -LO https://dl.google.com/go/go1.12.3.linux-amd64.tar.gz
- tar -C /usr/local -xzf go1.12.3.linux-amd64.tar.gz
- export PATH=$PATH:/usr/local/go/bin
- mkdir /root/go
- export GOPATH=/root/go
- export GOBIN=/usr/local/go/bin
- go env GOPATH
- read [How to write GO code](https://golang.org/doc/code.html)

## install dep
- curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh

## install sdk
- mkdir -p $GOPATH/src/github.com/operator-framework
- cd $GOPATH/src/github.com/operator-framework
- git clone https://github.com/operator-framework/operator-sdk
- cd operator-sdk
- git checkout master
- make dep
- make install

## Bootstrapping the Go project
- operator-sdk new podset-operator
- tree -I vendor

## Add a new API for the custom resource PodSet
- cd podset-operator
- operator-sdk add api --api-version=app.example.com/v1alpha1 --kind=PodSet

## Add a new controller that watches for PodSet
- operator-sdk add controller --api-version=app.example.com/v1alpha1 --kind=PodSet

## build the Docker image using the Operator SDK
- operator-sdk build agilesolutions/podset-operator
- docker login -u agilesolutions
- docker push agilesolutions/podset-operator

# to run this stuff
- log on to https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster
- start scenario
- minikube start
- git clone https://github.com/agilesolutions/podset-operator.git
- cd podset-operator

## Setup Service Account
- kubectl create -f deploy/service_account.yaml

## Setup RBAC
- kubectl create -f deploy/role.yaml
- kubectl create -f deploy/role_binding.yaml

## Setup the CRD
- kubectl create -f deploy/crds/app_v1alpha1_podset_crd.yaml

## Deploy the podset-operator
- kubectl create -f deploy/operator.yaml

## check the CRD
- kubectl get crd podsets.app.example.com


## check the operator controller
- kubectl get pods

## check if there's a CR using the CRD fullname...
- kubectl get podsets.app.example.com

## create our PodSet resource configured with 4 replicas
- kubectl apply -f examples/replicas4.yaml
- kubectl get pods -l app=example-podset
- kubectl delete pod xxx
- kubectl get pods -l app=example-podset
- kubectl apply -f examples/replicas2.yaml
- kubectl get pods -l app=example-podset
- kubectl describe podset/example-podset
- kubectl delete podset example-podset
- kubectl  get pods -l app=example-podset  

## more reading on this
[writing operators](https://medium.com/devopslinks/writing-your-first-kubernetes-operator-8f3df4453234)


