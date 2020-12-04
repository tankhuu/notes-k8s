# ReplicaSet

Replacement for ReplicationController

## Comparing a ReplicaSet to a ReplicationController

ReplicaSet has more expressive pod selectors.
Whereas a ReplicationController’s label selector only allows matching pods that include a certain label, a ReplicaSet’s selector also allows matching pods that lack a certain label or pods that include a certain label key, regardless of its value.

ReplicationController can't match label (env=production and env=devel), It can just match (env=production or env=devel).
While ReplicaSet can match, and ReplicaSet can match all pods that include a label with the key env, whatever its actual value is (you can think of it as env=\*).

The ReplicaSet isn’t any different from a ReplicationController. ReplicaSet which has much the same contents as the ReplicationController, The only difference is in the selector.

```
# Less Expressive way
selector:
  matchLabels:
    app: kubia

# Expressive way
selector:
  matchExpressions:
  - key: app
    operator: In
    values:
    - kubia
```

## Commands

`kubectl get rs`

`kubectl describe rs`
