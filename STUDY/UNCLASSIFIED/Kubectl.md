# Kubectl
- 쿠버네티스 클러스터를 제어하기 위한 CLI 툴

- 기본 형식
``` sh
kubectl [command] [TYPE] [NAME] [flags]
```
1) command
	\- 하나 이상의 리소스에서 수행하려는 동작을 지정
	\- ex) create, get, describe, delete

2) TYPE
	\- 리소스 타입을 지정
	\- 대소문자를 구분하지 않음
	\- 단수형, 복수형 또는 약어 형식 지정 가능
	
``` ad-info
title: TYPE 종류
collapse: close

|type 명     | 약어     |
| --- | --- |
| bindings  |     |
|componentstatuses| cs|
|configmaps|cm|
|endpoints|ep|
|events|ev|
|limitranges|limits|
|namespaces|ns|
|nodes|no|
|persistentvolumeclaims|pvc|
|persistentvolumes|pv|
|pods|po|
|podtemplates| |
|replicationcontrollers|rc|
|resourcequotas|quota|
|secrets| |
|serviceaccounts|sa|
|services|svc|
|mutatingwebhookconfigurations| |
|validatingwebhookconfigurations| |
|customresourcedefinitions|crd,crds|
|apiservices| |
|controllerrevisions| |
|daemonsets|ds|
|deployments|deploy|
|replicasets|rs|
|statefulsets|sts|
|tokenreviews| |
|localsubjectaccessreviews| |
|selfsubjectaccessreviews| |
|selfsubjectrulesreviews| |
|subjectaccessreviews| |
|horizontalpodautoscalers|hpa|
|cronjobs|cj|
|jobs| |
|certificatesigningrequests|csr|
|leases| |
|endpointslices| |
|events|ev|
|flowschemas| |
|prioritylevelconfigurations| |
|ingressclasses| |
|ingresses|ing|
|networkpolicies|netpol|
|runtimeclasses| |
|poddisruptionbudgets|pdb|
|podsecuritypolicies|psp|
|clusterrolebindings| |
|clusterroles| |
|rolebindings| |
|roles| |
|priorityclasses|pc|
|csidrivers| |
|csinodes| |
|csistoragecapacities| |
|storageclasses|sc|
|volumeattachments||
```

3) NAME
	\- 리소스 이름을 지정
	\- 대소문자를 구분
	\- 이름 생략시 모든 리소스에 대한 세부 사항이 표시됨
	\- 여러 리소스에 대한 작업을 수행할 때, 타입 및 이름별로 각 리소스를 지정하거나 하나 이상의 파일 지정 가능
		- 타입 및 이름으로 리소스 지정
			1. 리소스가 모두 동일한 타입인 경우
				`ex) kubectl get pod example-pod1 example-pod2`
			2. 여러 리소스 타입인 경우
				`ex) kubectl get pod/example-pod1 replicationcontroller/example-rc1`
		- 파일로 리소스 지정
			`ex) kubectl get -f file1.yaml -f file2.yaml -f file3.yaml`

4) flags
	\- 선택적 플래그를 지정
	
### 📌 주요 명령어
---
##### 🏷️ apply
- 리소스 생성
- 리소스가 정의된 YAML 파일을 이용해 리소스 생성 (파일뿐만아니라 URL도 가능)

``` sh
kubectl apply -f [YAML 파일명 of URL]
```

##### 🏷️ get
- 리소스 목록 조회
- 주요 TYPE : pods, nodes, deployments, replicasets, services
- flags:
	-o wide -> 더 많은 정보 조회
	-o json 또는 -o yaml... -> 출력형식 지정

``` sh
kubectl get [TYPE] [NAME] [flags]
```

##### 🏷️ describe
- 리소스의 자세한 상태 표시
- 리소스의 상세 정보나 상태, 실패한 이유를 확인할 때 주로 사용
``` sh
kubectl describe [TYPE 또는 NAME] 

또는

kubectl describe [TYPE] [NAME]
```