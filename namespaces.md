# Notes for Namespaces

## Using namespaces to group resources

Instead of hav- ing all your resources in one single namespace, you can split them into multiple name- spaces, which also allows you to use the same resource names multiple times (across different namespaces).

## Discovering other namespaces and their pods

`kubectl get ns`

`kubectl get po --namespace kube-system` | `kubectl get po -n kube-system`

## Creating a namespace

> CREATING A NAMESPACE FROM A YAML FILE

```
apiVersion: v1
kind: Namespace
metadata:
  name: custom-namespace
```

`kubectl create -f custom-namespace.yaml`

> CREATING A NAMESPACE WITH KUBECTL CREATE NAMESPACE

`kubectl create namespace custom-namespace`

## Managing objects in other namespaces

`kubectl create -f kubia-manual.yaml -n custom-namespace`

> To quickly switch to a different namespace, you can set up the following alias: alias kcd='kubectl config set-context \$(kubectl config current- context) --namespace '. You can then switch between namespaces using kcd some-namespace.

## The isolation provided by namespaces

Namespaces allow you to isolate objects into distinct groups, which allows you to operate only on those belonging to the specified namespace, they don’t provide any kind of isolation of running objects.

Whether namespaces provide network isolation depends on which networking solution is deployed with Kubernetes. When the solution doesn’t provide inter-namespace network isolation, if a pod in namespace foo knows the IP address of a pod in namespace bar, there is nothing preventing it from sending traffic, such as HTTP requests, to the other pod.
