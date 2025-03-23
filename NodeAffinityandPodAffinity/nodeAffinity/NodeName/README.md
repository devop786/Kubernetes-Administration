Node Affinity in Kubernetes
Node Affinity is a set of rules used by the Kubernetes scheduler to determine on which node a pod can run based on labels on nodes and conditions specified in the pod specification. It is a way to constrain which nodes your pod can be scheduled on based on labels and conditions defined by the user.

Node Affinity is specified in the pod's affinity section and can be divided into two types:

requiredDuringSchedulingIgnoredDuringExecution: The pod will only be scheduled on nodes that match the affinity rules.

preferredDuringSchedulingIgnoredDuringExecution: The scheduler will try to schedule the pod on a node that matches the affinity rules, but it can still schedule the pod on a different node if the preferred nodes are not available.

nodeName:

In Kubernetes, the nodename is a label associated with the node where a pod is running. You can use nodename as a part of Node Affinity if you want to schedule pods on a specific node.

For example, if you want a pod to be scheduled only on a node with a specific name, you can use nodename in the affinity rule like so:

kubectl get nodes 

NAME               STATUS   ROLES           AGE   VERSION
ip-172-31-32-173   Ready    <none>          32m   v1.29.15
ip-172-31-35-241   Ready    <none>          32m   v1.29.15
ip-172-31-36-100   Ready    <none>          31m   v1.29.15
ip-172-31-43-74    Ready    control-plane   35m   v1.29.15

apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec: 
  nodeName: ip-172-31-32-173 # Based on above output, you can schedule a pod on the specified node.
  containers:
  <you can define your container cinfguration>

Drawbacks:

you cant achieve HA, because if the node goes down pod will be deleted.
it cant accept the multiple values.
Example:
   nodeName: ip-172-31-32-173,<anotherNodeName> # Based on above output, you can schedule a pod on the specified node.
