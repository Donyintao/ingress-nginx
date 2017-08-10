# 基于角色的访问控制

这个例子展示了如何应用一个基于角色的访问控制的nginx ingress控制器。

## 概述

这个示例应用于在启用RBAC的环境中部署的nginx-ingss-控制器。

基于角色的访问控制由四个层次组成:

1.  `ClusterRole` - 分配给整个集群的角色权限
2.  `ClusterRoleBinding` - 将一个ClusterRole绑定到某个帐户
3.  `Role` - 分配给某个Namespace的角色权限
4.  `RoleBinding` - 将Role绑定到某个帐户

为了让RBAC应用到nginx-ingress-controller, controller会被分配到一个ServiceAccount. 
将"ServiceAccount"绑定到"Role"和"ClusterRole"定义的nginx-ingress-controller账户。

## 在本例中创建的服务帐户

创建一个ServiceAccount示例, "nginx-ingress-serviceaccount".

## 本示例中授予的权限

在本例中定义了两组权限, 定义一个名称为nginx-ingress-clusterrole的ClusterRole角色权限和一个名称为nginx-ingress-role的namespace角色权限。

### 集群权限

* `configmaps`, `endpoints`, `nodes`, `pods`, `secrets`: list, watch
* `nodes`: get
* `services`, `ingresses`: get, list, watch
* `events`: create, patch
* `ingresses/status`: update

### Namespace权限

* `configmaps`, `pods`, `secrets`: get
* `endpoints`: create, get, update

* `configmaps`: get, update (for resourceName `ingress-controller-leader-nginx`)
* `configmaps`: create

* `election-id`: `ingress-controller-leader`
* `ingress-class`: `nginx`
* `resourceName` : `<election-id>-<ingress-class>`

### Bindings

将服务帐户nginx-ingss-ServiceAccount绑定到角色"nginx-ingress-role"和集群角色"nginx-ingress-clusterrole".
