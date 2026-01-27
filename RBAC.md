### From Master Node, Create three serviceAccountn, ClusterRole, ClusterRoleBinding : 


`vi admin.yaml`

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin
  namespace: default
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: admin-role
rules:
- apiGroups: ["*"]
  resources: ["*"]
  verbs: ["*"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-binding
subjects:
- kind: ServiceAccount
  name: admin
  namespace: default
roleRef:
  kind: ClusterRole
  name: admin-role
  apiGroup: rbac.authorization.k8s.io
```
  
  
`vi general.yaml`
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: general
  namespace: default
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: general-role
rules:
- apiGroups: [""]
  resources: ["pods", "services", "endpoints", "namespaces"]
  verbs: ["get", "list", "watch"]
- apiGroups: ["apps"]
  resources: ["deployments", "replicasets", "daemonsets", "statefulsets"]
  verbs: ["get", "list", "watch"]
- apiGroups: ["batch"]
  resources: ["jobs", "cronjobs"]
  verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: general-binding
subjects:
- kind: ServiceAccount
  name: general
  namespace: default
roleRef:
  kind: ClusterRole
  name: general-role
  apiGroup: rbac.authorization.k8s.io  
```  
  
  
`vi other.yaml`
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: other
  namespace: default
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: other-role
rules:
- apiGroups: [""]
  resources: ["namespaces"]
  verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: other-binding
subjects:
- kind: ServiceAccount
  name: other
  namespace: default
roleRef:
  kind: ClusterRole
  name: other-role
  apiGroup: rbac.authorization.k8s.io
```
  
  
  
  
### From Master Node, now apply the files from : 
```
kubectl apply -f admin.yaml
kubectl apply -f general.yaml
kubectl apply -f other.yaml  
```




### From Master Node, a short-lived, temporary authentication token for a specific ServiceAccount : 
```
kubectl create token admin
kubectl create token general
kubectl create token other
```




### This is grabbed from /.kube/config file of master node. 
### We have to make 3 different config files whcih should be kept in other machine where the individual admin or general or other user will try for authentication and authorization, So, the below files needs to be edited which carry the tokens to access the cluster. 

`cat ~/.kube/config`
```
apiVersion: v1
kind: Config
clusters:
- cluster:
    certificate-authority-data: <YOUR_CA_DATA_HERE FROM MASTER>
    server: https://<YOUR_MASTER_NODE_IP>:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: admin
  name: admin-context
current-context: admin-context
users:
- name: admin
  user:
    token: <OUTPUT OF kubectl create token admin/geberal/token>
```	
	
	
Before this,please ensure whether the kubectl command should be run from those machines : 

# sudo apt update 
# sudo snap install kubectl --classic					//install kubectl 
# export KUBECONFIG=/app/user-general.yaml				//export the config file in those machines 
	

	
Now check the necessary access after login to the machine: 


kubectl auth can-i delete pods

