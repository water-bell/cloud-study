# Day 02 - Linux Users, Permissions, Processes, SSH

## 1. 사용자와 그룹

### 현재 사용자 확인

whoami

### 사용자가 속한 그룹 확인

group

## 2. Linux 파일 권한
파일 권한은 Owner, Group, Others로 구분된다.

-rwxr-xr--
│
├── Owner  : rwx
├── Group  : r-x
└── Others : r--

##chmod : 파일 권한 변경

ex) chmod 755 test.txt
755 -> rwxr-xr-x

##chown : 소유자 또는 그룹 변경

ex) chown user:group file

##sudo : 관리자(root) 권한으로 명령어를 실행한다.

ex) sudo command

## 3. Linux 프로세스

##프로세스 확인
ps
ps aux

##특정 프로세스 검색
ps aux | grep bash

##프로세스 종료
kill PID

##강제종료
kill -9 PID

## 4. SSH
SSH(Secure Shell)는 원격 컴퓨터에 안전하게 접속하기 위한 프로토콜이다.

##기본 SSH 포트 : TCP 22

##SSH Server 상태 확인
sudo systemctl status ssh
-> Active: active (running) //정상상태

## localhost 접속
ssh localhost

## 접속 종료
exit

## 5. SSH Key 인증
SSH key는 Private key와 Public key로 구성된다.

## Key 생성
ssh-keygen -t ed25519 -f ~/.ssh/cloud-study-key

-> ~/.ssh/cloud-study-key
   ~/.ssh/cloud-study-key.pub 
가 생성된다.

## 공개키 등록
cat ~/.ssh/cloud-study-key.pub >> ~/.ssh/authorized_keys

## Key를 이용한 SSH 접속
ssh -i ~/.ssh/cloud-study-key localhost

## SSH 관련 파일
~/.ssh/
├── cloud-study-key       # Private Key
├── cloud-study-key.pub   # Public Key
├── authorized_keys       # 접속을 허용할 Public Key
└── known_hosts           # 접속했던 서버 정보
