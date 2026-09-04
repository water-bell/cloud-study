# Day 3 - Linux 네트워크 기초

## 1. IP와 네트워크 인터페이스

```bash
ip addr
```

* `lo` : Loopback 인터페이스

  * `127.0.0.1`
  * 자기 자신과 통신할 때 사용
* `eth0` : 실제 네트워크 인터페이스

  * WSL2 환경에서 Ubuntu의 네트워크 인터페이스
* IP 주소는 네트워크에서 호스트를 식별하기 위해 사용

---

## 2. 포트와 프로세스 확인

```bash
ss -tuln
sudo ss -tulpn
```

* `-t` : TCP
* `-u` : UDP
* `-l` : Listening
* `-n` : 숫자로 표시
* `-p` : 해당 포트를 사용하는 프로세스 표시

주요 포트:

| 포트   | 용도         |
| ---- | ---------- |
| 22   | SSH        |
| 53   | DNS        |
| 80   | HTTP       |
| 443  | HTTPS      |
| 3306 | MySQL      |
| 5432 | PostgreSQL |

`sshd`는 SSH 서버 프로세스이며 TCP 22번 포트에서 연결을 기다린다.

---

## 3. Ping

```bash
ping 127.0.0.1
ping 8.8.8.8
ping google.com
```

* `ping`은 ICMP를 사용하여 네트워크 연결 상태를 확인한다.
* TCP/UDP와 달리 ICMP에는 포트 번호가 없다.
* `ping google.com`에서는 먼저 DNS를 통해 도메인을 IP로 변환한다.

```text
google.com
    ↓
   DNS
    ↓
 IP 주소
    ↓
  ICMP
    ↓
Google 서버
```

---

## 4. curl

```bash
curl https://google.com
curl -I https://google.com
curl -I http://google.com
```

`curl`을 이용하면 브라우저처럼 웹 서버에 HTTP/HTTPS 요청을 보낼 수 있다.

`-I` 옵션은 HTTP 응답의 헤더만 확인한다.

HTTP 요청:

```text
http://google.com
        ↓
     TCP :80
        ↓
      HTTP
```

HTTPS 요청:

```text
https://google.com
        ↓
     TCP :443
        ↓
  TLS Handshake
        ↓
      HTTP
```

---

## 5. HTTP와 HTTPS

* HTTP : 웹 통신 프로토콜
* HTTPS : HTTP + TLS
* HTTP 기본 포트 : `80`
* HTTPS 기본 포트 : `443`

URL의 `http://`, `https://`가 사용할 통신 방식을 결정하고, 포트 번호는 연결할 서비스의 위치를 지정한다.

---

## 6. TLS Handshake

TLS는 HTTPS 통신을 안전하게 만들어주는 보안 기술이다.

간단한 과정:

```text
Client                  Server
  │                       │
  │ ClientHello           │
  ├──────────────────────>│
  │                       │
  │ ServerHello + 인증서  │
  │<──────────────────────┤
  │                       │
  │ 키 교환 및 검증       │
  ├──────────────────────>│
  │                       │
  │ 암호화 통신 시작 🔐   │
  │<─────────────────────>│
```

TLS는 다음을 제공한다.

* 데이터 암호화
* 서버 인증
* 데이터 변조 방지

---

## 7. 오늘의 핵심 흐름

```text
https://google.com
        ↓
       DNS
        ↓
   IP 주소 획득
        ↓
    TCP :443
        ↓
 TLS Handshake
        ↓
    HTTP 요청
        ↓
   웹 서버 응답
```

### 핵심 정리

> IP = 어떤 호스트인가
> Port = 어떤 서비스인가
> TCP/UDP = 어떻게 데이터를 전달하는가
> DNS = 도메인을 IP로 변환
> HTTP = 웹 통신
> HTTPS = HTTP + TLS
> TLS = 안전한 암호화 통신을 위한 기술
