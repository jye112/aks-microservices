# Prometheus & Grafana 구성
Prometheus로 메트릭을 수집하고 Grafana 대시보드를 구성하여 수집된 메트릭을 시각화 할 수 있도록 한다.

## Repository 추가
```
$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
$ helm repo add grafana https://grafana.github.io/helm-charts
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


# ELK(ElasticSearch / Logstash / Kibana) 구성
Logstash가 애플리케이션 관련 로그를 수집하면 ElasticSearch가 해당 로그 데이터를 저장하고, Kibana를 통해 시각화(대시보드) 한다.

## Repository 추가
```
$ helm repo add elastic https://helm.elastic.co
```

## ElasticSearch 설치
```
$ helm install elasticsearch elastic/elasticsearch
```

## Logstash 설치
```
$ helm install logstash elastic/logstash
```

## Kibana 설치
```
$ helm install kibana elastic/kibana
```
