# Frontend - Wordpress(WEB)
Wordpress 웹 페이지에 해당하는 Web Layer 이다.

## wordpress.yaml
```
$ kubectl apply -f wordpress.yaml
```

* Deployment로 배포
  * Deployment는 Stateless 성격을 가지고 있으며 파드들이 하나의 볼륨을 공유한다.
  * 파드 간 볼륨을 공유하게 되면 Write 작업 시 충돌이 발생할 수 있지만 WEB Layer의 데이터들은 컨텐츠 파일이기 때문에 Read 작업만 수행하면 되므로 Write 작업에 따른 충돌이 큰 문제가 되지 않는다.
* 환경 변수
  * MySQL DB의 정보들은 wp-config.php 파일 내 환경 변수 처리된다.
  * 해당 환경 변수의 값들을 Deployment에 명시한다.
* 볼륨
  * Deployment는 모든 파드들이 하나의 PVC와 PV를 공유한다. 따라서 파드끼리 데이터를 서로 공유할 수 있다.
  * Azure Files의 경우 여러 파드에 붙을 수 있기 때문에 Azure Files을 볼륨으로 사용한다. (Azure Disk는 단일 파드에만 붙기 때문에 Stateless한 성격의 Deployment로 배포할 시 적합하지 않다.)
  * 스토리지로 Azure Files를 사용하기 위해서는 storage class를 "azurefile-premium"으로 사용하고 access mode도 "ReadWriteMany"로 한다.

## external-wordpress.yaml
```
$ kubectl apply -f external-wordpress.yaml
```

* 만약 ingress controller에 --watch-namespace 옵션이 사용되는 환경에서 다른 namespace의 service가 해당 ingress를 사용해야 할 경우 ingress가 동작되지 않는다.
* 이때 ExternalName 타입의 서비스를 생성해서 우회적으로 연결할 수 있다.
* ExternalName 타입의 서비스는 ingress가 배포된 namespace와 동일한 namespace에 배포해야 하며, externalName에 실제 애플리케이션(wordpress)의 서비스 도메인 주소를 명시해야 한다.
