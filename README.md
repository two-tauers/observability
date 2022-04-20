# observability

Observability for the cluster

## Grafana Loki

### Docs

https://grafana.com/docs/loki/latest/

https://github.com/grafana/helm-charts/tree/main/charts/loki-stack

### Install

```
helm repo add grafana https://grafana.github.io/helm-charts
helm install loki-stack grafana/loki-stack --values values.yaml --namespace o11y --create-namespace
```

Retrieve Grafana password:

```
kubectl get secret --namespace o11y loki-stack-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Port-forward to [localhost:3000](http://localhost:3000):

```
kubectl port-forward -n monitoring svc/loki-stack-grafana 3000:80
```

### Uninstall

```
helm delete loki-stack -n monitoring
```
