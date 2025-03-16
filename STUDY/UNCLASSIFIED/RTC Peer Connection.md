# RTC Peer Connection

- WebRTC (Web Real-Time Communication) API의 핵심 객체
- 브라우저 간 P2P(Peer-to-Peer) 연결을 설정하고 오디오, 비디오, 데이터 스트림을 송수신하는 데 사용됨
- 영상통화, P2P 파일 공유, 게임 채팅 같은 실시간 애플리케이션에서 많이 사용됨

#### ✅ 주요 기능
1. P2P 연결 관리
	- 브라우저 간 직접 연결 설정
2. 오디오/비디오 전송
	- 스트리밍 데이터 송수신
3. ICE(Interactive Connectivity Establishment) 후보 처리
	- NAT 및 방화벽을 넘어 연결 시도
4. 데이터 채널 전송
	- 파일, 텍스트 같은 비디오/오디오 이외의 데이터 전송 지원

#### ✅ RTCPeerConnection 기본 흐름
📌 **WebRTC 연결 과정**
1) 각 클라이언트가 RTCPeerConnection을 생성
2) 로컬 미디어(카메라, 마이크) 스트림 추가
3) SDP(Session Description Protocal) 교환 (offer/answer)
4) ICE 후보 교환 (네트워크 라우팅 정보 전달)
5) P2P 연결 확립 후 미디어/데이터 송수신

📌 **P2P 연결 설정 기본 코드**

1) A 클라이언트 (발신자)
``` javascript
const peerConnection = new RTCPeerConnection();

// 1. 로컬 스트림 추가 (카메라, 마이크)
navigator.mdeiaDevices.getUserMedis({ video: true, audio: true })
	.then((stream) => {
		stream.getTracks().forEach((track) => {
			peerConnection.addTrack(track, stream);
		});
	});

// 2. SDP Offer 생성 및 전송
peerConnection.createOffer()
	.then((offer) => {
		 return peerConnection.setLocalDescription(offer);
	})
	.then(() => {
		sendToServer('offer', peerConnection.localDescription);
	});

// 3. ICE 후보 생성 시 서버로 전송
peerConnection.onicecandidate = (event) => {
	if(event.candidate) {
		sendToServer('ice-candidate', event.candidate);
	}
};
```

2) B 클라이언트 (수신자)
``` javascript
const peerConnection = new RTCPeerConnection();

// 1. 로컬 스트림 추가
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
	.then((stream) => {
		stream.getTracks().forEach((track) => {
			peerConnection.addTrack(track, stream);
		});
	});

// 2. SDP Offer 수신 후 Answer 생성
socket.on('offer', (offer) => {
	peerConnection.setRemoteDescription(new RTCSessionDescription(offer))
		.then(() => peerConnection.createAnswer())
		.then((answer) => {
			return peerConnection.setLocalDescription(answer);
		})
		.then(() => {
			sendToServer('answer', peerConnection.localDescription);
		});
});

// 3. ICE 후보 수신 후 추가
socket.on('ice-candidate', (candidate) => {
	peerConnection.addIceCandidate(new RTCIceCandidate(candidate));
})
```


#### ✅ 핵심 개념
**1.SDP (Session Description Protocol)**
- WebRTC 연결에서 오디오/비디오 스트림을 설명하는 프로토콜
	=> 쉽게 말하면, 웹 브라우저가 서로 어떻게 통신해야하는지 정의하는 정보
- RTCPeerConnection.createOffer() 또는 createAnswer()를 호출하면 SDP가 생성됨
- WebRTC에서 브라우저 간 코덱, IP 주소, 포트, 미디어 정보 등을 교환하는 데 사용

**2. ICE (Interactive Connectivity Establishment)**
- NAT 및 방화벽을 넘어 P2P 연결을 설정하는 기술
	`webRTC에서는 브라우저끼리 P2P 연결을 맺어야 하는데, 네트워크 환경이 다를 경우 (예를 들어, 둘다 NAT(Network Address Translation)이나 방화벽 뒤에 있는 경우) 직접 연결이 어려울 수 있기때문에 이것을 해결하기 위해 ICE를 사용`
- 가능한 모든 네트워크 경로(후보)를 찾아 연결을 시도하는 과정
- RTCPeerConnection.onicecandidate 이벤트에서 ICE 후보 (네트워크 정보)가 생성됨
- ICE 후보를 상대방에게 전달해야 WebRTC가 정상적으로 연결됨

	📌 ICE 후보 (ICE Candidate)
	- ICE 과정에서 브라우저는 여러 개의 ICE 후보(상대방과 연결할 수 있는 IP 주소와 포트 정보)를 생성함.
	- RTCPeerConnection.onicecandidate 이벤트를 통해 생성됨

	- 종류
		1) Host Candidate (로컬 IP)
			- 사용자의 로컬 네트워크 IP 주소를 사용
			- 같은 네트워크에 있을 경우 직접 연결 가능
		2) STUN Candidate (공인 IP)
			- STUN 서버를 통해 공인 IP 주소를 조회해서 후보로 추가
			- 서로 다른 네트워크에 있어도 연결 가능할 수도 있음
		3) TURN Candidate (Relay)
			- TURN 서버를 통해 중계 서버를 거쳐 통신
			- 두 브라우저가 직접 연결할 수 없을 때 사용되며, 속도가 느리고 비용 발생

**3. STUN & TURN 서버**
- ICE가 제대로 동작하려면, 브라우저가 서로 연결할 수 있는 네트워크 정보를 찾아야 하는데, 이때 사용하는 것이 STUN/TURN 서버

📌 STURN (Session Traversal Utilities for NAT)
	- 브라우저가 자신의 공인 IP 주소를 확인하는 역할
	- 예를 들어, 공유기 뒤에서 실행 중인 브라우저가 자신의 외부 IP를 알아낼 때 사용
	- STUN만으로는 모든 네트워크에서 P2P연결이 안될 수도 있음
``` javascript
// Google의 무료 STUN 서버를 사용해서 ICE 후보를 찾는 방법
const peerConnection = new RTCPeerConnection({
	iceServers: [{ urls: 'stun:stun.l.google.com:19302' }]
});
```

📌 TURN (Traversal Using Relays around NAT)
	- P2P 연결이 실패할 경우, TURN 서버가 중계 서버 역할을 해서 연결을 유지함
	- 모든 데이터가 TURN 서버를 통해 전달되므로 비용이 발생하고 속도가 느려질 수 있음
	- 일반적으로 WebRTC에서는 STUN으로 연결을 시도하고, 실패하면 TURN을 사용하는 방식
```javascript
const peerConnection = new RTCPeerConnection({
	iceServers: [
		{ urls: 'stun:stun.l.google.com:19302' }, // STUN 서버
		{ urls: 'turn:turn.example.com', username: 'user', credential: 'pass' } // TURN 서버
	]
})
```