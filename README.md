### 혼자해보는 SE 실습 첫번 째!

[ 1. 리눅스 가상 서버 구축 및 웹 서비스 운영 ---> 2. 웹 서비스 배포 & 로그 모니터링 시스템 구축 ---> 3. 자동화 스크립트 + 백업 + cron ]

## 1. 리눅스 가상 서버 구축 및 웹 서비스 운영

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
![Apache Page] 캡쳐 파일 등록 첨부 완료!✅

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

sudo apt update && sudo apt upgrade -y   Apache라는 웹 서버 프로그램을 설치
sudo apt install apache2 -y
ip a  가상 서버 IP 확인:

결과 화면
http://[Ubuntu서버 IP주소] -> http://192.168.0.***/ 접속 시 Apache 기본 환영 페이지 확인. 서버가 잘 켜졌는지 확인하는거!
가상 머신 안에서만 열리는 거라서, 호스트 PC에서 가상 서버의 IP로 접속

실행권한 
chmod +x setup.sh

📌 필수 확인

✅ 가상 머신 네트워크 설정이 NAT일 경우, 접속이 안됌. 이럴 땐 "브리지 어댑터"로 바꿔줘야 함

바꾸는 방법:
VirtualBox 메인화면 → 가상 머신 선택

[설정] → [네트워크]

어댑터1 → "네트워크 어댑터 사용" 체크

연결 방식 → 브리지 어댑터(Bridged Adapter) 선택

확인 → 가상 머신 다시 시작

```

------------------------------------------------------------------------------------------
## 2. 웹 서비스 배포 & 로그 모니터링 시스템 구축

<✅Apache 웹서비스 배포 + 로그 모니터링 실습>

🎯 목표
직접 만든 웹페이지를 /var/www/html에 배포. Apache 로그 분석 툴 GoAccess 설치 → 실시간 모니터링

1. 웹사이트 배포 (간단한 HTML) --> sudo nano /var/www/html/index.html (포트폴리오 홈페이지)

```

<!DOCTYPE html>
<html>
  <head>
    <title>My Apache Site</title>
  </head>
  <body>
    <h2>Hello from my Ubuntu Apache Server!</h2>
    <p> Created by Dahee! </p>
  </body>
</html>

```
2.  Apache 로그 확인
sudo tail -f /var/log/apache2/access.log
→ 브라우저로 접속할 때마다 로그가 실시간 출력됨

3. 실시간 로그 분석 툴 설치 (GoAccess)
sudo apt install goaccess -y

4. GoAccess 실행
sudo goaccess /var/log/apache2/access.log --log-format=COMBINED -o report.html

5. 에서 결과 보기
sudo mv report.html /var/www/html/report.html
→ 브라우저에서 http://서버ip/report.html 접속!

------------------------------------------------------------------------------------------

## 3. 자동화 스크립트 + 백업 + cron

Apache 서버를 자동화해서, 실무에서 진짜 하는 백업/배포/유지관리 흐름해보자규우우! 악
Ubuntu 서버에서 Apache 설치 및 웹페이지 배포, 백업 스크립트를 자동화하고 cron을 통해 주기적 실행을 구성했습니다!

<✅Apache 웹서버 자동화 배포 + 백업 + 스케줄링 (cron)>

🎯 목표
1. Apache 웹 서버 자동 설치 & 페이지 배포 스크립트 (setup.sh)
2. 웹사이트와 로그 자동 백업 스크립트 (backup.sh)
3. cron에 등록하여 주기적으로 자동 백업 실행

1. setup.sh – Apache 설치 + index.html 배포 자동화 --> nano setup.sh

Apache 설치 => sudo apt update && sudo apt install apache2 -y

웹페이지 배포  => echo "<h1>Welcome to my automated Apache server!</h1><p>Created by [너의 이름]</p>" | sudo tee /var/www/html/index.html

Apache 재시작  => sudo systemctl restart apache2

echo "Apache 설치 및 웹페이지 배포 완료!"

chmod +x setup.sh

2. backup.sh – 웹 페이지와 로그 백업 자동화

nano backup.sh

백업 폴더 및 파일명 => 
TIMESTAMP=$(date +%Y-%m-%d_%H-%M-%S)
BACKUP_DIR=~/apache_backups
mkdir -p $BACKUP_DIR

백업 항목: 웹페이지와 로그 => tar -czf $BACKUP_DIR/backup_$TIMESTAMP.tar.gz /var/www/html /var/log/apache2

echo "백업 완료: $BACKUP_DIR/backup_$TIMESTAMP.tar.gz"

chmod +x backup.sh

3. cron에 등록해 자동 실행

크론 편집 => crontab -e

아래 한 줄 추가 (매일 오전 2시에 백업 실행) => 0 2 * * * /home/ubuntu/backup.sh >> /home/ubuntu/backup.log 2>&1

경로는 내 사용자 홈 디렉토리에 따라 수정!! 예: /home/ubuntu → 실제 사용자 경로 확인은 echo $HOME 로 가능

```
구성 파일

- `setup.sh`: Apache 설치 및 index.html 배포 자동화
- `backup.sh`: /var/www/html 및 Apache 로그 백업
- `cron`: 매일 오전 2시 자동 백업

예시 실행 결과

- 백업 파일: `~/apache_backups/backup_2025-04-22_02-00-00.tar.gz`
- cron 로그 파일: `~/backup.log`

```
