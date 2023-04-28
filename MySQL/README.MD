# Backend - MySQL(DB)
Wordpress 웹 페이지 구성 시 필요한 데이터를 포함하는 Database Layer 이다.

## mysql.yaml
* Statefulset 으로 배포
  * Statefulset은 파드를 재실행 해도 상태 값이 유지되며 PV/PVC를 각각의 파드에 1:1로 배치할 수 있다.
  * 각각의 파드 당 볼륨을 할당할 수 있으므로 볼륨 공유에 따른 충돌 발생을 방지할 수 있다.
* 볼륨
  * StatefulsSet을 사용하면 파드 하나 당 각각의 PV/PVC가 생성된다. 따라서 단일 파드에 붙일 수 있는 Azure Disk를 사용한다.
  * Azure Disk를 스토리지로 사용하기 위해서는 storage class를 "default"로 해도 무방하다.
