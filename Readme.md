# EFK stuck to the AKS cluster

- Fluent parses json logs
- Elastic indexes log fields (including nested fields)
- Kibana is the UI to see the logs

### Prerequisites

- Install [helm](https://phoenixnap.com/kb/install-helm)

### Install Elastic Search and Kibana

- Add helm repo
```
helm repo add elastic https://helm.elastic.co
helm repo update
```

- Install Elasticsearch and Kibana. (replace `password` and/or `username`). Ensure `appgw-ssl-certificate` in kibana-values.yaml
```
kubectl create secret generic elastic-credentials  --from-literal=password=[PASSWORD] --from-literal=username=elastic
helm install elasticsearch --version 7.17.3 elastic/elasticsearch -f ./elasticsearch-values.yaml
helm install kibana --version 7.17.3 elastic/kibana -f ./kibana-values.yaml
```

### Install Fluent

```
kubectl create -f fluentd-config.yaml -f fluentd-ds.yaml
```

### Teardown

```
kubectl delete -f fluentd-config.yaml -f fluentd-ds.yaml
helm uninstall elasticsearch kibana
kubectl delete secret elastic-credentials
```

### Troubleshooting

- Ensure no current PVCs assigned to the volume:

```
$ kubectl get pvc
```
