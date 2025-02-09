# Kubernetes 101

Source: https://minikube.sigs.k8s.io/docs/tutorials/kubernetes_101/

### Kubectl proxy

Useful command to expose kube API to localhost or outside network. Handy for troubleshooting etc...

``kubectl proxy``

And then curl to API can look something like this:

``curl http://localhost:8001/version``

Or specifically for pod:

``curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME``

### Exec commands on POD

From kubectl utility directly:

``kubectl exec $POD_NAME -- env``

If I want access directly to the pods shell:

``kubectl exec -ti $POD_NAME -- bash``

Breakdown of `-ti`:

  *  -t → Allocates a pseudo-TTY (terminal). This is needed for interactive programs.
  *  -i → Keeps stdin open, allowing user input.

Another way is to send oneliner as shell:

``kubectl exec $POD_NAME -- sh -c 'export TEST=foo && echo $TEST'``


### Label directly from kubectl

I can filter labels in get command like this:

``kubectl get pods -l app=kubernetes-bootcamp``

Add a new label next to existing one:

``kubectl label pods $POD_NAME version=v1``

I can also delete instance based on label:

``kubectl delete service -l app=kubernetes-bootcamp``

### Scale to more instances

Just add pods to development

``kubectl scale deployments/kubernetes-bootcamp --replicas=4``

### Wide display 

``kubectl get pods -o wide``

### All nemaspaces

If I want to display from every namespace:

``kubectl get pods -A``

### Directly upgrade image of an application/pods

I can make instat upgrade by setting a new image:

``kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=gcr.io/k8s-minikube/kubernetes-bootcamp:v2``

I can also check the deploy status:

``kubectl rollout status deployments/kubernetes-bootcamp``

Or to do a rollback:

``kubectl rollout undo deployments/kubernetes-bootcamp``

