# HTTPS Ingress 구성
* TLS 인증서를 발급 받아 HTTPS를 통해 인터넷에 서비스를 노출하는 방식으로 진행한다.
* 자동 Let's Encrypt 인증서 생성 및 관리 기능을 제공하는 cert-manager를 사용하여 인증서를 제공받는다.

## 1) Cert-manager 설치
* cert-manager는 기본 Kubernetes 인증서를 관리하는 컨트롤러이다.
* Let 's Encrypt , HashiCorp Vault , Venafi, 간단한 서명 키 쌍 또는 자체 서명과 같은 다양한 소스에서 인증서를 발급하는 데 도움이 된다.
```
# 리소스 유효성 검사를 사용하지 않도록 ingress-nginx namespace에 레이블 지정
$ kubectl label namespace ingress-nginx cert-manager.io/disable-validation=true

# cert-manager namespace 생성
$ kubectl create namespace cert-manager

# Jetstack Helm Repository 추가
$ helm repo add jetstack https://charts.jetstack.io
$ helm repo update

# cert-manager helm chart 설치
$ helm install cert-manager jetstack/cert-manager `
  --namespace cert-manager `
  --set installCRDs=true `
  --set nodeSelector."kubernetes\.io/os"=linux `
  --set webhook.nodeSelector."kubernetes\.io/os"=linux `
  --set cainjector.nodeSelector."kubernetes\.io/os"=linux 
```

## 2) CA 클러스터 발급자 만들기
* 인증서가 발급되기 전에 cert-manager에게는 Issuer 또는 ClusterIssuer 리소스가 필요하다.
* Issuer은 단일 네임스페이스에서 작동하고 ClusterIssuer은 모든 네임스페이스에서 작동한다.
```
$ kubectl apply -f cluster-issuer.yaml
```
* 발급된 인증서를 확인한다.
![image](https://user-images.githubusercontent.com/50107548/235066345-4636bafc-6264-42a3-a91d-cca771d054a1.png)
