# Minikube

Since Play With Kubernetes is unstable anymore, it is recommended to use local Minikube or other cluster.
I will use 5-node minikube cluster:

` minikube start --nodes 5 -p collabnix`

I ran into an issue when creating cluster. Fixed by removing all:

` minikube delete --all --purge`

And recreating again with:

` minikube start --nodes 5 -p collabnix`

Then you can check status by:

`minikube status -p collabnix`

Or by kubectl:

`kubectl get nodes`
