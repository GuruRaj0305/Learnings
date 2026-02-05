# RBAC (WHO → can do → WHAT → on WHICH resource)

Kubernetes can give access to users and services itself.

Kubernetes uses identity providers to manage users (kubernetes itself does not manages users) and give permissions. 

## there will be 3 high level concepts 
+ service account and users
+ roles and cluster roles
+ role binding and cluster role binding

## Role 
+ Namespace scoped
+ Defines permissions


## ClusterRole
+ Cluster scoped
+ For:
  + nodes
  + pv (Persistent Volume)
  + namespaces
  + or all namespaces

## RoleBinding
+ Connects User/ServiceAccount → Role
+ Namespace only

## ClusterRoleBinding

+ Connects User/ServiceAccount → ClusterRole
+ Entire cluster