# On-Premises Web Platform

**React·Node.js 웹 서비스와 온프레미스 배포·운영 환경을 직접 구축한 프로젝트입니다.**<br>

사내 서버에서 동작하는 웹 서비스를 만들고 서버 설치부터 HTTPS 적용, 배포 자동화, 로그 관리까지 맡았습니다.<br>
이후 관리자 인증과 콘텐츠 관리, 이미지 업로드, 다국어·테마 전환 기능을 추가했습니다.<br>

[포트폴리오 보기](https://domado.me/portfolio)

<br>
<br>

## ▎ 프로젝트 개요

![온프레미스 웹 서비스 개발과 배포·보안·운영을 정리한 프로젝트 개요](assets/images/slides/01-project-overview.png)

<br>

| 항목 | 내용 |
| --- | --- |
| 목표 | 사내 온프레미스 환경에서 웹 서비스를 제공하고 배포·운영 절차 정리 |
| 초기 구축 기간 | 2025.08.01 ~ 2025.08.13 |
| 인원·역할 | 1명 · 기획, 디자인, 프론트엔드, 백엔드, 서버·네트워크, 배포·운영 전담 |
| 서비스 구성 | React SPA + Node.js/Express API + Nginx Reverse Proxy |
| 운영 환경 | Ubuntu Server, 단일 공인 IP, 사내 네트워크 |
| 1.0 | 웹 서비스와 메일 API 구현, 서버 설치, HTTPS 적용, 배포·로그 관리 자동화 |
| 2.0 후속 개선 | JWT 관리자 인증, 콘텐츠 CRUD, 이미지 업로드·URL 서빙, 다국어·테마 전환 |

<br>
<br>

## ▎ 주요 기능

| 기능 | 구현 내용 |
| --- | --- |
| 반응형 웹 | React·TailwindCSS로 모바일과 PC 화면 구성, 공통 UI 컴포넌트 정리 |
| 다국어·테마 | Context API로 한·영 전환과 다크·라이트 모드 상태 관리 |
| 관리자 페이지 | JWT(HMAC) 인증, 관리자 전용 프로젝트·제품 데이터 CRUD, JSON 파일 저장 |
| 이미지 업로드 | Multer로 파일 저장, URL로 이미지 제공, 업로드 미리보기 |
| 문의 메일 | Nodemailer와 하이웍스 SMTPS 연동, 입력값 검증, 전송 오류 처리·로그 기록 |
| 화면 피드백 | 언어 선택 모달, 로딩 표시, 오류 안내, 모바일 플로팅 메뉴 조정 |

<br>
<br>

## ▎ 설계 판단과 시스템 구조
![1인 개발과 온프레미스 제약에서 배포·서빙·보안·업로드 대안을 비교한 IDEAL 자료](assets/images/slides/02-ideal-design-decisions.png)


### · IDEAL과 기술 선택



1인 개발과 제한된 구축 기간을 고려해 구현 속도, 반복 운영 부담, 장애 시 복구 방법을 함께 비교했습니다.<br>
Nginx는 정적 파일 제공과 API 라우팅을 맡고 PM2는 Node.js 프로세스를 관리하도록 구성했습니다.<br>
배포는 Shell Script로 묶고 HTTPS 인증서 발급·갱신에는 Certbot을 사용했습니다.<br>

<br>

| 검토 항목 | 비교한 방식 | 선택과 이유 |
| --- | --- | --- |
| 배포 | 수동 배포, Shell Script + PM2, Docker | 제한된 기간 안에 반복 작업을 줄이고 직접 관리할 수 있는 Shell Script + PM2 선택 |
| 서비스 제공 | Node.js 단독 서빙, Nginx Reverse Proxy | 정적 파일, API 요청, 도메인별 라우팅을 Nginx에서 구분 |
| HTTPS | HTTP 운영, TLS + Certbot | HTTPS 적용과 인증서 갱신 절차 구성 |
| 이미지 관리 | Base64 저장, 파일 업로드 + URL 서빙 | Multer로 파일을 저장하고 URL로 조회하는 방식으로 변경 |

<br>
<br>

## ▎ 시스템 아키텍처

![단일 공인 IP에서 Nginx로 도메인별 요청을 나누는 온프레미스 시스템 아키텍처](assets/images/slides/05-system-architecture.jpg)


공유기의 80·443 포트 요청을 Nginx 서버로 모으고 요청 도메인에 따라 웹 서비스와 기존 내부 서비스를 구분했습니다.<br>
React 빌드 결과는 정적 파일로 제공하고 API 요청은 Express로 전달합니다.<br>
다른 내부 서버로 향하는 요청도 Nginx를 거치도록 정리했습니다.<br>

<br>
<br>

## ▎ 서버 및 인프라

![Ubuntu Server와 Nginx·PM2·Certbot으로 구성한 서버 및 인프라](assets/images/slides/04-server-and-infrastructure.jpg)


Ubuntu Server 설치부터 무선 LAN 드라이버 오프라인 설치와 Netplan 설정까지 직접 진행했습니다.<br>
UFW에서 웹 서비스와 SSH에 필요한 포트만 허용하고 백엔드 포트는 외부에서 직접 접근하지 않도록 구성했습니다.<br>

<br>
<br>

## ▎ 사용 기술

![프론트엔드·백엔드·인프라에 사용한 기술 스택](assets/images/slides/03-technology-stack.jpg)

<br>

<table width="100%">
  <thead>
    <tr>
      <th width="180">영역</th>
      <th width="660">기술</th>
      <th width="280">적용 목적</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Frontend</strong></td>
      <td>
        <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=white" alt="JavaScript">
        <img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=white" alt="React">
        <img src="https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white" alt="Vite">
        <br>
        <img src="https://img.shields.io/badge/TailwindCSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white" alt="TailwindCSS">
        <img src="https://img.shields.io/badge/Axios-5A29E4?style=for-the-badge&logo=axios&logoColor=white" alt="Axios">
        <img src="https://img.shields.io/badge/Sonner-1E293B?style=for-the-badge&logo=react&logoColor=white" alt="Sonner">
      </td>
      <td>반응형 SPA·관리자 화면<br>다국어·테마 전환<br>로딩·오류 피드백</td>
    </tr>
    <tr>
      <td><strong>Backend / API</strong></td>
      <td>
        <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js">
        <img src="https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white" alt="Express">
        <img src="https://img.shields.io/badge/JWT%20(HMAC)-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white" alt="JWT(HMAC)">
        <br>
        <img src="https://img.shields.io/badge/Multer-FF6F00?style=for-the-badge&logo=node.js&logoColor=white" alt="Multer">
        <img src="https://img.shields.io/badge/Nodemailer-007396?style=for-the-badge&logo=gmail&logoColor=white" alt="Nodemailer">
        <img src="https://img.shields.io/badge/Validator.js-000000?style=for-the-badge&logo=javascript&logoColor=white" alt="Validator">
        <br>
        <img src="https://img.shields.io/badge/Sanitize--HTML-5A29E4?style=for-the-badge&logo=javascript&logoColor=white" alt="sanitize-html">
        <img src="https://img.shields.io/badge/CORS-FF6F00?style=for-the-badge&logo=securityscorecard&logoColor=white" alt="CORS">
        <img src="https://img.shields.io/badge/Express%20Rate%20Limit-2B037A?style=for-the-badge&logo=express&logoColor=white" alt="Rate-Limit">
        <br>
        <img src="https://img.shields.io/badge/dotenv-000000?style=for-the-badge&logo=dotenv&logoColor=white" alt="dotenv">
      </td>
      <td>관리자 인증·콘텐츠 CRUD<br>이미지 업로드·문의 메일<br>입력 검증·요청 제어</td>
    </tr>
    <tr>
      <td><strong>DevOps / Infra</strong></td>
      <td>
        <img src="https://img.shields.io/badge/Ubuntu%20Server-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu Server">
        <img src="https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white" alt="Nginx">
        <img src="https://img.shields.io/badge/Certbot-003A70?style=for-the-badge&logo=letsencrypt&logoColor=white" alt="Certbot">
        <br>
        <img src="https://img.shields.io/badge/PM2-2B037A?style=for-the-badge&logo=pm2&logoColor=white" alt="PM2">
        <img src="https://img.shields.io/badge/GitHub%20Deploy%20Key-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub">
        <img src="https://img.shields.io/badge/Bash_Shell-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white" alt="Bash Shell">
        <br>
        <img src="https://img.shields.io/badge/SSH-2C2D72?style=for-the-badge&logo=openssh&logoColor=white" alt="SSH">
        <img src="https://img.shields.io/badge/UFW-F05032?style=for-the-badge&logo=linux&logoColor=white" alt="UFW">
      </td>
      <td>서버 구축·도메인 라우팅<br>HTTPS·접근 제어<br>배포 자동화·프로세스 관리</td>
    </tr>
    <tr>
      <td><strong>Monitoring / Logging</strong></td>
      <td>
        <img src="https://img.shields.io/badge/tmux-1BB91F?style=for-the-badge&logo=tmux&logoColor=white" alt="tmux">
        <img src="https://img.shields.io/badge/htop-00BFA5?style=for-the-badge&logo=linux&logoColor=white" alt="htop">
        <img src="https://img.shields.io/badge/iftop-007396?style=for-the-badge&logo=linux&logoColor=white" alt="iftop">
        <br>
        <img src="https://img.shields.io/badge/logrotate-555555?style=for-the-badge&logo=linux&logoColor=white" alt="logrotate">
        <img src="https://img.shields.io/badge/vnstat-004B87?style=for-the-badge&logo=linux&logoColor=white" alt="vnstat">
        <img src="https://img.shields.io/badge/crontab-5A29E4?style=for-the-badge&logo=linux&logoColor=white" alt="crontab">
      </td>
      <td>리소스·트래픽 확인<br>서비스 로그 확인<br>로그 회전·압축·정기 백업</td>
    </tr>
  </tbody>
</table>

<br>
<br>

## ▎ 보안 및 안정성 설계

![HTTPS·방화벽·입력 검증과 메일 전송 설정을 정리한 보안 및 안정성 설계](assets/images/slides/06-security-and-reliability.jpg)


### · 요청 검증과 관리자 인증

Express API에 validator, sanitize-html, 요청 횟수 제한, CORS 허용 목록을 적용했습니다.<br>
관리자 API는 JWT(HMAC) 토큰을 검증한 뒤 콘텐츠 생성·수정·삭제 요청을 처리합니다.<br>
관리자 화면에는 별도의 Refresh Token 없이 로그인 상태를 유지하는 로직을 구현했습니다.<br>

<br>

### · HTTPS와 메일 전송

Certbot으로 인증서를 발급하고 자동 갱신을 구성했으며 HTTP 요청은 HTTPS로 전환하도록 설정했습니다.<br>
인증서 검증에 쓰는 ACME 경로는 리다이렉트 예외로 처리했습니다.<br>


문의 메일은 하이웍스 SMTPS로 전송합니다.<br>
메일 발신자는 회사 계정으로 고정하고 Reply-To에 문의자의 주소를 넣어 발신자와 회신 대상을 분리했습니다.<br>
전송 실패 원인을 확인할 수 있도록 오류 처리와 로그를 추가했습니다.<br>

<br>
<br>

## ▎ 배포와 운영
![소스 동기화부터 빌드·서비스 반영·상태 확인까지의 배포 자동화 파이프라인](assets/images/slides/08-deployment-pipeline.jpg)

### · 배포 자동화 파이프라인

읽기 전용 GitHub Deploy Key로 소스를 가져오고 deploy.sh에 빌드와 Nginx·PM2 반영 절차를 묶었습니다.<br>
배포 후에는 curl로 서비스 응답을 확인하도록 했습니다.<br>
PM2에는 프로세스 자동 재시작과 서버 부팅 시 실행을 설정했습니다.<br>

<br>
<br>

## ▎ 서버 모니터링 및 운영 구조

![tmux에서 리소스·네트워크·서비스 로그를 함께 확인하는 운영 화면](assets/images/slides/07-monitoring-and-operations.jpg)


tmux 화면을 세 영역으로 나눠 htop의 CPU·메모리, iftop의 네트워크 트래픽, 서비스 로그를 함께 확인했습니다.<br>
Nginx의 access/error 로그는 도메인별로 분리하고 logrotate로 회전·압축·보관을 관리했습니다.<br>
매일 03:00에 로그를 백업해 7일간 보관하도록 crontab을 설정했으며 vnstat으로 누적 트래픽을 확인했습니다.<br>

<br>
<br>

## ▎ 기여 범위 및 구축 결과

![프론트엔드·백엔드·보안·인프라·운영의 기여 범위와 구축 결과를 정리한 통합 자료](assets/images/slides/09-contribution-and-results.png)


프론트엔드와 API 개발, 서버 설치, 네트워크 설정, 배포·운영까지 단독으로 맡았습니다.<br>
1.0에서 서비스 운영 환경을 구성한 뒤, 2.0에서는 관리자 기능과 이미지 처리 방식, 화면 사용성을 보완했습니다.<br>

<br>

| 직접 맡은 영역 | 구축 결과 |
| --- | --- |
| 프론트엔드 | 반응형 SPA, 다국어·테마 전환, 관리자 UI, 로딩·오류 피드백 구현 |
| 백엔드·인증 | 문의 메일 API, JWT 인증, 관리자 콘텐츠 CRUD, 업로드 API 구현 |
| 이미지 처리 | Base64 저장 방식에서 파일 업로드·URL 서빙으로 변경, 로컬·배포 환경의 경로 설정 정리 |
| 인프라·네트워크 | 단일 공인 IP에서 두 서비스 라우팅, HTTPS 적용, 내부 포트 직접 노출 제한 |
| 배포·운영 | Shell Script 배포, PM2 자동 재시작, 로그 분리·회전·백업, tmux 모니터링 구성 |

<br>
<br>

## ▎ 주요 문제와 해결

### · 단일 공인 IP에서 두 서비스 연결

사내 공인 IP 하나로 서로 다른 내부 서버의 서비스를 제공해야 했습니다.<br>
공유기 포트포워딩의 진입점을 Nginx로 모으고 Host 헤더에 따라 도메인별로 요청을 나누었습니다.<br>
공인 IP를 추가하지 않고 두 서비스를 함께 운영하도록 구성했습니다.<br>

<br>

### · 인증서 발급 실패와 WebSocket 연결 문제

HTTP 전체 리다이렉트 때문에 인증서 검증 파일에 접근할 수 없는 문제가 있었습니다.<br>
ACME 경로가 리다이렉트보다 먼저 처리되도록 예외를 적용하고 검증 경로의 응답을 확인한 뒤 인증서를 발급했습니다.<br>
WebSocket 연결에는 Upgrade·Connection 헤더 처리를 추가했습니다.<br>

<br>

### · 배포 서버의 이미지 403·404 오류

로컬에서 정상적으로 보이던 업로드 이미지가 Ubuntu 배포 환경에서는 403·404 오류를 반환했습니다.<br>
Nginx alias로 직접 제공하던 경로를 Node.js로 프록시하도록 바꾸고 애플리케이션에서 파일을 제공하도록 정리했습니다.<br>
업로드 경로와 조회 URL은 환경 설정으로 관리해 로컬·배포 환경의 차이를 반영했습니다.<br>

<br>

### · 서버 설치 직후 네트워크 연결 복구

Ubuntu Server 설치 직후 네트워크 장치를 사용할 수 없어 필요한 패키지를 내려받지 못했습니다.<br>
무선 LAN 연결에 필요한 파일을 오프라인으로 옮겨 설치하고 Netplan 설정과 DHCP 상태를 점검해 연결을 복구했습니다.<br>

<br>
<br>

## ▎ 이미지 자료 (2.0 ver)

![2.0 버전 첫 접속 시 한국어와 영어를 선택하는 언어 설정 화면](assets/images/gallery/v2-01.png)

<br>

![2.0 버전 웹사이트 메인 화면과 언어·테마 전환 메뉴](assets/images/gallery/v2-02.png)

<br>

![2.0 버전 제품 소개 메인 화면](assets/images/gallery/v2-03.png)

<br>

![2.0 버전 프로젝트 소개 메인 화면](assets/images/gallery/v2-04.png)

<br>

![2.0 버전 관리자 메뉴 표시와 언어·다크모드 전환 안내](assets/images/gallery/v2-05.png)

<br>

![2.0 버전 관리자 로그인 화면](assets/images/gallery/v2-06.png)

<br>

![2.0 버전 관리자 콘솔과 최근 안내 화면](assets/images/gallery/v2-07.png)

<br>

![2.0 버전 제품 목록과 관리자용 등록·수정·삭제 메뉴](assets/images/gallery/v2-08.png)

<br>

![2.0 버전 제품 등록·수정 및 이미지 업로드 화면](assets/images/gallery/v2-09.png)

<br>
<br>

## ▎ 이미지 자료 (1.0 ver)

![1.0 버전 데스크톱·모바일 사이트 인트로와 로고 애니메이션 설명](assets/images/gallery/v1-01.jpg)

<br>

![1.0 버전 웹사이트 메인 화면](assets/images/gallery/v1-02.jpg)

<br>

![1.0 버전 메인 화면의 반응형 구성과 배경 영상 설명](assets/images/gallery/v1-03.jpg)

<br>

![1.0 버전 로고·햄버거 메뉴·플로팅 버튼 구성](assets/images/gallery/v1-04.jpg)

<br>

![1.0 버전 상단 로고와 문의·맨 위로 이동 버튼 동작 설명](assets/images/gallery/v1-05.jpg)

<br>

![1.0 버전 햄버거 메뉴와 슬라이드 내비게이션 설명](assets/images/gallery/v1-06.jpg)

<br>

![1.0 버전 제품 소개 메인 화면](assets/images/gallery/v1-07.jpg)

<br>

![1.0 버전 프로젝트 소개 메인 화면](assets/images/gallery/v1-08.jpg)

<br>

![1.0 버전 푸터와 정책·문의 페이지 이동 안내](assets/images/gallery/v1-09.jpg)

<br>

![1.0 버전 개인정보 처리방침과 접근성 안내 페이지](assets/images/gallery/v1-10.jpg)

<br>

![1.0 버전 회사 소개 페이지](assets/images/gallery/v1-11.jpg)

<br>

![1.0 버전 제품 목록 페이지](assets/images/gallery/v1-12.jpg)

<br>

![1.0 버전 프로젝트 목록 페이지](assets/images/gallery/v1-13.jpg)

<br>

![1.0 버전 제품·프로젝트 카드의 상세 모달 화면](assets/images/gallery/v1-14.jpg)

<br>

![1.0 버전 문의 양식과 지도 화면](assets/images/gallery/v1-15.jpg)

<br>

![1.0 버전 문의 메일 수신과 전송 상태 알림 화면](assets/images/gallery/v1-16.jpg)
