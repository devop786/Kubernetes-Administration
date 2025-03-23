Pod Affinity in Kubernetes
What is Pod Affinity?
Pod Affinity is a feature in Kubernetes that allows you to constrain which nodes your pods can be scheduled on based on the labels of other pods already running on those nodes. This is useful for workloads that need to run on the same node as certain other pods.

Why Use Pod Affinity?
Co-location: When you want certain applications to be co-located on the same node for performance reasons or to reduce network latency between services.

Application dependencies: When your applications need to run near other specific services.

Failover: To ensure related services are on the same machine for high availability during node failures.

Types of Pod Affinity
Required During Scheduling, Ignored During Execution: This is a hard requirement, which means the scheduler will not schedule the pod unless the condition is satisfied.

Preferred During Scheduling, Ignored During Execution: This is a soft requirement, which means the scheduler will try to place the pod in the desired location but will still allow placement elsewhere if necessary.

Syntax of Pod Affinity
Pod Affinity is defined within the affinity field of the pod spec under podAffinity.

affinity:
  podAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            <label-key>: <label-value>
        topologyKey: <topology-key>
labelSelector: Defines the labels of the pods with which we want to co-locate.

topologyKey: Defines the scope in which the affinity is applied, such as kubernetes.io/hostname, which ensures the pods are scheduled on the same node.

Example of Pod Affinity
Let's take a look at an example where we have two applications, login-form and registration, and we want to ensure that the login-form pod is scheduled on the same node as a registration pod.

Example YAML Deployment with Pod Affinity:
apiVersion: apps/v1
kind: Deployment
metadata:
  name: login-form
  namespace: med-dev
  labels:
    app: login-form
spec:
  replicas: 1
  selector:
    matchLabels:
      app: login-form
  template:
    metadata:
      labels:
        app: login-form
    spec:
      affinity:
        podAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchLabels:
                  app: registration  # Ensure the login-form pod is scheduled on the same node as registration pods
              topologyKey: "kubernetes.io/hostname"  # Pods must be scheduled on the same node
      containers:
        - name: registrationform
          image: mohammadaszadali/registrationform:12
          imagePullPolicy: Always
          ports:
            - containerPort: 80
          livenessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10
            failureThreshold: 3
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10
            failureThreshold: 3
Explanation of the Example:
labelSelector:

This field ensures that the login-form pod is scheduled on a node where there is a pod with the label app=registration. It means that login-form pods should run on the same node as registration pods.

topologyKey:

The topologyKey: kubernetes.io/hostname ensures that the pods will be placed on the same node. If you have multiple nodes, the scheduler will try to place them on the same physical or virtual machine.

requiredDuringSchedulingIgnoredDuringExecution:

This is a hard requirement, meaning that the pod will not be scheduled unless the login-form pod can be placed on the same node as a registration pod.

Notes:
requiredDuringSchedulingIgnoredDuringExecution: If the condition is not met, the pod will remain in the Pending state and will not be scheduled.

preferredDuringSchedulingIgnoredDuringExecution: If you want to express a preference (not a strict requirement) for co-locating pods, use this option. It allows Kubernetes to attempt to schedule the pod in the preferred topology, but it won't block the pod from running on another node if necessary.

Conclusion
Pod Affinity is an important tool in Kubernetes for managing where your pods should run in relation to other pods, allowing for better resource utilization, performance, and network latency. By using matchLabels and topologyKey, you can ensure that related services are scheduled on the same node for efficiency.
