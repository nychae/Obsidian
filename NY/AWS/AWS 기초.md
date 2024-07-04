
### 온프라미스와 AWS 용어 비교
---
- 방화벽 = 보안그룹
- ACL = NACL
- 관리자 권한 = IAM
- L4, 로드 밸런서 = ELB(Elastic Load Balancing), 탄력적인 로드 밸런서, ALB, NLB, GWLB, CLB(x)
- 네트워크 = VPC(Virtual Private Cloud)
- 서버 = EC2(Elastic Compute Cloud)
- NAS = EFS(Elastic File System)
- 디스크 = EBS(Elastic Block Store)
- DB = RDS 


### AWS 기본 구성도
---
![[Pasted image 20240701153806.png]]
1. Route53이 도메인에 대한 응답
2. 서버 EC2로 접속
3. 로컬 디스크인 EBS에서 데이터 읽음
4. 비즈니스적으로 요청한 데이터가 있는 경우 DB에서 읽어옴
5. S3(객체 스토리지 서비스)에서 이미지 읽어옴

### AWS 인프라
---
1) 리전(Region)
	- 리전들이 모여 AWS 서비스를 구성함
	- AWS는 물리적으로 떨어진 지역에 여러 개의 클라우드 인프라를 운영하는데, 이 지역을 리전(Region)이라고 함
	- 서울 리전(ap-northeast-2), 미국 버지니아 북부 리전(us-east-1) 등과 같이 국가나 지역을 식별할 수 있는 이름을 붙임
	- 네트워크 속도를 위해 리전을 여러 곳에 위치시킴 (멀리 떨어진 서버에 접속하기 위해서는 그만큼 경유하는 라우터 개수가 많아져 속도가 느려지기 때문)
	- 각 리전은 물리적으로 완전히 분리되어 있으며, AWS 콘솔 상에서도 완전히 다른 리소스로 구분함

2) 가용 영역(Availability Zone)
	- 가용영역들이 모여 리전을 구성함
		  한 리전 아래 여러 개의 데이터 센서가 운영되고 있다는 의미.
			가용 영역을 여러 곳에 두고 운영한다면, 하나의 가용 영역에 문제가 발생해도 단순한 변경만으로 가용 영역 운영 가능
			 => 가동률 or 가용성(Availability)
	- 데이터 센터로서 실제 물리적으로는 완전히 독립되어 있지만, AWS 콘솔에서 리소스별로 구분하지는 않음

3) 에지 로케이션(Edge Location)
	-  CDN(Content Delivery Network)을 이루는 캐시 서버로 CDN들의 여러 서비스들을 가장 빠른 속도로 제공(캐싱)하기 위한 거점.
			`CDN(Content Delivery Network)`
			` - 지리적으로 분산된 서버들을 연결한 네트워크로서 웹 컨텐츠의 복사본을 사용자에 가까운 곳에 두거나 동적 컨텐츠(ex: 라이브 비디오 피드)의 전달을 활성화하여 웹 성능 및 속도를 향상할 수 있게 하는 것 `
