Example of Node Affinity Using Multiple Label Values:
If you want to use Node Affinity to schedule a pod on a node that has either env=dev or env=uat, you can use the In operator within the nodeAffinity rules like this:

apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: "env"
            operator: In
            values:
            - "dev"
            - "uat"
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
Explanation:
key: "env": Refers to the label key you want to use.

operator: In: This operator allows you to specify a list of acceptable values.

values: ["dev", "uat"]: The pod will be scheduled on nodes that have the label env with either the value dev or uat.

This way, your pod can be scheduled on nodes that have either env=dev or env=uat labels.

Note:
Kubernetes doesn't support multiple values in a single label (e.g., env=dev,uat), so you'll need to create distinct labels for each value.

You can also use nodeSelector in a similar way, though Node Affinity offers more flexibility and is recommended for more complex scheduling requirements.
