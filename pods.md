# Notes of PODs

- A container shouldn't run multiple processes.
- A pods shouldn't contain multiple containers if they don't need to run on the same machine.

## DECIDING WHEN TO USE MULTIPLE CONTAINERS IN A POD

when deciding whether to put two containers into a single pod or into two separate pods, you always need to ask yourself the following questions:

- Do they need to be run together or can they run on different hosts?
- Do they represent a single whole or are they independent components?
- Must they be scaled together or individually?

## Pod Descriptor Explaination

- `apiVersion`: Kubernetes API version used in this YAML descriptor
- `kind`: Type of Kubernetes object/resource
- `metadata`: Pod metadata
  - `name`:
  - `labels`:
  - `annotations`:
  - `namespace`:
  - ...
- `spec`: Pod specification/contents
  - `containers`:
    - `image`:
    - `name`:
    - `ports`:
    - `resources`:
    - `volumeMounts`:
  - `volumes`:
    - `name`:
    - `secret`:
- `status`: Detailed status of the current running pod and its containers
  - `conditions`:
  - `containerStatuses`:

## Viewing apllication logs

Containerized applications usually log to the standard output and standard error stream instead of writing their logs to files.
The container runtime (Docker in your case) redirects those streams to files and allows you to get the container’s log by running:

`kubectl logs <pod-name>`

Show logs for container when a pod has many containers running.

`kubectl logs <pod-name> -c <container-name>`

> Container logs are automatically rotated daily and every time the log file reaches 10MB in size. The kubectl logs command only shows the log entries from the last rotation.

## Request to POD

- port forwarding:

`kubectl port-forward kubia-manual 8888:8080`
`curl localhost:8888`

- kubectl expose command to create a service to gain access to the pod externally

## Organizing pods

### Labels

Organizing services into smaller groups based on arbitrary criteria, so every developer and system administrator dealing with your system can eas- ily see which pod is which. And you’ll want to operate on every pod belonging to a cer- tain group with a single action instead of having to perform the action for each pod individually.

Labels are declared in metadata section.

Some suggestions for labels:

- app: ui | as | pc | sc | os
- rel: stabel | beta | canary
- env: prod | stage | dev | debug

- Show Labels Command: `kubectl get po --show-labels` | `kubectl get po -L creation_method,env`
- Modify labels: `kubectl label po kubia-manual creation_method=manual`
- Change existing labels: `kubectl label po kubia-manual-v2 env=debug --overwrite`

### Label Selectors

Label selectors allow you to select a subset of pods tagged with certain labels and perform an operation on those pods.
A label selector is a criterion, which filters resources based on whether they include a certain label with a certain value.
A label selector can select resources based on whether the resource

- Contains (or doesn’t contain) a label with a certain key
- Contains a label with a certain key and value
- Contains a label with a certain key, but with a value not equal to the one you specify

To see all pods you created manually (you labeled them with creation_method=manual)
`kubectl get po -l creation_method=manual`

To list all pods that include the env label
`kubectl get po -l env`

And those that don’t have the env label
`kubectl get po -l '!env'`

Other selectors can be use
`creation_method!=manual`
`env in (prod,devel)`
`env notin (prod,devel)`

Multiple conditions in a label selector
A selector can also include multiple comma-separated criteria. Resources need to match all of them to match the selector.
`app=pc,rel=beta`

> Make sure to use single quotes around !env, so the bash shell doesn’t
> evaluate the exclamation mark.

## Using labels and selectors to constrain pod scheduling

when your hardware infrastructure isn’t homogenous. If part of your worker nodes have spinning hard drives, whereas others have SSDs, you may want to schedule certain pods to one group of nodes and the rest to the other. Another example is when you need to schedule pods perform- ing intensive GPU-based computation only to nodes that provide the required GPU acceleration.

## Using labels for categorizing worker nodes

when the ops team adds a new node to the cluster, they’ll categorize the node by attaching labels specifying the type of hardware the node provides or anything else that may come in handy when scheduling pods.

`kubectl label node gke-kubia-85f6-node-0rrx gpu=true`
`kubectl get nodes -l gpu=true`

## Scheduling pods to specific nodes

```
apiVersion: v1
kind: Pod
metadata:
  name: kubia-gpu
spec:
  nodeSelector:
    gpu: "true"
  containers:
  - image: luksa/kubia
  name: kubia
```

## Annotating pods

A great use of annotations is adding descriptions for each pod or other API object, so that everyone using the cluster can quickly look up information about each individ- ual object. For example, an annotation used to specify the name of the person who created the object can make collaboration between everyone working on the cluster much easier.

Labels should be short, whereas annotations can contain relatively large blobs of data (up to 256 KB in total).

```
kubectl get po kubia-zxzij -o yaml
apiVersion: v1
kind: pod
metadata:
annotations: kubernetes.io/created-by: |
      {"kind":"SerializedReference", "apiVersion":"v1",
"reference":{"kind":"ReplicationController", "namespace":"default", ...
```

`kubectl annotate pod kubia-manual mycompany.com/someannotation="foo bar"`

## Useful commands

Get yaml descriptor of an existing pod

`kubectl get po kubia-xxzij -o yaml`

Discover possible API object fields

`kubectl explain pods`

Explanation of the object and lists the attributes the object can contain

`kubectl explain pod.spec`

Create pod

`kubectl create -f kubia.yaml`

See the full descriptor of the pod

`kubectl get po kubia-manual -o yaml`
