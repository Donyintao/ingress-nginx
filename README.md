# Role Based Access Control

这个例子演示了如何部署一个nginx ingress controller和基于RBAC角色的访问控制, 并使用ConfigMap来启用nginx vts模块和普罗米修斯的输出指标。

## Overview

这个例子适用于nginx-ingress-controllers被部署在一个启用了RBAC的环境。

Role Based Access Control是由四个资源组成 :

1.  `ClusterRole` - 权限分配给角色,适用于整个cluster
2.  `ClusterRoleBinding` - 绑定ClusterRole到一个特定的帐户
3.  `Role` - 权限分配给角色,适用于一个指定的namespace
4.  `RoleBinding` - 绑定Role到一个指定的帐户

为了将RBAC适用到nginx-ingss-controller，将该controller分配给ServiceAccount。这个ServiceAccount需要绑定到nginx-ingress-controller中定义的Roles和ClusterRoles。

## Service Accounts created in this example

本示例中定义了一个ServiceAccount示例, `nginx-ingress-serviceaccount`.

## Permissions Granted in this example

本示例中定义了两组权限。由ClusterRole名为nginx-ingress-clusterrole定义的集群范围权限和名为nginx-ingress-role的Role定义的名称空间特定权限。

### Cluster Permissions

这些权限是为了使nginx-ingress-controller能够作为跨集群的ingress的功能而授予的。这些权限被授予了名为nginx-ingress-ClusterRole的ClusterRole。

* `configmaps`, `endpoints`, `nodes`, `pods`, `secrets`: list, watch
* `nodes`: get
* `services`, `ingresses`: get, list, watch
* `events`: create, patch
* `ingresses/status`: update

### Namespace Permissions

这些权限主要用于nginx-ingress namespace。这些权限被授予了名为nginx-ingress-role的Role

* `configmaps`, `pods`, `secrets`: get
* `endpoints`: create, get, update

### Bindings

The ServiceAccount `nginx-ingress-serviceaccount` is bound to the Role
`nginx-ingress-role` and the ClusterRole `nginx-ingress-clusterrole`.
ServiceAccount nginx-ingss-ServiceAccount将被绑定到nginx-ingss-role和nginx-ingress-ClusterRole。

## Namespace created in this example

在本例中定义了名为nginx-ingress的名称空间。


## Usage

1.  Create the `Namespace`, `Service Account`, `ClusterRole`, `Role`,
`ClusterRoleBinding`, and `RoleBinding`.

```sh
kubectl create -f https://raw.githubusercontent.com/Donyintao/nginx-ingress/master/nginx-ingress-controller-rbac.yml
```

2. Create default backend
```sh
kubectl create -f https://raw.githubusercontent.com/Donyintao/nginx-ingress/master/default-backend.yml
```

3. Create the nginx-ingress-controller

For this example to work, the Service must be in the nginx-ingress namespace:

```sh
kubectl create -f https://raw.githubusercontent.com/Donyintao/nginx-ingress/master/nginx-ingress-controller.yml
```
