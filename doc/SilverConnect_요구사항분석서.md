# SilverConnect 요구사항 분석서

> **프로젝트명:** 실버커넥트 (SilverConnect) - 은퇴자 재능·재취업 매칭 플랫폼  
> **버전:** 1.0  
> **작성일:** 2025년 05월

---

## 목차

1. [개요](#1-개요)
2. [기능 관점 분석 - 유스케이스 다이어그램](#2-기능-관점-분석)
3. [구조 관점 분석 - 클래스 다이어그램](#3-구조-관점-분석)
4. [행위 관점 분석 - 순차 다이어그램](#4-행위-관점-분석)
5. [상태기계 다이어그램](#5-상태기계-다이어그램)

---

## 1. 개요

SilverConnect는 은퇴자들이 보유한 전문 지식과 경험을 활용할 수 있도록 기업 및 개인과 연결해주는 AI 기반 매칭 플랫폼이다. 본 문서는 기능 관점, 구조 관점, 행위 관점에서 시스템을 분석하며, 각각 유스케이스 다이어그램, 클래스 다이어그램, 순차 다이어그램으로 표현한다.

**주요 액터:**
- **은퇴자 (Retiree):** 재취업 또는 재능기부를 희망하는 사용자
- **구인자 (Employer):** 시니어 전문인력을 필요로 하는 기업 또는 개인
- **관리자 (Admin):** 시스템 운영 및 사용자 관리 담당자
- **AI 매칭 시스템:** 자동 매칭 알고리즘 수행 주체

---

## 2. 기능 관점 분석

### 2.1 유스케이스 다이어그램

```mermaid
graph TB
    subgraph Actors["액터"]
        R([은퇴자])
        E([구인자])
        A([관리자])
        AI([AI 매칭 시스템])
    end

    subgraph System["SilverConnect 시스템"]
        subgraph ProfileMgmt["프로필 관리"]
            UC1[프로필 등록/수정/삭제]
            UC2[자격증·수상이력 추가]
            UC3[근무형태 선택]
        end

        subgraph Matching["AI 매칭"]
            UC4[구인공고 자동 추천]
            UC5[은퇴자 후보 자동 추천]
            UC6[매칭 점수 산출]
            UC7[매칭 알고리즘 개선]
        end

        subgraph JobPosting["구인공고 관리"]
            UC8[구인공고 등록]
            UC9[구인공고 검색]
            UC10[공고 수정/마감]
        end

        subgraph Communication["소통"]
            UC11[실시간 채팅]
            UC12[면담 예약]
            UC13[알림 발송]
        end

        subgraph Activity["활동 이력"]
            UC14[활동 이력 조회]
            UC15[리뷰·평점 등록]
            UC16[활동 확인서 발급]
        end

        subgraph Mentoring["재능기부·멘토링"]
            UC17[재능기부 등록]
            UC18[멘토링 매칭]
        end

        subgraph AdminFunc["관리자 기능"]
            UC19[사용자 관리]
            UC20[통계 현황 확인]
        end
    end

    R --> UC1
    R --> UC2
    R --> UC3
    R --> UC4
    R --> UC11
    R --> UC12
    R --> UC14
    R --> UC15
    R --> UC17
    R --> UC9

    E --> UC8
    E --> UC10
    E --> UC5
    E --> UC11
    E --> UC12
    E --> UC14
    E --> UC15
    E --> UC9

    AI --> UC4
    AI --> UC5
    AI --> UC6
    AI --> UC7

    A --> UC19
    A --> UC20
    A --> UC16

    UC12 --> UC13
    UC4 --> UC6
    UC5 --> UC6
    UC17 --> UC18
```

### 2.2 유스케이스 설명서

#### UC1: 프로필 등록/수정/삭제

| 항목 | 내용 |
|---|---|
| **유스케이스 ID** | UC1 |
| **유스케이스명** | 프로필 등록/수정/삭제 |
| **액터** | 은퇴자 |
| **개요** | 은퇴자가 이름, 연령, 경력, 전문분야 등 프로필 정보를 등록·수정·삭제한다 |
| **사전 조건** | 은퇴자가 시스템에 로그인한 상태 |
| **사후 조건** | 프로필 정보가 시스템에 저장되어 AI 매칭에 활용됨 |
| **기본 흐름** | 1. 프로필 메뉴 접근 → 2. 정보 입력(이름/연령/경력/전문분야) → 3. 근무형태 선택 → 4. 저장 |
| **예외 흐름** | 필수 항목 미입력 시 오류 메시지 표시 후 재입력 요청 |
| **관련 요구사항** | FR-001, FR-003, FR-004 |

#### UC4: 구인공고 자동 추천 (AI 매칭)

| 항목 | 내용 |
|---|---|
| **유스케이스 ID** | UC4 |
| **유스케이스명** | 구인공고 자동 추천 |
| **액터** | 은퇴자, AI 매칭 시스템 |
| **개요** | AI가 은퇴자의 경력·전문분야를 분석하여 최적의 구인공고를 자동 추천한다 |
| **사전 조건** | 은퇴자 프로필이 등록된 상태, 매칭 가능한 구인공고 존재 |
| **사후 조건** | 매칭 점수 순으로 정렬된 추천 목록 제공 |
| **기본 흐름** | 1. 프로필 분석 요청 → 2. AI가 경력/전문분야 분석 → 3. 매칭 점수 산출 → 4. 상위 공고 목록 반환 |
| **예외 흐름** | 매칭 공고 없을 시 "현재 적합한 공고가 없습니다" 안내 |
| **관련 요구사항** | FR-005, FR-007, FR-008 |

#### UC12: 면담 예약

| 항목 | 내용 |
|---|---|
| **유스케이스 ID** | UC12 |
| **유스케이스명** | 면담 예약 |
| **액터** | 은퇴자, 구인자 |
| **개요** | 구인자와 은퇴자가 면담 일정을 조율하고 예약한다 |
| **사전 조건** | 양측이 매칭된 상태로 채팅이 가능한 상태 |
| **사후 조건** | 면담 일정이 등록되고 양측에 알림 발송 |
| **기본 흐름** | 1. 면담 예약 요청 → 2. 가능 일정 제시 → 3. 상대방 확인 → 4. 예약 확정 → 5. 알림 발송 |
| **예외 흐름** | 일정 충돌 시 다른 일정 제안 |
| **관련 요구사항** | FR-013, FR-014 |

---

## 3. 구조 관점 분석

### 3.1 클래스 다이어그램

```mermaid
classDiagram
    class User {
        +String userId
        +String email
        +String password
        +String phone
        +String userType
        +DateTime createdAt
        +login()
        +logout()
        +updatePassword()
    }

    class Retiree {
        +String name
        +int age
        +String career
        +String expertise
        +String workType
        +List~String~ certificates
        +List~String~ awards
        +float avgRating
        +registerProfile()
        +updateProfile()
        +deleteProfile()
        +addCertificate()
        +viewMatchingList()
    }

    class Employer {
        +String companyName
        +String businessNumber
        +String industry
        +String contactPerson
        +postJob()
        +updateJob()
        +closeJob()
        +viewCandidates()
    }

    class Admin {
        +manageUsers()
        +viewStatistics()
        +issueActivityCert()
    }

    class JobPosting {
        +String jobId
        +String title
        +String description
        +String workType
        +String location
        +String salary
        +String industry
        +DateTime deadline
        +String status
        +create()
        +update()
        +close()
        +search()
    }

    class MatchingResult {
        +String matchId
        +float matchScore
        +String status
        +DateTime matchedAt
        +calculateScore()
        +updateFeedback()
    }

    class AIMatchingEngine {
        +analyzeProfile()
        +analyzeJobRequirements()
        +calculateMatchScore()
        +generateRecommendations()
        +updateAlgorithm()
    }

    class ChatRoom {
        +String roomId
        +DateTime createdAt
        +String status
        +sendMessage()
        +loadHistory()
    }

    class Message {
        +String messageId
        +String content
        +DateTime sentAt
        +String messageType
        +send()
    }

    class Interview {
        +String interviewId
        +DateTime scheduledAt
        +String location
        +String status
        +reserve()
        +cancel()
        +sendNotification()
    }

    class Review {
        +String reviewId
        +int rating
        +String content
        +DateTime createdAt
        +String reviewerType
        +write()
        +display()
    }

    class TalentDonation {
        +String donationId
        +String category
        +String description
        +String status
        +register()
        +match()
        +complete()
        +issueCertificate()
    }

    class ActivityHistory {
        +String historyId
        +String activityType
        +DateTime activityDate
        +String result
        +record()
        +view()
    }

    User <|-- Retiree
    User <|-- Employer
    User <|-- Admin

    Employer "1" --> "0..*" JobPosting : 등록
    Retiree "1" --> "0..*" MatchingResult : 받음
    JobPosting "1" --> "0..*" MatchingResult : 대상

    AIMatchingEngine --> MatchingResult : 생성
    AIMatchingEngine --> Retiree : 분석
    AIMatchingEngine --> JobPosting : 분석

    Retiree "1" --> "0..*" ChatRoom : 참여
    Employer "1" --> "0..*" ChatRoom : 참여
    ChatRoom "1" --> "0..*" Message : 포함

    Retiree --> Interview : 참여
    Employer --> Interview : 신청
    Interview --> Message : 알림생성

    Retiree "1" --> "0..*" Review : 작성
    Employer "1" --> "0..*" Review : 작성
    Review --> MatchingResult : 연관

    Retiree "1" --> "0..*" TalentDonation : 등록
    TalentDonation --> ActivityHistory : 기록

    Retiree "1" --> "0..*" ActivityHistory : 보유
    Employer "1" --> "0..*" ActivityHistory : 보유
```

---

## 4. 행위 관점 분석

### 4.1 순차 다이어그램 1: AI 매칭 추천 프로세스

```mermaid
sequenceDiagram
    actor R as 은퇴자
    participant UI as 플랫폼 UI
    participant PM as 프로필 서비스
    participant AI as AI 매칭 엔진
    participant DB as 데이터베이스
    participant N as 알림 서비스

    R->>UI: 매칭 추천 요청
    UI->>PM: 프로필 정보 조회 요청
    PM->>DB: 은퇴자 프로필 조회
    DB-->>PM: 프로필 데이터 반환
    PM-->>UI: 프로필 정보 반환

    UI->>AI: 매칭 분석 요청 (프로필 데이터 전달)
    AI->>DB: 활성 구인공고 목록 조회
    DB-->>AI: 구인공고 목록 반환
    AI->>AI: 매칭 점수 산출 (경력/전문분야/근무형태 분석)
    AI-->>UI: 매칭 점수순 추천 목록 반환

    UI-->>R: 추천 구인공고 목록 표시

    R->>UI: 특정 공고 관심 표시 (피드백)
    UI->>AI: 피드백 데이터 전달
    AI->>AI: 알고리즘 개선 반영
    AI->>DB: 피드백 저장
```

### 4.2 순차 다이어그램 2: 면담 예약 프로세스

```mermaid
sequenceDiagram
    actor E as 구인자
    participant UI as 플랫폼 UI
    participant CS as 채팅 서비스
    participant IS as 면담 예약 서비스
    participant NS as 알림 서비스
    participant DB as 데이터베이스
    actor R as 은퇴자

    E->>UI: 은퇴자 프로필 확인 후 면담 요청
    UI->>CS: 채팅방 생성 요청
    CS->>DB: 채팅방 정보 저장
    DB-->>CS: 저장 완료
    CS-->>UI: 채팅방 ID 반환
    UI-->>E: 채팅방 화면 표시
    UI-->>R: 채팅방 입장 알림

    E->>UI: 면담 일정 제안 (날짜/시간/장소)
    UI->>IS: 면담 예약 생성 요청
    IS->>DB: 예약 정보 저장 (상태: 대기중)
    DB-->>IS: 저장 완료
    IS->>NS: 은퇴자에게 예약 알림 발송 요청
    NS-->>R: SMS/이메일 알림 발송

    R->>UI: 면담 일정 확인 및 수락
    UI->>IS: 예약 확정 요청
    IS->>DB: 예약 상태 업데이트 (확정)
    DB-->>IS: 업데이트 완료
    IS->>NS: 양측 확정 알림 발송 요청
    NS-->>E: 면담 확정 알림 발송
    NS-->>R: 면담 확정 알림 발송
    UI-->>E: 면담 예약 완료 화면 표시
```

### 4.3 순차 다이어그램 3: 리뷰 및 평점 등록 프로세스

```mermaid
sequenceDiagram
    actor R as 은퇴자
    participant UI as 플랫폼 UI
    participant MS as 매칭 서비스
    participant RS as 리뷰 서비스
    participant DB as 데이터베이스
    actor E as 구인자

    Note over R,E: 매칭 활동 완료 후

    R->>UI: 리뷰 작성 요청
    UI->>MS: 매칭 완료 상태 확인
    MS->>DB: 매칭 이력 조회
    DB-->>MS: 완료된 매칭 정보 반환
    MS-->>UI: 리뷰 작성 가능 확인

    UI-->>R: 리뷰 작성 폼 표시
    R->>UI: 평점 및 리뷰 내용 입력
    UI->>RS: 리뷰 저장 요청
    RS->>DB: 리뷰·평점 저장
    DB-->>RS: 저장 완료
    RS->>DB: 구인자 프로필 평균 평점 업데이트
    DB-->>RS: 업데이트 완료
    RS-->>UI: 리뷰 등록 완료
    UI-->>R: 등록 완료 메시지 표시

    Note over E: 구인자도 동일하게 은퇴자 리뷰 작성
    E->>UI: 은퇴자에 대한 리뷰 작성
    UI->>RS: 리뷰 저장 요청
    RS->>DB: 리뷰·평점 저장 및 은퇴자 평균 평점 업데이트
    DB-->>RS: 저장 완료
    RS-->>UI: 등록 완료
```

---

## 5. 상태기계 다이어그램

### 5.1 구인공고 상태 다이어그램

```mermaid
stateDiagram-v2
    [*] --> 작성중 : 구인자가 공고 작성 시작

    작성중 --> 게시중 : 공고 등록 완료 (필수항목 입력)
    작성중 --> [*] : 작성 취소

    게시중 --> 매칭진행중 : AI 매칭 후보 발생
    게시중 --> 마감 : 구인자가 수동 마감
    게시중 --> 마감 : 마감일 도래

    매칭진행중 --> 게시중 : 매칭 후보 거절 (재공개)
    매칭진행중 --> 면담중 : 면담 예약 확정
    매칭진행중 --> 마감 : 구인자가 수동 마감

    면담중 --> 채용완료 : 채용 결정
    면담중 --> 게시중 : 면담 후 불합격 (재공개)

    채용완료 --> [*] : 리뷰 등록 후 종료
    마감 --> [*] : 마감 처리 완료
```

### 5.2 은퇴자 매칭 상태 다이어그램

```mermaid
stateDiagram-v2
    [*] --> 프로필등록 : 회원가입 완료

    프로필등록 --> 매칭대기 : 프로필 공개 설정
    프로필등록 --> 프로필등록 : 프로필 수정

    매칭대기 --> 추천받음 : AI 매칭 추천 수신
    매칭대기 --> 프로필등록 : 프로필 비공개

    추천받음 --> 지원완료 : 공고에 지원
    추천받음 --> 매칭대기 : 관심없음 표시

    지원완료 --> 면담예약됨 : 구인자가 면담 수락
    지원완료 --> 매칭대기 : 구인자 거절

    면담예약됨 --> 면담완료 : 면담 진행
    면담예약됨 --> 매칭대기 : 면담 취소

    면담완료 --> 채용확정 : 최종 합격
    면담완료 --> 매칭대기 : 불합격 (재매칭)

    채용확정 --> 활동중 : 근무/활동 시작
    활동중 --> 리뷰등록 : 활동 완료
    리뷰등록 --> 매칭대기 : 추가 매칭 희망
    리뷰등록 --> [*] : 활동 종료
```

---

## 부록: 요구사항 추적 매트릭스

| 유스케이스 | 관련 기능 요구사항 | 관련 비기능 요구사항 |
|---|---|---|
| UC1 프로필 관리 | FR-001, FR-002, FR-003, FR-004 | NFR-003, NFR-004 |
| UC4 AI 매칭 추천 | FR-005, FR-007, FR-008 | NFR-001, NFR-006 |
| UC5 은퇴자 추천 | FR-006, FR-007, FR-008 | NFR-001, NFR-006 |
| UC8 구인공고 등록 | FR-009 | NFR-001, NFR-004 |
| UC9 공고 검색 | FR-010 | NFR-001, NFR-003 |
| UC11 실시간 채팅 | FR-012 | NFR-001, NFR-002 |
| UC12 면담 예약 | FR-013, FR-014 | NFR-001 |
| UC14 활동 이력 | FR-015 | NFR-004 |
| UC15 리뷰·평점 | FR-016, FR-017 | NFR-004 |
| UC17 재능기부 | FR-018, FR-019, FR-020 | NFR-003 |
| UC19 사용자 관리 | IR-004 | NFR-004 |
