kubectl get nodes
kubectl label node <nodeName> env=dev
kubectl label node <nodename2> env=dev

nodeSelector: This is the section where you specify the node labels you want to match. In this case:

env: dev This ensures the pod will be scheduled on nodes that have the label disktype=ssd.


Pod Specification: The pod is a simple Nginx container running on the selected node.

Advantages:

we can label to mutiple nodes and we can achieve HA.

Disadvantage:

we cant pass mutiple values to it.
