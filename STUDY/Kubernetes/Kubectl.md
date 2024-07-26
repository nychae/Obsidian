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
- 하나 이상의 리소스 목록 조회
- 주요 TYPE : pods, nodes, deployments, replicasets, services
- flags:
	-o wide -> 더 많은 정보 조회
	-o json 또는 -o yaml... -> 출력형식 지정

``` sh
kubectl get [TYPE] [NAME] [flags]

# TYPE에 해당하는 목록을 일반 텍스트 출력 형식으로 나열
# 여러개의 TYPE 나열 가능
kubectl get [TYPE1(, TYPE2, ...)]

# TYPE에 해당하는 목록을 일반 텍스트 출력 형식으로 나열하고 추가 정보(ex. 노드 이름)를 포함
kubectl get [TYPE] -o wide

# NAME에 해당하는 TYPE을 일반 텍스트 출력 형식으로 나열
kubectl get [TYPE] [NAME]
```

##### 🏷️ describe
- 리소스의 자세한 상태 표시
- 리소스의 상세 정보나 상태, 실패한 이유를 확인할 때 주로 사용

``` sh
# NAME에 해당하는 TYPE의 세부사항 표시
kubectl describe [TYPE] [NAME]
 또는
kubectl describe [TYPE]/[NAME]

# 모든 TYPE의 정보 표시
kubectl describe [TYPE]
```

##### 🏷️ delete
- 리소스 제거

``` sh
# 파일에 지정된 타입과 이름을 사용하여 삭제
kubectl delete -f [파일명.확장자]

# <labe-key> = <label-value> 레이블이 있는 모든 타입 삭제
kubectl delete [TYPE1(, TYPE2, ...)] -l <label-key> = <label-value> 

# 초기화 되지 않은 타입 포함 모든 타입 삭제
kubectl delete [TYPE] --all
```

##### 🏷️ logs
- 파드의 컨테이너에 대한 로그 출력

``` sh
# NAME에 해당하는 pod에서 로그의 스냅샷 표시
kubectl logs [NAME]

# NAME에 해당하는 pod에서 로그 스트리밍 시작 (리눅스의 'tail -f'와 비슷)
kubectl logs -f [NAME]
```

##### 🏷️ exec
- 파드의 컨테이너에 대해 명령 실행

``` sh
# NAME에 해당하는 pod에서 date를 실행한 결과 표시 (기본적으로 첫번째 컨테이너에서 출력)
kubectl exec [NAME] -- date

# NAME에 해당하는 pod의 특정 컨테이너에서 date를 실행한 결과 표시
kubectl exec [NAME] -c [container-name] -- date

# NAME에 해당하는 pod에서 대화식 TTY를 연결해 /bin/bash 실행 (기본적으로 첫번째 컨테이너에서 출력)
kubectl exec -it [NAME] -- /bin/bash
```

##### 🏷️ diff
- 라이브 구성에 대해 파일이나 표준입력의 차이점 출력

``` sh
# 파일에 포함된 리소스의 차이점 출력
kubectl diff -f <파일명.확장자>

# 표준입력에서 파일을 읽어 차이점 출력
cat <파일명.확장자> | kubectl diff -f -
```