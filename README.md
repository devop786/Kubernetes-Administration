Here's the content formatted as a `README.md` file based on your provided information:

```markdown
# Kubernetes Role-Based Access Control (RBAC)

## Overview

Role-Based Access Control (RBAC) in Kubernetes helps define who can access specific resources and what actions they can perform on those resources. RBAC enables fine-grained access control and allows administrators to assign permissions based on the needs of users, service accounts, and groups within a Kubernetes cluster.

## Key Concepts

### 1. **Role**

A **Role** in Kubernetes is a set of permissions within a specific namespace. It defines what actions (verbs) can be performed on which resources within that namespace.

#### Key Points:
- A Role is **namespace-scoped**, meaning it applies only within the namespace where it is created.
- It specifies what actions a user or service account can perform on resources such as Pods, Services, Deployments, etc.
- Actions (verbs) that can be specified include `get`, `list`, `create`, `delete`, `update`, `patch`, and `watch`.

#### Example of a Role:
Here’s an example of a Role that allows a user to perform certain actions on Pods and Services within a namespace (`namespace-1`).

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-service-role
  namespace: namespace-1
rules:
- apiGroups: [""]
  resources: ["pods", "services"]
  verbs: ["get", "list", "create"]
```

- **Role Name:** `pod-service-role`
- **Namespace:** `namespace-1`
- **Permissions:** `get`, `list`, and `create` Pods and Services within `namespace-1`.

---

### 2. **RoleBinding**

A **RoleBinding** grants the permissions defined in a Role to a user, service account, or group within a specific namespace.

#### Key Points:
- A RoleBinding is **namespace-scoped** and applies only to the namespace where it is created.
- It connects a **user**, **service account**, or **group** to a Role within the same namespace.

#### Example of a RoleBinding:
Here’s an example of a RoleBinding that binds the `pod-service-role` to a user `azzu` within the `namespace-1` namespace.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: azzu-role-binding
  namespace: namespace-1
subjects:
- kind: User
  name: azzu
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-service-role
  apiGroup: rbac.authorization.k8s.io
```

- **RoleBinding Name:** `azzu-role-binding`
- **Namespace:** `namespace-1`
- **Subjects:** The `User` named `azzu` is bound to the `pod-service-role` Role.
- **RoleReference:** The Role being referenced is `pod-service-role`.

---

## Key Differences Between Role and RoleBinding

| Aspect                  | Role                              | RoleBinding                          |
|-------------------------|-----------------------------------|--------------------------------------|
| **Scope**               | Namespace-scoped                 | Namespace-scoped                    |
| **Purpose**             | Defines permissions on resources | Grants those permissions to a user/group/service account |
| **Usage**               | Assign permissions within a namespace | Bind a user, group, or service account to a Role within a namespace |

---

## How They Work Together

- **Role:** Defines the set of permissions (verbs, resources) for a user or service account on resources in a namespace.
- **RoleBinding:** Binds a specific user, service account, or group to a Role. Without a RoleBinding, a Role has no effect because it doesn't apply to any subjects.

You can also bind multiple subjects (users, groups, service accounts) to a single Role using a RoleBinding.

---

## Example Scenario

### Requirement:
Imagine a user, `azzu`, needs read-only access to Pods in a specific namespace (`namespace-1`).

### Steps:
1. **Create a Role** in `namespace-1` that allows `get` and `list` actions on Pods:
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: Role
   metadata:
     name: read-only-pod-role
     namespace: namespace-1
   rules:
   - apiGroups: [""]
     resources: ["pods"]
     verbs: ["get", "list"]
   ```
2. **Create a RoleBinding** that binds this Role to the user `azzu`:
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: RoleBinding
   metadata:
     name: azzu-read-only-role-binding
     namespace: namespace-1
   subjects:
   - kind: User
     name: azzu
     apiGroup: rbac.authorization.k8s.io
   roleRef:
     kind: Role
     name: read-only-pod-role
     apiGroup: rbac.authorization.k8s.io
   ```

### Outcome:
- The user `azzu` is granted read-only access to Pods within `namespace-1`.

---

## Conclusion

In Kubernetes, **Roles** and **RoleBindings** are essential tools for controlling access to cluster resources in a granular way. By defining roles with specific permissions and binding them to users or service accounts, you can ensure that only the appropriate individuals or systems have access to the resources they need.
