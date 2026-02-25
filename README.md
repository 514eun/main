# main
#1차 코드 설명
# GNS3 기반 인프라 & 네트워크 통합 자동화 파이썬 스크립트

## 프로젝트 소개
Linux 서버(Rocky/Ubuntu) 인프라 구축부터 Cisco 네트워크 장비(라우터 6대, 스위치 10대) 설정 및 백업까지, CLI에서 수동으로 타이핑하며 세팅하던 모든 과정을 Python으로 한 번에 자동화한 프로젝트입니다.

## 제작 배경 (Why I made this)
GNS3 환경에서 복잡한 토폴로지를 구성할 때마다 반복되는 iSCSI/Samba 스토리지 마운트, 웹 서버 초기 세팅, 그리고 수많은 네트워크 장비의 라우팅 상태를 일일이 확인하는 것은 실수(Typo)를 유발하고 시간이 너무 오래 걸렸습니다. 
"이 귀찮은 과정들을 메뉴판처럼 만들어서 한 번의 실행으로 끝낼 수는 없을까?"라는 고민에서 시작해, paramiko와 netmiko 라이브러리를 활용한 통합 관리형 CLI 툴을 개발하게 되었습니다.

## 주요 기능

### 1. 네트워크 장비 자동화 (Cisco IOS)
* Netmiko 연동: 라우터 6대, 스위치 10대에 일괄 SSH 접근.
* 원클릭 상태 점검: show ip route, show vtp status, show spanning-tree 등 실무에서 자주 쓰는 11가지 명령어를 메뉴화하여 바로 확인.
* 자동 백업 시스템: 장비별 running-config를 추출해 .txt 파일로 로컬에 자동 백업하고, 필요한 인터페이스 설정만 따로 파싱해서 출력.

### 2. 서버 인프라 원클릭 세팅 (Rocky / Ubuntu 지원)
* 스토리지 자동 구성: iSCSI 타겟/이니시에이터 설정, 사용자별 용량 제한(Quota), Samba/NFS 공유 및 자동 마운트(fstab).
* 웹/DB 서버 배포: Apache 가상 호스트(VirtualHost) 구성, PHP, MariaDB, FTP(vsftpd) 자동 설치 및 초기 계정 세팅.
* DNS 관리: BIND9 환경에서 파이썬 파일 입출력을 통해 도메인 레코드(A 레코드 등) 추가 및 초기화 기능 구현.

### 3. 보안 및 로그 모니터링
* SSH, FTP, Jupyter 접속 로그를 실시간으로 파싱.
* 비정상적인 로그인 실패 로그를 감지하고, 5회 이상 실패 시 "IP 차단 필요" 경고 문구 출력 (보안 관제 기초 로직 직접 구현).

## 사용 기술 및 환경
* Language: Python 3.x
* Library: netmiko, paramiko (SSH 통신)
* Infra: GNS3, Cisco IOS, Rocky Linux, Ubuntu Linux

## 실행 시 주의사항 (Troubleshooting)
초기 실행 시 라이브러리가 없으면 ModuleNotFoundError가 발생할 수 있습니다. 가상환경 세팅 후 아래 패키지를 먼저 설치해 주세요.

```bash
pip install netmiko paramiko
python main.py
