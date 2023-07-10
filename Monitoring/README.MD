# Prometheus & Grafana 구성
```
Prometheus로 메트릭을 수집하고 Grafana 대시보드를 구성하여 수집된 메트릭을 시각화 할 수 있도록 한다.
```

## Monitoring 용 Namespace 생성
```
$ kubectl create ns monitoring
```

## Prometheus 설치
```
$ helm install prometheus -n monitoring -f values.yaml bitnami/kube-prometheus

```

## Grafana 설치
```
$ helm install grafana -n monitoring -f values.yaml bitnami/grafana

```
