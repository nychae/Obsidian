# EKS(Elastic Kubernetes Service)

- AWS에서 제공하는 관리형 Kubernetes 서비스로, Kubernetes 클러스터를 쉽게 배포, 관리, 확장할 수 있도록 도와주며 AWS 인프라와 통합되어 다양한 AWS 서비스와 원활히 작동함

### 📌 특징
---
1. 관리형 서비스
	- 관리형 Kubernetes 컨트롤 플레인 
		\- Amazon EKS는 Kubernetes의 컨트롤 플레인(마스터 노드)을 관리함
		\- 사용자는 컨트롤 플레인을 직접 관리할 필요없이 AWS가 이를 대신 관리하고 유지보수함
	- 자동 업데이트 및 패치
		\- Kubernetes의 버전 업데이트 및 패치를 자동으로 처리하여 최신 기능과 보안 패치를 사용할 수 있도록 지원함

2. 고가용성 및 내구성
	- 고가용성 컨트롤 플레인
		\- 여러 가용 영역(AZ)에 걸쳐 컨트롤 플레인을 자동으로 배포하여 고가용성 보장
	- 자동 복구
		\- 컨트롤 플레인이 장애를 겪을 경우 자동으로 복구되며, 안정적인 운영 유지

3. 보안 및 권한 관리
	 - IAM 통합
		\- AWS IAM(Identity and Access Management)과 통합되어 세밀한 권한 관리를 제공함 
			-> 클러스터와 리소스에 대한 접근 권한을 제어할 수 있게함 
	 - 네트워크 정책
		\- Kubernetes 네트워크 정책을 사용해 Pod 간의 트래픽을 제어하고, 보안 그룹을 통해 인바운드 및 아웃바운드 트래픽 제거 가능 
	 - AWS PrivateLink
		\- EKS API 서버에 대한 안전한 프라이빗 액세스 제공 

4. 확장성 및 유연성
	- 자동 확장
		\- Kubernetes의 클러스터 오토스케일러와 Pod 오토스케일러를 지원하여 수요 변화에 따라 자동으로 클러스터 및 pod를 확장하거나 축소할 수 있음
	- Fargate 지원
		\- AWS Fargate와 통합되어 서버리스 방식으로 Pod를 실행할 수 있음 
			-> 인프라 관리의 복잡성을 줄이고, 리소스 사용량에 따라 자동으로 확장 및 축소할 수 있게 함 
			
``` ad-info
title: Pod
color: 255, 255, 255
collapse: close
Pod
- 가장 작은 배포 가능한 컴퓨팅 단위로, 하나 이상의 컨테이너 그룹을 나타냄
- 동일한 네트워크 네임스페이스, IP 주소 및 스토리지 볼륨을 공유하는 컨테이너 그룹을 제공
- Pod 내의 컨테이너는 협력하여 애플리케이션의 특정 기능을 구현하며, 서로 긴밀하게 상호작용 할 수 있음		
- 특징
	1) 단일 IP 주소
		\- Pod 내의 모든 컨테이너는 동일한 네트워크 네임스페이스와 IP주소를 공유
			-> Pod 내의 컨테이너 간에 로컬 네트워크를 통해 통신 가능
	2) 스토리지 볼륨 공유
		\- Pod는 하나 이상의 스토리지 볼륨을 정의할 수 있으며, Pod 내의 모든 컨테이너는 이 볼륨을 공유할 수 있음 -> 데이터 공유 및 상태 저장 애플리케이션에 유리
	3) 동일한 네트워크 네임스페이스
		\- Pod 내의 컨테이너는 동일한 네트워크 네임스페이스를 공유하여, 서로 간에 로컬 호스트 이름을 사용하여 통신 가능
	4) 단일 프로세스 또는 다중 프로세스
		\- 일반적으로 하나의 Pod는 하나의 주요 프로세스를 실행하는 단일 컨테이너로 구성되지만, 경우에 따라 보조 프로세스를 실행하는 여러 컨테이너로 구성될 수 있음
	5) 생명 주기 관리
		\- Kubernetes가 관리하며, 필요에 따라 생성, 삭제 및 복제됨
		\- 상태는 Kubernetes 컨트롤러에 의해 지속적으로 모니터링 됨

- 생명주기
	1. Pending
		\- Pod가 생성되었지만, 아직 컨테이너가 시작되지 않은 상태
		\- 스케줄러가 적절한 노드를 찾고 있는 단계
	2. Running
		\- Pod가 적절한 노드에 배치되었고, 모든 컨테이너가 실행 중인 상태
	3. Succeeded
		\- Pod 내의 모든 컨테이너가 성공적으로 종료된 상태
		\- 주로 배치 작업과 같은 일회성 작업에 해당
	4. Failed
		\- Pod 내의 하나 이상의 컨테이너가 비정상적으로 종료된 상태
	5. Unknown
		\- Pod의 상태를 알 수 없는 경우 (네트워크 문제 등...)
```

5. 통합 및 생태계
	- AWS 서비스 통합
		\- ELB(Elastic Load Balancer)를 통한 로드 밸런싱, EBS를 통한 스토리지, CloudWatch를 통한 로깅 및 모니터링, IAM을 통한 인증같은 다양한 AWS 서비스와 통합되어있음
	- Kubernetes 생태계 지원
		 \- 표준 kubernetes API를 사용하므로 기존 kubernetes 도구 및 플러그인과 호환됨

6. 멀티 클러스터 및 하이브리드 클라우드
	- 멀티 클러스터 관리
		\- AWS의 여러 리전에서 kubernetes 클러스터를 쉽게 배포하고 관리할 수 있음
	- 하이브리드 클라우드 지원
		\- AWS Outputs, AWS Wavelength, AWS Local Zone을 통해 온프레미스 및 엣지 위치에서도 kubernetes 클러스터 실행 가능

7. 비용 효율성
	- 사용한 만큼만 지불
		\- 사용한 리소스에 대해서만 비용을 지불. 컨트롤 플레인, 워커 노드, Fargate 등의 사용량에 따라 과금
	- EC2 스팟 인스턴스
		\- 비용을 절감하기 위해 EC2 스팟 인스턴스 사용

8. DevOps 및 CI/CD 통합
	 - CI/CD 파이프라인
		\- AWS CodePipeline, AWS CodeBuild, Jenkins 등과 통합하여 CI/CD 파이프라인 구축 
	 - GitOps 지원
		\- GitOps 방식으로 kubernetes 리소스를 관리하고 배포 가능


### 📌 구성 요소
---
1. EKS 클러스터
	- Amazon EKS는 클러스터를 생성하고 관리함
	- 클러스터는 Kubernetes의 모든 기능을 사용할 수 있도록 구성됨
2. 워커 노드
	- EC2 인스턴스 또는 Fargate를 사용하여 Kubernetes Pod를 실행하는 노드
	- 사용자는 워커 노드를 직접 관리하며, 이를 통해 애플리케이션 배포
3. 노드 그룹
	- 동일한 설정을 가진 워커 노드의 그룹
	- 노드 그룹을 통해 워커 노드를 쉽게 관리하고 확장가능


### 📌 EKS(Amazon Elastic Kubernetes Service) VS ECS(Amazon Elastic Container Service)
---
- AWS에서 제공하는 컨테이너 오케스트레이션 서비스

##### 🏷️ EKS(Amazon Elastic Kubernetes Service)
- AWS에서 관리하는 Kubernetes 서비스로, Kubernetes 클러스터를 쉽게 생성/운영할 수 있게 함
	`Kubernetes: 오픈소스 컨테이너 오케스트레이션 플랫폼으로, 많은 사용자와 확장성이 있는 애플리케이션 지원`

- 특징
	1. 표준화된 API
		\- Kubernetes API를 통해 컨테이너 오케스트레이션을 표준화할 수 있음
	2. 멀티 클라우드 및 온프레미스 지원
		\- EKS는 하이브리드 클라우드 환경과 멀티 클라우드 배포를 지원
	3. 커뮤니티 지원
		\- Kubernetes의 광범위한 커뮤니티와 도구 생태계를 활용할 수 있음

- 장점
	1. 확장성
		\- 대규모 애플리케이션을 위한 강력한 확장성 제공
	2. 유연성
		\- 다양한 워크로드와 애플리케이션에 맞게 유연하게 설정 가능
	3. 에코시스템
		\- Helm, Prometheus, Grafana 등 다양한 오픈 소스 도구와 통합 가능

- 단점
	1. 복잡성
		\- 초기 설정과 관리가 복잡할 수 있으며, Kubernetes에 대한 깊은 이해가 필요
	2. 비용
		\- EKS 클러스터와 관련된 추가 비용이 발생할 수 있음

- 사용 사례
	\- <span style="background:#d3f8b6">대규모</span> 애플리케이션 배포 및 관리
	\- 멀티 클라우드 및 하이브리드 클라우드 환경에서의 컨테이너 오케스트레이션
	\- Kubernetes 에코시스템을 활용한 애플리케이션 개발 및 배포

##### 🏷️ ECS(Amazon Elastic Container Service)
- AWS에서 제공하는 완전 관리형 컨테이너 오케스트레이션 서비스
- AWS Fargate와의 통합을 통해 서버리스 방식으로 컨테이너 실행

- 특징
	1. AWS 전용
		\- AWS에 최적화된 서비스로, AWS의 다른 서비스와 원활하게 통합됨
	2. Fargate 지원
		\- 서버리스 방식으로 컨테이너를 실행할 수 있는 옵션 제공
	3. 간편한 관리
		\- 클러스터와 인프라 관리를 단순화

- 장점
	1. 단순성
		\- 설정 및 관리가 간단하여 초보자도 쉽게 사용 가능
	2. 통합성
		\- AWS의 다른 서비스(S3, DynamoDB, RDS 등)와 원활하게 통합됨
	3. 비용 효율성
		\- Fargate를 통해 서버리스 방식으로 사용한 만큼만 비용 지불 가능

- 단점
	1. 유연성 부족
		\- Kubernetes만큼의 유연성과 확장성을 제공하지 않음
	2. 멀티 클라우드 지원 부족
		\- AWS 전용 서비스로, 멀티 클라우드 환경에서는 제한적

- 사용 사례
	\- <span style="background:#d3f8b6">간단한</span> 컨테이너 기반 애플리케이션 배포
	\- 서버리스 환경에서의 컨테이너 실행
	\- AWS 서비스와의 통합이 중요한 애플리케이션


### 📌 AWS Load Balancer Controller
---
![](https://i.imgur.com/EA0UVwU.png)

- Kubernetes 클러스터의 AWS Elastic Load Balancer를 관리
- 컨트롤러를 사용해 클러스터 앱을 인터넷에 노출할 수 있음
- 컨트롤러는 클러스터 Service 또는 Ingress 리소스를 가리키는 AWS 로드 밸런서를 프로비저닝
- 클러스터의 여러 Pod를 기리키는 단일 IP 주소 또는 DNS 이름을 생성
- Ingress 또는 Service리소스를 감시하고 이에 대한 응답으로 해당 AWS Elastic Load Balancing 리소스를 생성함
	\- Ingress: AWS Application Load Balancer(ALB) 생성
	\- Service: AWS Network Load Balancer(NLB) 생성

##### 🏷️ ALB 생성 과정
**단계 1: AWS Load Balancer Controller 설치**
1. IAM 정책 생성
	\- AWS Load Balancer Controller가 필요한 IAM 정책 생성
``` sh
curl -o iam_policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/install/iam_policy.json
aws iam create-policy \
	--policy-name AWSLoadBalancerControllerIAMPolicy \
	--policy-document file://iam_policy.json
```

2. IAM 역할 생성 및 연결
	\- EKS 클러스터에 대한 IAM 역할을 생성하고 연결
``` sh
eksctl create iamserviceaccount \
	--cluster <cluster-name> \
	--namespace kube-system \
	--name aws-load-balancer-controller \
	--attach-policy-arn arn:aws:iam::<account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
	--approve
```

3. Helm 차트 추가 및 업데이트
	\- AWS Load Balancer Controller를 설치하기 위한 Helm 차트를 추가하고 업데이트
	`(Helm: Kubernetes 패키지 관리를 도와줌. (Node.js의 npm같은 역할))`
``` sh
helm repo add eks https://aws.github.io/eks-charts
helm repo update
```

4. Helm을 사용하여 Controller 설치
``` sh
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=<cluster-name> -- set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
```

**단계 2: Ingress 리소스 설정**
1. Ingress 리소스 정의
	\- ingress.yaml 파일을 생성하고 다음 내용 추가
``` yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
	name: example-ingress
	namespace: default
	annotations:
		kubernetes.io/ingress.class: alb
		alb.ingress.kubernetes.io/scheme: internet-facing
		alb.ingress.kubernetes.io/target-type: ip
spec:
	rules:
		- http:
			paths:
				-path: /*
				 pathType: ImplementationSpecific
				 backend:
					 service:
						 name: example-service
						 port:
							 number: 80
```

2. Ingress 리소스 적용
``` sh
kubectl apply -f alb-ingress.yaml
```

**단계 3: 확인 및 디버깅**
1. ALB 상태 확인
``` sh
kubectl get ingress example-ingress -n default
```

2. 디버깅
	\- 문제 발생시 AWS Load Balancer Controller 로그를 확인해 문제 해결
``` sh
kubectl logs -n kube-system deployment/aws-load-balancer-controller
```