# Ingress Controller 구성
* Ingress 규칙이 동작하게 하기 위한 컨트롤러이다.
* 외부로부터 네트워크 요청을 수신했을 때 Ingress 규칙에 기반해 이 요청을 어떻게 처리할지 결정하고 이를 통해 Ingress에 적용된 규칙이 활성화된다.

* Ingress Controller 설치 방법
```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml
```

<참고> Ingress 생성 시 admission 관련 오류 발생했을 때
```
$ kubectl delete validatingwebhookconfiguration ingress-nginx-admission
```

# HTTPS Ingress 구성
* Ingress란 클러스터 외부의 네트워크로부터 액세스 받기 위한 컨트롤러로 L7 로드밸런서에 해당한다.
* Ingress Controller는 외부로부터 네트워크 요청을 수신했을 때 Ingress 규칙에 기반해 이 요청을 어떻게 처리할지 결정하고 이를 통해 Ingress에 적용된 규칙이 활성화된다.
* External IP(Public IP)가 붙어 서비스 된다.
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

## 2) CA 클러스터 발급자 생성
* 인증서가 발급되기 전에 cert-manager에게는 Issuer 또는 ClusterIssuer 리소스가 필요하다.
* Issuer은 단일 네임스페이스에서 작동하고 ClusterIssuer은 모든 네임스페이스에서 작동한다.
```
$ kubectl apply -f cluster-issuer.yaml
$ kubectl get clusterissuer
```

## 3) Ingress 생성
* 두 애플리케이션은 실행되지만 ClusterIP 형식의 서비스로 구성되며 인터넷에서 액세스할 수 없다.
* 애플리케이션을 공개적으로 사용할 수 있도록  Kubernetes Ingrss 리소스를 만든다.
* wordpress 애플리케이션으로 트래픽을 라우팅하는 규칙을 구성한다.
```
$ kubectl apply -f ingress-nginx.yaml
$ kubectl get ingress -n ingress-nginx
```

```
spec:
  tls:
  - hosts: 
    - wordpress.jyeclc.com  // 인증서를 적용할 도메인 주소
    secretName: tls-secret  // 인증서 key가 저장되는 secret으로 별도의 사전 생성 없이 cert-manager에 의해 자동 생성된다.
 ```
 ```
 $ kubectl get certificate -n ingress-nginx
 $ kubectl get secret -n ingress-nginx
 ```