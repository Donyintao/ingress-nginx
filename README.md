# Kubernetes 服务暴露介绍

Ingress是Kubernetes 1.2版本以后出现的服务暴露功能；到目前为止Kubernetes总共有三种暴露服务的方式: LoadBlancer Service、NodePort Service、Ingress。

## Ingress介绍

简单来说就是Ingress可以通过nginx等开源的反向代理负载均衡器实现对外暴露服务

## Ingress组件

- 反向代理负载均衡器
- Ingress Controller
- Ingress

### 反向代理负载均衡器

反向代理负载均衡器很简单，说白了就是 nginx、apache 什么的；在集群中反向代理负载均衡器可以自由部署，可以使用 Replication Controller、Deployment、DaemonSet 等方式部署。

### Ingress Controller

Ingress Controller实质上可以理解为是个监视器Ingress Controller通过不断地跟kubernetes API打交道，实时的感知后端 service、pod 等变化，比如新增和减少 pod，service 增加与减少等；当得到这些变化信息后，Ingress Controller再结合下文的 Ingress生成配置，然后更新反向代理负载均衡器，并刷新其配置，达到服务发现的作用。

### Ingress

Ingress 简单理解就是个规则定义；比如说某个域名对应某个 service，即当某个域名的请求进来时转发给某个service;这个规则将与 Ingress Controller结合，然后 Ingress Controller 将其动态写入到负载均衡器配置中，从而实现整体的服务发现和负载均衡。

## ingress-nginx-controller

Kubernetes已经将Nginx与Ingress Controller合并为一个组件，所以Nginx无需单独部署，只需要部署Ingress Controller即可。

### Ingress Controller install

注意: nginx-ingress-controller v0.16.0版本后移除了`nginx-module-vts`模块，采用全新的方式来监控域名和服务状态。

```sh
# kubectl apply -f https://raw.githubusercontent.com/Donyintao/nginx-ingress/master/ingress-nginx-namespace.yaml
# kubectl apply -f https://raw.githubusercontent.com/Donyintao/nginx-ingress/master/ingress-nginx-rbac.yaml
# kubectl apply -f https://raw.githubusercontent.com/Donyintao/nginx-ingress/master/ingress-nginx-configmap.yaml
# kubectl apply -f https://raw.githubusercontent.com/Donyintao/nginx-ingress/master/ingress-nginx-controller.yaml
```