# Preparing 5-None Kubernetes Cluster

Source: https://collabnix.github.io/kubelabs/kube101.html

Tutorial uses Play Labs as underlay - https://labs.play-with-k8s.com/. You can create "VMs" (docker-in-docker instances) for single session which can last up-to 4 hours.
Little bit tricky with copying into web-terminal.

## Prepare clsuter

Use `kubeadm init` from their source as shel executable file.

Then copy `kubeadm join` command on all other existing worker nodes, eg.:

`kubeadm join --token 4f924f.14eb7618a20d2ece 192.168.0.8:6443 --discovery-token-ca-cert-hash  sha256:a5c25aa4573e06a0c11b11df23c8f85c95bae36cbb07d5e7879d9341a3ec67b3
`

You can verify on master node by:

`kubectl get nodes`

### System kube-system pods

It will create system pods that can be verified by:

`kubectl get pods -A`

Or by specifying a namespace:

`kubectl -n kube-system get pods`

## Problems with cluster

There is unidentified problem with network on 2 core-dns pods:

`[node1 kubelabs]$ kubectl get pods -A
NAMESPACE     NAME                            READY   STATUS              RESTARTS      AGE
kube-system   coredns-5d78c9869d-5zc2s        0/1     `ContainerCreating`   0             45m
kube-system   coredns-5d78c9869d-hdq2q        0/1     `ContainerCreating`   0             45m`

Little more details:

`[node1 kubelabs]$ kubectl describe pod coredns-5d78c9869d-5zc2s -n kube-system 
----------Omitted---------

Events:
  Type     Reason                  Age                    From               Message
  ----     ------                  ----                   ----               -------
  Warning  FailedScheduling        48m                    default-scheduler  0/1 nodes are available: 1 node(s) had untolerated taint {node.kubernetes.io/not-ready: }. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling..
  Normal   Scheduled               47m                    default-scheduler  Successfully assigned kube-system/coredns-5d78c9869d-5zc2s to node1
  Warning  FailedCreatePodSandBox  47m                    kubelet            Failed to create pod sandbox: rpc error: code = Unknown desc = failed to setup network for sandbox "7fa2130083825d3ad4a29ad855d149fda68dc172e55d0114248e667e202f0982": plugin type="bridge" name="kubernetes" failed (add): no IP ranges specified
  Normal   SandboxChanged          2m45s (x203 over 47m)  kubelet            Pod sandbox changed, it will be killed and re-created.
`

I do not have a solution yet.
