# 🔥 Bium (비움) — Backend

> 기록에도 온도가 있으니까.

기록의 중요도와 보존 기간에 따라 메모를 **불(Fire)**과 **얼음(Ice)**으로 나눠 관리하는 모바일 메모·협업 서비스, **비움**의 백엔드 저장소입니다. React Native 클라이언트, Spring Boot 서버, MySQL(AWS RDS) 구조로 동작하며 AWS EC2에 배포됩니다.

이 저장소는 그중 **Spring Boot 서버 부분**입니다.

---

## 💡 왜 만들었나

기존 메모 앱을 쓰면서 사람들이 겪는 불편은 대체로 같았습니다 — 필요 없어진 메모가 계속 쌓이고, 중요한 메모와 임시 메모가 뒤섞이고, 팀 협업 내용은 여러 서비스에 흩어져 있었습니다.

팀 내부 의견만으로는 근거가 부족하다고 판단해 「휘발성 메모 앱 사용 경험 및 기능 선호도」라는 이름으로 자체 설문조사를 진행했고, 59명의 응답을 바탕으로 세 가지 문제를 좁혔습니다.

| 문제 | 해결 방향 |
|---|---|
| 낮은 정보 관리 기능 (임시 메모가 계속 쌓임) | 기록을 **비워**, 중요한 정보만 남긴다 |
| 획일적인 보관 방식 (성격이 다른 기록도 같은 방식으로 보관) | **온도**라는 개념으로 기록을 구분한다 |
| 협업 도구의 분리 (공지·할일·일정·파일이 여러 서비스에 흩어짐) | 하나의 앱 안에 **팀스페이스** 기능을 만든다 |

## 🔥❄️ 핵심 개념

- **불 메모(Fire Memo)** — 6·12·24시간 중 소멸 시간을 선택하면, 그 시간이 지난 뒤 자동으로 사라지는 임시 기록. 기본 메모 작성 방식입니다.
- **얼음 메모(Ice Memo)** — 오래 남겨야 할 중요한 기록을 얼려서 보관. 홈에서 불 메모를 얼음 메모로 언제든 전환할 수 있습니다.
- **팀스페이스(Team Space)** — 팀원들과 공지·할일·일정·파일을 함께 관리하는 협업 공간. 개인 기록과 팀 협업을 하나의 앱 안에서 연결합니다.

## ⚙️ 주요 기능 (API 기준)

| 도메인 | 기능 |
|---|---|
| **인증** | 회원가입 / 로그인 / 로그아웃 / 아이디·비밀번호 찾기 / 비밀번호 재확인 / 2단계 인증(2FA) / 액세스 토큰 재발급 |
| **메모** | 작성 / 조회 / 단건 조회 / 휴지통 |
| **메모 이미지** | 업로드 / 메모별 조회 |
| **팀스페이스** | 생성 / 목록·단건 조회 / 팀원 초대·조회 / 공지 작성·조회 / 할일 작성·조회 / 팀 파일 업로드·조회 |
| **친구** | 목록 조회 / 친구 요청 / 대기 중인 요청 조회 |
| **알림 / 검색 / 서비스 공지 / 문의** | 조회 및 등록 |
| **파일** | S3 업로드용 Presigned URL 발급 (연동 준비 중) |

## 🔐 인증 구조

JWT 기반 인증을 직접 구현했습니다. 액세스 토큰은 30분, 리프레시 토큰은 14일로 만료 시간을 다르게 두고 `/api/auth/refresh`로 재발급하는 구조이며, 여기에 더해 2단계 인증(2FA) 엔드포인트도 별도로 두었습니다.

## 🛠 기술 스택

![Java](https://img.shields.io/badge/Java-21-orange)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.3.0-6DB33F)
![Spring Security](https://img.shields.io/badge/Spring%20Security-JWT-6DB33F)
![MySQL](https://img.shields.io/badge/MySQL-AWS%20RDS-4479A1)
![AWS EC2](https://img.shields.io/badge/Deploy-AWS%20EC2-FF9900)

- **Language / Framework**: Java 21, Spring Boot 3.3.0
- **인증**: Spring Security + JJWT (Access/Refresh Token, 2FA)
- **DB**: Spring Data JPA + MySQL (AWS RDS)
- **배포**: AWS EC2
- **클라이언트**: React Native (별도 저장소)

## 🏗 아키텍처

```
React Native (Client)
        │  REST API
        ▼
Spring Boot (AWS EC2)  ──  MySQL (AWS RDS)
```

## 👥 팀 딸깍

| 역할 | 이름 |
|---|---|
| Developer | Chawon Go, Taejeong Ryu, Minseok Kim, Youngjun Tak |
| Designer | Yunyoung Choi |

## 🏆 성과

- 자체 설문조사(응답자 59명)를 바탕으로 사용자 리서치부터 기능 정의까지 진행
- 관련 학술 논문이 ICRP 학술지에 게재 예정이며, 국제학술대회 발표를 앞두고 있습니다
- 백엔드 코드 기준 약 3,900줄, 컨트롤러 17개 도메인으로 구성

## 🚧 현재 상태

핵심 기능(인증, 메모, 팀스페이스, 친구, 알림, 검색)은 구현되어 있으며, 2026년 9월 완성을 목표로 마무리 작업을 진행 중입니다. 파일 업로드는 Presigned URL 발급 API까지 준비되어 있고 실제 S3 연동은 진행 중입니다.

## 실행 방법

```bash
./gradlew bootRun
```

`application.properties`의 `DB_HOST`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `jwt.secret` 환경변수를 실행 환경에 맞게 설정해야 합니다.
