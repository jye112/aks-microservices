# Ingress
- 클러스터 외부의 네트워크로부터 액세스 받기 위한 컨트롤러로 L7 로드밸런서에 해당한다.
- Ingress Controller는 외부로부터 네트워크 요청을 수신했을 때 Ingress 규칙에 기반해 이 요청을 어떻게 처리할지 결정하고 이를 통해 Ingress에 적용된 규칙이 활성화된다.
- External IP(Public IP)가 붙어 서비스 된다.

# Ingress Controller
- Ingress 규칙이 동작하게 하기 위한 컨트롤러
- 외부로부터 네트워크 요청을 수신했을 때 Ingress 규칙에 기반해 이 요청을 어떻게 처리할지 결정하고 이를 통해 Ingress에 적용된 규칙이 활성화된다.

## Ingress Controller 설치 방법
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml
```

### <참고> Ingress 생성 시 admission 관련 오류 발생했을 때
```
kubectl delete validatingwebhookconfiguration ingress-nginx-admission
```
