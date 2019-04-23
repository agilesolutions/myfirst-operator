# about Centos k8s controllers
CRDs are only a mean to specify a configuration, though. The cluster still needs controllers to monitor its state and reconcile the resource 
to match with the configuration (make consistent with another).Operators are controllers working in association with custom resources to perform tasks that generally… “human operators” have to take care of. Operators are the CRDs associated with controllers which observe and act upon changes in the configuration or changes in the state of the cluster
The etcd operator manages etcd clusters deployed to Kubernetes and automates tasks related to operating an etcd cluster.


## how this project was created
log on to [katacoda k8s course site](https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster)
## install go
- curl -LO https://dl.google.com/go/go1.12.3.linux-amd64.tar.gz
- tar -C /usr/local -xzf go1.12.3.linux-amd64.tar.gz
- export PATH=$PATH:/usr/local/go/bin
- mkdir /root/go
- export GOPATH=/root/go
- export GOBIN=/usr/local/go/bin
- export PATH=$PATH:$(go env GOPATH)/bin
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
- git config --global user.email robert.rong@agile-solutions.ch
- git config --global user.name agilesolutions
- mkdir -p $GOPATH/src/github.com/agilesolutions/
- cd $GOPATH/src/github.com/agilesolutions/
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
- mkdir -p $GOPATH/src/github.com/agilesolutions
- cd $GOPATH/src/github.com/agilesolutions
- git clone https://github.com/agilesolutions/podset-operator.git
- cd podset-operator
- go install & build

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
- kubectl delete pods -l app=example-podset
- kubectl get pods -l app=example-podset
- kubectl apply -f examples/replicas2.yaml
- kubectl get pods -l app=example-podset
- kubectl describe podset/example-podset
- kubectl delete podset example-podset
- kubectl  get pods -l app=example-podset  

## more reading on this
[writing operators](https://medium.com/devopslinks/writing-your-first-kubernetes-operator-8f3df4453234)


