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
\- ECR에서 이미지를 가져와서 사용할 수 있도록 Kubernetes 매니페스트 파일 작성

```` ad-info
title: 쿠버네티스 매니페스트 파일(Kubernetes Manifeset File)
color: 255, 255, 255
collapse: close

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
	labels:
		app: <Deployment에 대한 레이블>
spec: #Deployment의 사양
	replicas: <원하는 Pod의 수>
	selector: #관리할 Pod를 선택하는 레이블 셀렉터
		matchLabels:
			app: <애플리케이션 이름>
	template: #Pod의 템플릿 정의
		metadata: #Pod에 대한 메타데이터
			labels:
				app: <애플리케이션 이름>
		spec: #Pod의 사양(컨테이너 정의 등)
			containers: #Pod내에서 실행될 컨테이너 정의
				- name: <컨테이너 이름>
				  image: <사용할 Docker 이미지 이름>
				  ports:
					  - containerPort: <포트번호>
				  livenessProbe: # 정상적으로 동작하고 있는지 확인하는데 사용됨
					  # 첫번째 방식(HTTP GET): 특정 HTTP 엔드포인트로 GET요청을 보내고, 
											  응답코드 확인
					  httpGet:
						  path: <헬스체크 경로>
						  port: <포트 번호>
					  # 두번째 방식(TCP Socket): 특정 포트로 TCP 연결을 시도해 성공여부 확인
					  tcpSocket:
						  port: <포트 번호>
					  # 세번째 방식(Exec): 컨테이너 내부에서 특정 명령을 실행해 결과를 확인
					  exec:
						  command:
							  - <명령어>
					  # 설정 옵션
					  initialDeplySeconds: <초> # Pod가 실행되고 몇 초 후 첫번째 Probe를 실행할지 
												  지정
					  periodSeconds: <초> # Probe를 실행하는 주기 지정
					  timeoutSeconds: <초> # Probe의 타임아웃 시간 설정
					  successThreshold: <횟수> # 연속적으로 성공해야 하는 Probe 횟수 지정
					  failureThreshold: <횟수> # 연속적으로 실패해야 하는 Probe 횟수 지정, 
												  이 횟수 초과시 Pod 재시작
				  readinessProbe: # Pod가 클라이언트 요청을 처리할 준비가 되었는지 확인
									실패시 해당 Pod는 서비스의 로드 밸런서에서 제거됨
					  # livenessProbe와 같은 세 가지 방식(HTTP GET, TCP Socket, Exec)사용
					  # 설정 옵션도 livenessProbe와 동일
					
	
```

3. Service 매니페스트 파일
``` yaml
apiVersion: <API 버전>
kind: Service
metadata: #Service의 메타데이터
	name: <Service 이름>
spec: #Service의 사양
	selector: # Service가 연결할 Pod를 선택하는 레이블 셀렉터
		app: <애플리케이션 이름>
	ports: # Service에서 노출할 포트 정보
	- protocal: <사용할 프로토콜> # ex. TCP, UDP
	  port: <클러스터 내부에서 Service가 노출하는 포트>
	  targetPort: <Pod 내 컨테이너에서 사용하는 포트>
	type: <서비스 유형> # ex. ClusterIP(default), NodePort, LoadBalancer
```
````

**4. 매니페스트 파일 적용**
\- kubectl을 사용해 매니페스트 파일을 클러스터에 적용
``` sh
kubectl apply -f deployment.yaml
kubectl apply -f serivce.yaml
```

**5. 배포 상태 확인**
\- 배포 상태를 확인해 애플리케이션이 올바르게 배포되었는지 확인
```sh
kubectl get pods
kubectl get svc
```

**6.External IP로 접속**


### 📌 CI/CD 파이프라인 구축
---
`CI/CD 파이프라인 - 코드 변경 사항을 자동으로 빌드, 테스트, 배포하는데 사용`

##### 🏷️ AWS CodePipeline

- 전체 파이프라인 개요
	1. 코드 저장소
		\- 코드 변경 사항을 관리하기 위한 GitHub, AWS CodeCommit 등
	2. 빌드 및 테스트
		\- AWS CodeBuild를 사용해 Docker 이미지를 빌드하고 테스트
	3. 이미지 푸시
		\- 빌드된 Docker 이미지를 ECR에 푸시
	4. 배포
		\- AWS CodeDeploy 또는 Kubernetes(EKS)를 사용해 이미지 배포


**1) ECR 저장소 생성**
\- ECR 저장소를 생성해 Docker 이미지 저장

**2) CodeCommit 저장소 생성** (또는 GitHub 저장소 사용)
\- CodeCommit 저장소를 생성해 소스 코드를 저장

**3) CodeBuild 프로젝트 설정**
\- CodeBuild 프로젝트를 생성해 Docker 이미지를 빌드하고 ECR에 푸시

```` ad-info
title: CodeBuild 프로젝트 생성
collapse: close

1. AWS Management Console에서 CodeBuild로 이동
2. 프로젝트 생성 클릭
3. 프로젝트 이름 입력후, 소스 공급자로 CodeCommit (또는 GitHub) 선택
4. 환경 설정에서 Docker이미지를 선택하고 관리혈 이미지로 설정
5. 빌드 사양에서 buildspec.yaml 파일을 사용하도록 설정
	\- buildspec.yaml
``` yaml
version: <버전>

phases:
	pre_build:
		commands:
			- echo Logging in to Amazon ECR
			- aws ecr get-login-password --region <리전 명> | docker login --username AWS --password-stdin <계정 ID>.dkr.ecr.<리전 명>.amazonaws.com
	build:
		commands:
			- echo Build started on `date`
			- echo Building the Docker image...
			- docker build -t <레포지토리 명>
			- docker tag <이미지 명>:<태그 명> <계정 ID>.dkr.ecr.<리전 명>.amazonaws.com/<레포지토리명>:<태그 명>
	post_build:
		commands:
			- echo Build completed on `date`
			- echo Pushing the Docker image...
			- docker push <계정 ID>.dkr.ecr.<리전 명>.amazonaws.com/<레포지토리 명>:<태그 명>
```
````

**4) CodePipeline 설정**
\- CodePipeline을 사용해 전체 CI/CD 파이프라인 정의

``` ad-info
title: CodePipeline 생성
collapse: close

1. AWS Management Console에서 CodePipeline으로 이동
2. 파이프라인 생성 클릭
3. 파이프라인 이름 입력 후, 새 서비스 역할을 설정하도록 설정
4. 소스 단계에서 CodeCommit (또는 GitHub)을 선택하고 저장소와 브랜치 지정
5. 빌드 단계에서 CodeBuild 프로젝트 선택
6. 배포 단계에서 배포 공급자로 AWS CodeDeploy 또는 EKS 선택

```

\- 배포를 자동화 하기 위해 CodePipeline에서 CodeBuild 프로젝트를 사용해 Kubernetes 클러스터에 배포
`buildspec.yaml`
``` yaml
version: <버전>

phases:
	pre_build:
		commands:
			- echo Logging in to Amazon ECR
			- aws ecr get-login-password --region <리전 명> | docker login --username AWS --password-stdin <계정 ID>.dkr.ecr.<리전 명>.amazonaws.com
			- kubectl config use-context arn:aws:eks:<리전 명>:<계정 ID>:cluster/<클러스터 명>
	build:
		commands:
			- echo Build started on `date`
			- echo Building the Docker image...
			- docker build -t <레포지토리 명>
			- docker tag <이미지 명>:<태그 명> <계정 ID>.dkr.ecr.<리전 명>.amazonaws.com/<레포지토리명>:<태그 명>
	post_build:
		commands:
			- echo Build completed on `date`
			- echo Pushing the Docker image...
			- docker push <계정 ID>.dkr.ecr.<리전 명>.amazonaws.com/<레포지토리 명>:<태그 명>
			- echo Deploying to Kubernetes cluster...
			- kubectl apply -f deployment.yaml
```

##### 🏷️Jenkins
**1) Jenkins 설정**

**2) Jenkins 플러그인 설치**
\- Jenkins 플러그인을 설치해 AWS와 Docker 통합
```ad-info
title: Jenkins 플러그인 설치
collapse: close
1. Jenkins Dashboard로 이동
2. Manage Jenkins > Manage Plugins
3. Available 탭에서 플러그인 설치
	- Amazon EC2
	- Amazon ECR
	- Docker Pipeline
	- Kubernetes CLI
	- Pipeline: AWS Steps
```

**3) Jenkins 파이프라인 구성**
\- Jenkins 파이프라인을 구성해 Docker 이미지를 빌드하고
1. Jenkins 플러그인 설치 : Jenkins에 AWS ECR 플러그인 설치
2. 빌드 스크립트 작성 : Jenkins 파이프라인 스크립트에서 Docker 이미지를 빌드하고 ECR에 푸시하는 단계 추가

**4) ECR에 Docker 이미지 푸시**
\- Jenkins 파이프라인이 성공적으로 실행되면 Docker이미지를 ECR에 푸시
\- Jenkinsfile의 `Push To ECR`단계

**5) EKS에 배포**
\- Jenkins 파이프라인이 ECR에 이미지를 푸시한 후, EKS 클러스터에 이미지를 배포
\- Jenkinsfile의 `Deploy to EKS`단계

	`Jenkinsfile`
``` groovy
pipeline {
	agent any
	environment {
		AWS_REGION = '<리전 명>'
		ECR_REPO_NAME = '<레포지토리 명>'
		ECR_REGISTRY = '<계정 ID>.dkr.ect.<리전 명>.amazonaws.com'
		IMAGE_TAG = '<태그 명>'
		AWS_CREDENTIALS_ID = '<JENKINS에 저장된 AWS 자격증명 ID>'
		KUBE_CONFIG = crendentials(')
	}

	stages {
		stage('Checkout') {
			steps {
				// 소스 코드 체크아웃
				git '<git 주소>'
			}
		}
		
		stage('Build') {
			steps {
				script {
					// Docker 이미지 빌드
					sh "docker build -t ${ECR_REPO}:${IMAGE_TAG} ."
				}
			}
		}

		stage('Login to ECR') {
			steps {
				script {
					// AWS CLI를 사용해서 ECR에 로그인
					withCredentials([[$class: 'AmazonWebServiceCredentials', crendentialsId: AWS_CREDENTIALS_ID]]) {
						sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REPO}"
					}
				}
			}
		}

		stage('Push to ECR') {
			steps {
				script {
					// Docker 이미지 ECR에 푸시
					sh "docker push ${ECR_REPO}:${IMAGE_TAG}"
				}
			}
		}

		stage('Deploy to EKS') {
			steps {
				script {
					withCredentials([[$class: 'AmazonWebServiceCredentials', crendentialsId: AWS_CREDENTIALS_ID]]) {
						sh 'kubectl apply -f deployment.yaml'
					}
				}
			}
		}
	}

	post {
		success {
			echo 'Build and Push to ECR succeeded!'
		}
		failure {
			echo 'Build or Push to ECR failed'
		}
	}

}
```

