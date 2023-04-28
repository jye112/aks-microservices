# Frontend - Wordpress(WEB)
* Deployment로 배포
  * Deployment는 Stateless 성격을 가지고 있으며 파드들이 하나의 볼륨을 공유한다.
  * 파드 간 볼륨을 공유하게 되면 Write 작업 시 충돌이 발생할 수 있지만 WEB Layer의 데이터들은 컨텐츠 파일이기 때문에 Read 작업만 수행하면 되므로 Write 작업에 따른 충돌이 큰 문제가 되지 않는다.
* 환경 변수
  * MySQL DB의 정보들은 wp-config.php 파일 내 환경 변수 처리된다.
  * 해당 환경 변수의 값들을 Deployment에 명시한다.
* 볼륨
  * Deployment는 모든 파드들이 하나의 PVC와 PV를 공유한다. 따라서 파드끼리 데이터를 서로 공유할 수 있다.
  * Azure Files의 경우 여러 파드에 붙을 수 있기 때문에 Azure Files을 볼륨으로 사용한다. (Azure Disk는 단일 파드에만 붙기 때문에 Stateless한 성격의 Deployment로 배포할 시 적합하지 않다.)
