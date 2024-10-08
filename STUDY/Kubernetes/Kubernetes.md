# Kubernetes
- 컨테이너화된 애플리케이션의 배포, 관리, 확장, 자동화 등을 지원하는 오픈소스 플랫폼
- 컨테이너 오케스트레이션을 위한 표준 플랫폼으로 널리 사용되고 있음
	` 컨테이너 오케스트레이션: 컨테이너화된 애플리케이션의 배포, 관리, 확장 및 유지 보수를 자동화하는 것`

### 📌 특징
---
1. 자동화된 배포 및 롤백
	\- 컨테이너 애플리케이션을 배포하고, 업그레이드하며, 롤백할 수 있는 기능을 자동화 함
	\- 새로운 버전을 배포할 때, 기존 버전을 대체하거나 롤백할 수 있음

2. 서비스 디스커버리 및 로드 밸런싱
	\- 클러스터 내에서 서비스 디스커버리와 로드 밸런싱을 지원
	\- 서비스는 클러스터 내의 다른 서비스나 외부 트래픽에 대한 접근을 쉽게 제공

3. 자동화된 스케일링
	\- 애플리케이션의 부하를 모니터링하고, 필요에 따라 컨테이너의 수를 자동으로 조절
	\- Horizontal Pod Autoscaler(HPA)와 Vertical Pod Autoscaler(VPA)를 통해 오토 스케일링을 지원

4. 자체 치유(Self-Healing)
	\- 컨테이너가 실패하거나 노드가 다운되면, 자동으로 새로운 컨테이너를 시작하거나 다른 노드에서 복구를 시도

5. Stateful 및 Stateless  애플리케이션 지원
	\- Stateful 애플리케이션을 위한 StatefulSet, Stateless 애플리케이션을 위한 Deployment 등의 리소스를 지원
	\- 데이터 저장 및 지속성을 관리할 수 있음

6. 다양한 런타임 지원
	\- Docker를 포함한 다양한 컨테이너 런타임을 지원
	\- CRI-O, containerd 등 다양한 컨테이너 런타임 사용 가능

7. 자원 관리 및 할당
	\- 자원 요청 및 제한을 설정하여 컨테이너가 사용하는 CPU와 메모리를 제어할 수 있기때문에 이를 통해 자원을 효율적으로 관리/조절 가능

8. 서비스 메시와 네트워크 정책
	\- 서비스 메시를 통해 마이크로서비스 간의 통신 관리 가능
	\- 네트워크 정책을 사용해 클러스터 내의 트래픽 흐름을 제어하고 보안을 강화할 수 있음

9. 컨테이너 배포 및 관리
	\- 컨테이너를 클러스터 내에서 배포하고, 관리하며, 상태를 유지하는데 이를 통해 복잡한 애플리케이션 아키텍처를 간편하게 관리 가능

10. 커스터마이징 및 확장성
	\- Custom Resource Definitions(CRDs)와 Operators를 통해 클러스터를 커스터마이즈하고, 특정 애플리케이션의 요구 사항에 맞게 확장 가능


### 📌 구성요소
---
![](https://i.imgur.com/MxBk4mV.png)

##### 🏷️ Node
- 정의
	\- 클러스터를 구성하는 물리적 또는 가상 머신
	\- Kubernetes는 노드에서 컨테이너를 실행
- 역할
	\- 각 노드는 컨테이너화된 애플리케이션의 실행 환경을 제공
- 종류
  1) Master Node
	  \- 클러스터의 중앙 제어 지점으로서, 클러스터의 관리 및 조정을 전담
	  \- 일반적으로 마스터 노드는 단일 지점 실패의 위험이 있으므로, 고가용성(HA)을 위해 다중 마스터 노드 구성 가능
  1) Worker Node
	  \- 클러스터의 실제 작업을 수행하며, 애플리케이션의 컨테이너를 실행하고 관리
	  \- 일반적으로 여러 개의 워커 노드가 클러스터에 배포되어 애플리케이션의 높은 가용성과 확장성을 지원함

		\- 구성요소
			1. **Kubelet**: 워커 노드에서 실행되는 에이전트로, 노드에서 실행 중인 컨테이너와 Pod의 상태를 모니터링하고, 클러스터의 desired state를 유지함
			2. **Kube-Proxy**: 네트워크 프록시로, 클러스터 내의 서비스와 Pod간의 네트워크 통신을 관리함. Service를 통해 Pod에 대한 접근을 라우팅
			3. **Container Runtime**: 컨테이너를 실행하는 소프트웨어로 Docker, containerd, CRI-O와 같은 다양한 런타임이 사용될 수 있음
			4. **Pod**: 워커 노드에서 실행되는 컨테이너의 집합으로 Pod는 컨테이너 간의 네트워킹 및 스토리지 공유를 제공함

##### 🏷️ Pod
- 정의
	\- 하나 이상의 컨테이너가 함께 실행되는 Kubernetes의 가장 작은 배포 단위
- 역할
	\- 컨테이너들 간의 네트워킹, 스토리지, 라이프사이클을 공유
	\- 같은 Pod 내의 컨테이너는 로컬 네트워크와 파일 시스템을 공유

##### 🏷️ Service
- 정의
	\- Pod 집합에 대해 ==일관된 접근점을 제공==하는 쿠버네티스 리소스
- 역할
	\- 클러스터 내의 Pod에 대해 안정적인 네트워크 접근을 제공하고, 로드밸런싱 수행
	\- DNS 이름을 통해 Pod에 접근할 수 있게 해줌

##### 🏷️ Deployment
- 정의
	\- 애플리케이션의 ==배포와 관리를 담당==하는 쿠버네티스 리소스
- 역할
	\- 특정 상태를 유지하도록 Pod를 관리
	\- 롤링 업데이트, 롤백, 스케일링 등을 지원
	
##### 🏷️ Ingress
- 정의
	\- 클러스터 내 ==서비스에 대한 외부 접근을 관리==하는 API 객체
- 역할
	\- HTTP 및 HTTPS 요청을 라우팅하고, 경로 기반의 트래픽 관리와 SSL 종료를 지원

##### 🏷️ ReplicaSet
- 정의
	\- 특정 수의 Pod 복제본을 유지하는 쿠버네티스 리소스
- 역할
	\- Deployment와 함께 작동하여 Pod의 복제본을 유지`(Deployment는 내부적으로 ReplicaSet을 사용하여 Pod의 상태를 관리함)`

##### 🏷️ StatefulSet
- 정의
	\- 상태가 있는 애플리케이션을 위한 쿠버네티스 리소스
- 역할
	\- Pod에 고유한 정체성과 안정적인 네트워크 식별자를 제공하며, Pod의 순서 및 안정적인 저장소를 지원

##### 🏷️ Job
- 정의
	\- 일회성 작업을 실행하는 쿠버네티스 리소스
- 역할
	\- 작업이 성공적으로 완료될 때까지 Pod를 실행하고, 완료 후 Pod를 종료함

##### 🏷️ CronJob
- 정의
	\- 애플리케이션 설정 정보를 저장하는 쿠버네티스 리소스
- 역할
	\- 주기적으로 특정 작업을 실행할 수 있도록 스케줄링

##### 🏷️ ConfigMap
- 정의
	\- 애플리케이션 설정 정보를 저장하는 쿠버네티스 리소스
- 역할
	\- 설정 데이터를 관리하고, Pod에서 환경 변수나 파일로 사용할 수 있도록 제공

##### 🏷️ Secret
- 정의
	\- 민감한 데이터를 안전하게 저장하는 쿠버네티스 리소스
- 역할
	\- 암호, 토큰, SSH 키 등의 민감한 정보를 관리하고, Pod에서 사용할 수 있도록 제공

##### 🏷️ Namespace
- 정의
	\- 클러스터 내의 자원을 논리적으로 구분하는 쿠버네티스 리소스
- 역할
	\- 클러스터의 자원을 격리하여 여러 프로젝트나 팀이 동일한 클러스터를 공유할 수 있도록 함

##### 🏷️ Volume
- 정의
	\- Pod에 대한 저장 공간을 제공하는 쿠버네티스 리소스
- 역할
	\- Pod에서 사용할 수 있는 다양한 형태의 스토리지 솔루션을 지원하며, 컨테이너의 수명과는 별개로 데이터의 지속성을 보장


### 📌 구성요소간의 관계
---
1) Node와 Pod
	\- Pod는 Node에서 실행
	\- 각 Node는 여러 Pod를 호스팅할 수 있으며, Pod는 Node의 자원을 사용해 실행됨

2) Pod와 Service
	\- Service는 Pod의 집합에 접근하기 위한 안정적인 인터페이스 제공
	\- Service는 내부적으로 Pod의 IP를 관리하고, 로드밸런싱 수행

3) Deployment와 ReplicaSet
	\- Deployment는 ReplicaSet을 생성하여 관리
	\- ReplicaSet은 지정된 수의 Pod 복제본을 유지하며, Deployment는 이를 통해 애플리케이션의 상태를 관리함

4) StatefulSet과 Pod
	\- StatefulSet은 Pod를 순차적으로 생성하고 관리하며, 각 Pod에 고유한 정체성을 부여함
	\- StatefulSet은 안정적인 저장소와 네트워크 ID를 제공

5) Job과 Pod
	\- Job은 완료될 때까지 Pod를 실행
	\- Job은 특정 작업이 완료될 때까지 Pod를 관리하고, 성공적으로 완료된 후에는 Pod를 종료

6) ConfigMap/Secret과 Pod
	\- ConfigMap과 Secret은 Pod의 환경 변수나 파일로 주입되어 애플리케이션 설정이나 민감한 정보를 제공

7) Namespace와 기타 리소스
	\- Namespace는 리소스(ex. Pod, Service, Deployment 등)를 논리적으로 구분함
	\- 여러 Namespace를 사용하여 클러스터 내의 자원을 조직화할 수 있음

8) Ingress와 Service
	\- Ingress는 외부의 HTTP/HTTPS 요청을 Service로 라우팅
	\- Ingress는 Service를 통해 Pod에 접근할 수 있도록 설정함
