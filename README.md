# how this project was created
log on to https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster
## install go
- curl -LO https://dl.google.com/go/go1.12.3.linux-amd64.tar.gz
- tar -C /usr/local -xzf go1.12.3.linux-amd64.tar.gz
- export PATH=$PATH:/usr/local/go/bin
- mkdir /root/go
- export GOPATH=/root/go
- export GOBIN=/usr/local/go/bin
- go env GOPATH
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
## more reading on this
[writing operators](https://medium.com/devopslinks/writing-your-first-kubernetes-operator-8f3df4453234)
