# AKS with Azure Key Vault

![image](https://user-images.githubusercontent.com/50107548/235052130-7dc2cbb5-0f96-4a8a-b6d4-b05bd3aa8bf8.png)

* Azure Key Vault Controller을 통해 Azure Key vault 상에 있는 Secret을 쿠버네티스 클러스터 상의 Native Secret 형식으로 불러와 사용할 수 있다.
* Azure Key Vault Env Injector을 통해 Azure Key vault 상에 있는 Secret을 환경 변수로 컨테이너에 주입시킬 수 있다.


## 1) Azure Key vault 생성 및 액세스 정책 설정
* Azure Key vault를 생성하고 Secret을 생성한다.
* Azure Key vault 상에서 액세스 정책을 설정한다. 애플리케이션 형태의 AKS의 Managed Identity를 액세스 정책에 추가하여 AKS에서 Key vault에 접근할 수 있도록 한다.

## 2) 클러스터에 Azure Key vault Controller 설치
```
kubectl create ns akskeyvault
```
```
helm repo add spv-charts http://charts.spvapi.no
helm repo update
```
```
helm install akv2k8s spv-charts/akv2k8s --namespace akskeyvault
```

## 3) AzureKeyvaultSecret 생성
* 반드시 애플리케이션이 배포되는 Namespace와 동일한 Namespace에 배포해야 한다.

## 4) 애플리케이션에서 Secret 참조
* AzureKeyvaultSecret을 생성하면 Azure Key vault 상의 Secret 값을 가지는 쿠버네티스의 Native secret이 생성된다.
![image](https://user-images.githubusercontent.com/50107548/235057643-7c81edd3-73fa-46c2-9b79-f014a7b60f40.png)
* 애플리케이션에서는 해당 Secret을 참조하면 된다.
```
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom: 
            secretKeyRef:
              name: db-secret
              key: password
```
