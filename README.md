### 혼자해보는 SE 실습!!

리눅스 가상 서버 구축 및 웹 서비스 운영

#### 개요
VirtualBox를 활용하여 Ubuntu Server를 설치하고, Apache 웹 서버를 구축하였습니다.  
간단한 방화벽 설정과 함께 웹 서비스가 외부에서 접근 가능하도록 구성하였습니다.

#### 실습 환경
- VirtualBox 7.x
- Ubuntu Server 20.04 LTS
- Apache2
- UFW 방화벽

#### 주요 내용
- Ubuntu Server 설치 및 기본 보안 설정
- Apache2 웹 서버 설치 및 실행 확인
- UFW 방화벽 설정을 통한 외부 접근 제어
- 브라우저를 통한 웹 페이지 확인

#### 실행 화면
![Apache Page](screenshots/apache-default.png)

#### 배운 점
- 리눅스 서버 환경의 구조와 기본 명령어 숙지
- 가상 머신 네트워크 설정 및 IP 접근 개념
- 웹 서버와 방화벽의 기본 구성 원리 이해

- Ubuntu Server 20.04 환경에서 Apache 웹 서버를 설치 및 구동하는 실습입니다.

#### 실습 내용

- VirtualBox에 Ubuntu Server 설치
- Apache 웹 서버 설치
- 브라우저에서 서버 접속 테스트

#### 사용 명령어 요약

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 -y
ip a 

결과 화면
http://[Ubuntu서버 IP주소] -> http://192.168.0.***/ 접속 시 Apache 기본 환영 페이지 확인

실행권한 
chmod +x setup.sh

