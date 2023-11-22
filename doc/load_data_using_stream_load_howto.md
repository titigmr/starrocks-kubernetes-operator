# Load Data Using Stream Load

The issue is that when the load client residents in a different network other than FE/BE's private network. FE's HTTP
307 brings BE's private network address to the client who does not recognize and can't process the redirection.

If you install StarRocks with Helm, you can add the following configuration to the `values.yaml` file:

For `kube-starrocks` Helm chart:

```yaml
starrocks:
  starRocksFeProxySpec:
    replicas: 1
    # set the resolver for nginx server, default kube-dns.kube-system.svc.cluster.local
    resolver: ""
    limits:
      cpu: 1
      memory: 2Gi
    requests:
      cpu: 1
      memory: 2Gi
    service:
      type: NodePort
      ports:
        - containerPort: 8080
          name: http-port
          nodePort: 30180
          port: 8080
```

For `starrocks` Helm chart:

```yaml
starRocksFeProxySpec:
  replicas: 1
  # set the resolver for nginx server, default kube-dns.kube-system.svc.cluster.local
  resolver: ""
  limits:
    cpu: 1
    memory: 2Gi
  requests:
    cpu: 1
    memory: 2Gi
  service:
    type: NodePort
    ports:
      - containerPort: 8080
        name: http-port
        nodePort: 30180
        port: 8080
```

Please
see https://github.com/StarRocks/starrocks-kubernetes-operator/blob/main/helm-charts/charts/kube-starrocks/values.yaml
for more details about how to configure `starRocksFeProxySpec`.

### Deploy Fe Proxy Using Helm

If you install StarRocks with StarRocksCluster CR yaml, please see [deploy_a_starrocks_cluster_with_fe_proxy.md](
../examples/starrocks/deploy_a_starrocks_cluster_with_fe_proxy.yaml)