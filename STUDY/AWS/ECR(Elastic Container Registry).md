# ECR(Elastic Container Registry)
- AWS에서 제공하는 완전 관리형 Docker 컨테이너 레지스트리 서비스
- 컨테이너 이미지를 저장, 관리 및 배포할 수 있는 기능을 제공하며 AWS의 다른 서비스와 원활하게 통합됨

### 📌 특징
---
1. 완전 관리형 서비스
	\- 컨테이너 이미지 저장소의 운영, 확장 및 보안 관리 작업을 AWS가 대신 처리
2. 통합된 인증 및 권한 관리
	\- AWS IAM과 통합되어 세밀한 액세스 제어를 제공
	\- 사용자는 IAM 정책을 통해 이미지 저장소에 대한 액세스를 제어함
3. 높은 가용성과 내구성
	\- AWS의 글로벌 인프라를 활용해 높은 가용성과 내구성을 제공
	\- 데이터는 여러 가용 영역에 걸쳐 복제됨
4. 간편한 이미지 Push 및 Pull
	\- AWS CLI 또는 Docker CLI를 사용해 이미지를 ECR로 푸시하거나 ECR에서 풀 수 있음
5. 통합된 CI/CD 파이프라인
	\- AWS CodePipiline, Jenkins 등과 같은 CI/CD 도구와 쉽게 통합해 자동화된 빌드 및 배포 파이프라인 구축 가능
6. 취약점 스캔
	\- 컨테이너 이미지의 보안 취약점을 스캔하는 기능을 제공하여, 보안 취약점을 사전에 발견하고 해결 가능


### 📌 사용방법
---
**1. ECR Repository 생성**

**2. Docker 이미지를 ECR로 Push**
1) ECR에 로그인
	\- AWS CLI를 사용해 Docker CLI를 ECR에 인증
``` sh
aws ecr get-login-password --region <region 명> | docker login --username AWS --password-stdin <계정 ID>.dkr.ecr.<resion 명>.amazonaws.com
``` 

2) Docker 이미지 빌드
``` sh
docker build -t <이미지 명>
```
 
3) Docker 이미지 태그 지정
	\- 로컬 Docker 이미지를 ECR 레포지토리에 맞게 태그 지정
``` sh
docker tag <이미지 명>:<태그 명> <계정 ID>.dkr.ecr.<region 명>.amazonaws.com/<레포지토리 명>:<태그 명>
```

4) Docker 이미지 Push
	\- 태그가 지정된 이미지를 ECR 레포지토리에 Push
``` sh
docker push <계정 ID>.dkr.ecr.<region 명>.amazonaws.com/<레포지토리 명>:<태그 명>
```

**3. Docker 이미지를 ECR에서 Pull**
1) ECR에 로그인
``` sh
aws ecr get-login-password --region <region 명> | docker login --username AWS --password-stdin <계정 ID>.dkr.ecr.<resion 명>.amazonaws.com
```

2) Docker 이미지 Pull
	\- ECR 레포지토리에서 이미지를 로컬 환경으로 Pull
``` sh
docker pull <계정 ID>.dkr.ecr.<region 명>.amazonaws.com/<레포지토리 명>:<태그 명>
```


### 📌 EKS로 배포하는 방법
---
**1. EKS 클러스터 생성**

**2.ECR 이미지 사용을 위한 IAM 역할 구성**
 \- IAM 역할 생성 및 정책 연결

**3. Kubernetes 매니페스트 파일 작성**

```` ad-info
title: 쿠버네티스 매니페스트 파일(Kubernetes Manifeset File)
color: 255, 255, 255
collapse: open

- 쿠버네티스 매니페스트 파일(Kubernetes Manifest File)
	\- 쿠버네티스 리소스를 정의하고 설정하기 위한 YAML 또는 JSON파일
	\- 파드(Pod), 서비스(Service), 디플로이먼트(Deployment) 등 다양한 쿠버네티스 리소스를 생성, 관리 및 배포하는데 사용

- 기본 구조
``` yaml
apiVersion: <API 버전>
kind: <리소스 종류>
metadata:
	name: <리소스 이름>
	labels: 
		<키>:<값>
spec: <리소스의 상세 설정>
```

- 예제
1. Pod 매니페스트 파일
``` yaml
apiVersion: <API 버전>
kind: Pod
metadata: 
	name: <Pod 이름>
	labels:
		<키>:<값>
spec:
	containers: #Pod에 포함된 컨테이너 목록
		- name: <컨테이너 이름>
		  image: <사용할 Docker 이미지 명>
		  ports:
			  -  containerPort: <포트 번호>
		  env: #컨테이너에서 사용할 환경변수
			  - name: <환경변수 명>
				value: <값>
		  volumes: #Pod에서 사용할 볼륨 정의 
			  - name: <볼륨 이름>
			    emptyDir: <Pod가 시작될 때 생성되며, Pod가 삭제되면 사라지는 빈 디렉토리>
```

2. Deployment 매니페스트 파일
``` yaml
apiVersion: <API 버전>
kind: Deployment
metadata:
	name: <Deployment 이름>
	
```
````
