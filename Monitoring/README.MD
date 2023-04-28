# Monitoring 용 Namespace 생성
```
$ kubectl create ns monitoring
```

# Prometheus 설치
```
$ helm install prometheus -n monitoring -f values.yaml bitnami/kube-prometheus

```

# Grafana 설치
```
$ helm install grafana -n monitoring -f values.yaml bitnami/grafana

```