## WebRTC (Web Real Time Communication)

- 브라우저와 모바일 애플리케이션에서 별도의 소프트웨어 없이 음성, 영상 미디어, 텍스트, 파일과 같은 데이터들을 실시간 통신(RTC)으로 주고 받을 수 있게 해주는 기술

**특징**

- 플러그인 불필요 (브라우저 기본 API 지원)
- P2P 기반 (중앙 서버 거치지 않고 직접 연결)
- NAT/방화벽 우회 지원 (ICE/STUN/TURN 사용)
- 멀티미디어 처리 + 전송 모두 브라우저 내부에서 가능

## 동작 흐름 (고수준 개념)

WebRTC 연결이 성립되기까지 4단계로 요약할 수 있어요.

**시그널링(Signaling)**

- **목적**: 두 브라우저가 서로 연결하기 전에 **네트워크 정보와 미디어 정보(SDP)**를 교환하는 단계.
- **방법**: WebRTC 표준에 정의된 건 없음 → WebSocket, Socket.io, HTTP POST 등 아무거나 가능.
- 교환하는 정보
    - SDP(Session Description Protocol): 코덱, 미디어 형식, 전송 방식 등 세션 정보
    - ICE 후보(IP 주소, 포트 정보)

### 구조 다이어그램

```bash
[Peer A] <----P2P----> [Peer B]
   ↑                      ↑
   |                      |
   └--- Signaling Server --┘
            (WebSocket, HTTP 등)
```

- **Signaling Server**: 연결 성립 전에만 사용 (실제 미디어 데이터는 거치지 않음)
- **STUN/TURN 서버**: NAT 방화벽 환경에서 경로 설정 보조

## CS 관점에서 중요 포인트

- **P2P vs Client-Server**
    - WebRTC는 기본적으로 P2P이지만, 시그널링·TURN 서버는 필수적인 중앙 요소
- **네트워크 계층**
    - TCP보단 UDP 기반 (지연 최소화)
    - 보안: DTLS + SRTP로 암호화
- **코덱**
    - 오디오: Opus (대부분 브라우저 기본 지원)
    - 비디오: VP8/VP9/H.264
- **NAT Traversal**
    - 홈·회사 네트워크에서 NAT/방화벽 때문에 직접 연결 어려움 → STUN/TURN 사용
- **확장성**
    - 1:1 통화는 P2P 가능
    - 그룹 통화는 SFU(Selective Forwarding Unit)나 MCU(Multipoint Control Unit) 사용

## OpenVidu를 이용한 WebRTC 구현

OpenVidu는 **WebRTC 기반의 오픈소스 미디어 서버**

기본 WebRTC는 P2P 방식이라 **1:N / N:N 그룹 통화, 방송, 녹화, 권한 관리** 등을 구현하려면 직접 SFU·MCU 서버를 구축해야 하는데, OpenVidu가 이 부분을 쉽게 제공

- **구성**
    - **OpenVidu Server**: 세션/연결/토큰 관리
    - **Kurento Media Server(KMS)**: 미디어 처리 (SFU, 녹화, 필터 등)
    - **OpenVidu Browser**: 클라이언트 SDK (JS/TS)
    - **OpenVidu REST API**: 서버 애플리케이션에서 세션, 토큰 발급

---

## 2. 기본 동작 구조

```
[Client 1] ----\
[Client 2] ----> [OpenVidu Server] --> [Kurento Media Server] --> 스트림 라우팅
[Client 3] ----/
```

- **브라우저 ↔ OpenVidu Server**: WebRTC 연결
- **OpenVidu Server ↔ App Server**: REST API로 세션·토큰 생성
- **Kurento**: SFU 방식으로 미디어 스트림을 중계 (P2P가 아니라 서버 중계)

---

## 3. 흐름 (스트리밍 구현 절차)

1. **App Server**에서 OpenVidu REST API 호출 → 세션(Session) 생성
2. 세션 ID로 토큰(Token) 발급
3. *클라이언트(React)**에서 토큰으로 OpenVidu 서버에 연결
4. 로컬 미디어 캡처(getUserMedia) 후 publish
5. 다른 참가자들이 해당 스트림을 subscribe

---

## 4. 서버 설정 (Docker 기반)

OpenVidu는 Docker Compose로 손쉽게 실행 가능.

서버가 실행되면:

https://your-server-ip:4443 접속 → OpenVidu Dashboard 확인 가능.

---

## 5. 서버-앱 연동 예시

App 서버(Node.js/NestJS 기준)에서 토큰 발급 API 구현:

---

## 6. React + TypeScript 클라이언트 예시

```tsx
import { OpenVidu } from 'openvidu-browser';
import { useEffect, useState } from 'react';
import axios from 'axios';

const OPENVIDU_SERVER_URL = 'https://your-server-ip:4443';
const SESSION_ID = 'sessionA';

export default function VideoRoom() {
  const [session, setSession] = useState<any>(null);
  const [publisher, setPublisher] = useState<any>(null);
  const [subscribers, setSubscribers] = useState<any[]>([]);

  useEffect(() => {
    joinSession();
  }, []);

  const joinSession = async () => {
    const OV = new OpenVidu();
    const mySession = OV.initSession();

    // 원격 스트림 수신
    mySession.on('streamCreated', (event: any) => {
      const subscriber = mySession.subscribe(event.stream, undefined);
      setSubscribers((prev) => [...prev, subscriber]);
    });

    // 세션 연결
    const token = await getTokenFromServer();
    await mySession.connect(token, { clientData: 'User1' });

    // 로컬 미디어 발행
    const pub = await OV.initPublisherAsync(undefined, {
      audioSource: undefined,
      videoSource: undefined,
      publishAudio: true,
      publishVideo: true,
      resolution: '640x480',
      frameRate: 30
    });

    mySession.publish(pub);

    setSession(mySession);
    setPublisher(pub);
  };

  const getTokenFromServer = async () => {
    const res = await axios.post('/api/get-token', { sessionId: SESSION_ID });
    return res.data.token;
  };

  return (
    <div>
      <h2>OpenVidu Video Room</h2>
      <div id="local-video" ref={(el) => el && publisher?.addVideoElement(el)}></div>
      {subscribers.map((sub, i) => (
        <div key={i} ref={(el) => el && sub.addVideoElement(el)}></div>
      ))}
    </div>
  );
}
```

---

## 7. OpenVidu vs 순수 WebRTC 비교

| 항목 | WebRTC 직접 구현 | OpenVidu |
| --- | --- | --- |
| 시그널링 서버 | 직접 구현 필요 | 기본 제공 |
| NAT Traversal | STUN/TURN 직접 세팅 | 내장 지원 |
| 그룹 통화 | SFU 직접 구현 필요 | 바로 사용 가능 |
| 녹화, 방송 | 별도 개발 필요 | 옵션으로 제공 |
| 난이도 | 높음 | 낮음 |

---

### 참고 사이트

[[WebRTC] React+TypeScript+WebRTC 개념정리 + 구현하기](https://juyami.tistory.com/120)

[WebRTC API - Web API | MDN](https://developer.mozilla.org/ko/docs/Web/API/WebRTC_API)