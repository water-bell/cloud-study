# Day 5 - Firewall & External Access

## 1. Firewall 확인

```bash
sudo ufw status

UFW는 Ubuntu에서 사용하는 방화벽 관리 도구이다.

Status: inactive

현재 UFW가 비활성화되어 있는 것을 확인했다.

## 2. 현재 열려 있는 포트 확인
sudo ss -tulpn | grep -E ':22|:80'

현재 SSH와 Nginx가 각각 22번, 80번 포트에서 요청을 기다리고 있는 것을 확인했다.

22 → SSH → sshd
80 → HTTP → Nginx
22 → SSH
80 → HTTP
0.0.0.0:80 → 모든 IPv4 인터페이스에서 접속 대기
[::]:80 → 모든 IPv6 인터페이스에서 접속 대기

## 3. 네트워크 인터페이스 확인
ip addr show eth0

WSL2의 eth0 인터페이스에서 IPv4 주소를 확인했다.

172.31.18.66/20
lo → Loopback 인터페이스
127.0.0.1 → 자기 자신
eth0 → 실제 네트워크 통신에 사용하는 인터페이스
172.31.18.66 → WSL2의 현재 IP

## 4. WSL2 IP로 Nginx 접속
curl http://127.0.0.1
curl http://172.31.18.66

두 주소 모두 Nginx의 HTML 페이지가 정상적으로 반환되는 것을 확인했다.

127.0.0.1
   ↓
WSL2
   ↓
Nginx :80

172.31.18.66
   ↓
WSL2 eth0
   ↓
Nginx :80

## 5. Windows에서 WSL2 웹 서버 접속

Windows Chrome에서:

http://172.31.18.66

으로 접속하여 Nginx 웹 페이지가 정상적으로 표시되는 것을 확인했다.

이를 통해 Windows → WSL2 → Nginx로 HTTP 요청이 전달되는 것을 확인했다.

## 6. LISTEN과 외부 접근의 차이

Nginx가 80번 포트에서 LISTEN하고 있다고 해서 인터넷에서 바로 접속할 수 있는 것은 아니다.

외부 요청
   ↓
Firewall
   ↓
서버
   ↓
Nginx :80
Nginx → 애플리케이션
Port → 서비스가 사용하는 통신 창구
Firewall → 해당 포트에 대한 접근 허용/차단
## 7. AWS Security Group

AWS EC2에서는 Security Group을 통해 네트워크 접근을 제어한다.

예시:

HTTP
TCP 80
0.0.0.0/0

SSH
TCP 22
내 IP
HTTP 80 → 웹 서버 접속 허용
SSH 22 → 관리 목적의 SSH 접속 허용
SSH는 가능한 한 모든 IP(0.0.0.0/0)에 공개하지 않는 것이 좋다.

## 전체 흐름
Internet
   ↓
AWS Security Group
   ↓
EC2
   ↓
Ubuntu Firewall
   ↓
Nginx :80
   ↓
HTML 응답

## 핵심 정리

LISTEN = 해당 포트에서 서비스가 요청을 기다리고 있음
Firewall = 네트워크 접근을 허용하거나 차단
UFW = Ubuntu의 방화벽 관리 도구
22 = SSH
80 = HTTP
eth0 = 네트워크 인터페이스
Security Group = AWS에서 EC2의 네트워크 접근을 제어
LISTEN하고 있다고 해서 외부 접근이 반드시 가능한 것은 아님
