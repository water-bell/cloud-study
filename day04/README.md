# Day 4 - Nginx 웹 서버 기초

## 1. Nginx 설치

```bash
sudo apt update
sudo apt install nginx -y
```

Nginx는 대표적인 웹 서버이자 Reverse Proxy 서버이다.

---

## 2. Nginx 서비스 확인

```bash
sudo systemctl status nginx
```

* `active (running)` → Nginx가 정상적으로 실행 중
* `enabled` → 시스템 부팅 시 자동으로 실행되도록 설정

---

## 3. Nginx 프로세스 확인

```bash
ps aux | grep nginx
```

Nginx는 Master Process와 여러 Worker Process로 구성된다.

```text
Nginx
 ├── Master Process
 └── Worker Processes
```

* Master Process → Worker Process 관리
* Worker Process → 실제 HTTP 요청 처리
* Master는 `root`, Worker는 일반적으로 `www-data` 권한으로 실행

---

## 4. Nginx 포트 확인

```bash
sudo ss -tulpn | grep :80
```

Nginx가 TCP 80번 포트에서 HTTP 요청을 기다리고 있는 것을 확인했다.

```text
TCP :80
   ↓
Nginx
   ↓
HTTP 요청 처리
```

* HTTP 기본 포트 → `80`
* HTTPS 기본 포트 → `443`

---

## 5. curl로 웹 서버 접속

```bash
curl http://localhost
```

`curl`을 사용하여 실행 중인 Nginx 웹 서버에 HTTP 요청을 보내고 HTML 응답을 확인했다.

```text
curl
 ↓
127.0.0.1:80
 ↓
Nginx
 ↓
HTML 응답
```

---

## 6. Chrome에서 확인

Chrome에서:

```text
http://localhost
```

로 접속하여 Nginx 기본 페이지가 정상적으로 표시되는 것을 확인했다.

---

## 7. HTML 직접 수정

Nginx 기본 HTML 파일:

```text
/var/www/html/index.nginx-debian.html
```

수정:

```bash
sudo nano /var/www/html/index.nginx-debian.html
```

직접 작성한 HTML을 저장한 후 Chrome을 새로고침하여 변경된 페이지를 확인했다.

---

## 8. 전체 흐름

```text
Chrome / curl
      ↓
http://localhost
      ↓
127.0.0.1:80
      ↓
Nginx
      ↓
/var/www/html/index.nginx-debian.html
      ↓
HTML 응답
      ↓
Chrome에 웹 페이지 표시
```

## 핵심 정리

> Nginx = 웹 서버
> Master Process = Worker 관리
> Worker Process = HTTP 요청 처리
> Port 80 = HTTP 기본 포트
> `/var/www/html` = Nginx 웹 파일이 위치하는 디렉터리
> `curl` = 터미널에서 HTTP/HTTPS 요청을 보내는 도구
> `systemctl` = Linux 서비스 관리
> `ss` = 포트 및 네트워크 연결 확인
