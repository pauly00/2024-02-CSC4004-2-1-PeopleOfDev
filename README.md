# MomentUp

**2024년 2학기 공개SW프로젝트02 1조 개발의 민족**

## 프로젝트 소개

**MomentUp**은 랜덤 알림 기반의 실시간 활동 인증 커뮤니티 앱입니다. 사용자는 지정한 시간대에 랜덤으로 울리는 알림을 받아 제한 시간 내에 현재 활동을 촬영·업로드하고, AI(YOLOv5)가 해당 활동을 분석·인증합니다. 그룹과 챌린지를 통해 서로 동기부여하며 건강한 디지털 습관을 형성합니다.

---

## 디렉토리 구조

```
.
├── MomentUp/                   # Android 클라이언트 앱 (Java / Kotlin)
│   └── app/src/main/java/com/example/momentup/
│       ├── Activity             # 로그인, 회원가입, 알람, 그룹, 알림 화면
│       ├── Fragment             # 홈, 캘린더, 챌린지, 마이페이지, 카메라
│       ├── api/                 # Retrofit API 인터페이스
│       ├── service/             # FCM 메시징 서비스
│       ├── dto, model,          # 데이터 모델 및 요청/응답 DTO
│       │   request, response
│       └── ...
│
├── src/                        # Spring Boot 백엔드 서버 (Java 21)
│   └── main/java/com/api/momentup/
│       ├── api/                 # REST 컨트롤러 (7개)
│       ├── domain/              # JPA 엔티티
│       ├── service/             # 비즈니스 로직
│       ├── repository/          # JPA / QueryDSL 레포지토리
│       ├── dto/                 # 요청·응답 DTO
│       ├── exception/           # 커스텀 예외
│       └── config/              # Firebase, Quartz, Swagger, CORS 설정
│
└── yolo_object_detection/      # AI 분석 서버 (Python / FastAPI)
    ├── main.py                  # FastAPI 앱 진입점
    └── app/
        ├── routes.py            # POST /api/predict_images
        ├── yolo_service.py      # YOLOv5 추론·활동 판별
        └── settings.py          # 업로드 디렉토리 설정
```

---

## 주요 기능

### 인증 / 회원
- 회원가입 (프로필 이미지 포함), 로그인
- 아이디·비밀번호 찾기 (이메일 발송)
- 팔로우 요청 / 수락 / 거절 / 언팔로우

### 홈 피드
- 팔로우한 사용자의 최신 게시물 피드
- 소속 그룹 목록 및 각 그룹 최신 게시물 시간 확인

### 실시간 활동 인증
- Quartz 스케줄러로 사용자가 설정한 시간대 내 랜덤 FCM 푸시 알림 발송
- 알림 수신 후 제한 시간 내 사진 촬영·업로드
- YOLOv5 AI 서버(`POST /api/predict_images`)가 사진 분석 후 활동 유형 반환

### 알람 관리
- 알람 생성·수정·삭제·활성화/비활성화
- 그룹별 또는 전체 알람 설정

### 캘린더
- 날짜별 업로드 성공 여부 시각화 (초록 = 성공, X = 미업로드)
- 날짜 선택 시 해당 날 업로드 목록 표시
- 마이페이지에서 개인 활동 달력 확인

### 그룹 커뮤니티
- 그룹 생성 (이름, 해시태그, 소개, 썸네일)
- 초대 코드로 가입 신청 → 그룹장 승인/거절
- 그룹 인기 게시물 / 최신 게시물 조회
- 그룹 멤버 목록·강제 퇴장

### 게시물
- 사진 게시물 작성 (개인 / 그룹)
- 좋아요 추가·취소·개수 조회
- 댓글 작성, 댓글 좋아요·취소

### 챌린지
- 챌린지 생성 (7일, 30일 등 타입 지정, 대표 이미지)
- 챌린지 목록·상세·캘린더 기록 조회
- 챌린지 삭제

### 검색
- 키워드로 유저·그룹 통합 검색
- 최근 검색 기록 조회·삭제

### 알림 센터
- 팔로우 요청, 그룹 가입 요청 등 앱 내 알림 목록

---

## 기술 스택

| 구분 | 기술 |
|------|------|
| Android 클라이언트 | Java, Kotlin, Android SDK 34, CameraX, Retrofit2, Glide, MaterialCalendarView, FCM |
| 백엔드 | Java 21, Spring Boot, Spring Data JPA, QueryDSL, Quartz Scheduler |
| 데이터베이스 | MySQL |
| AI 분석 서버 | Python, FastAPI, YOLOv5 |
| 푸시 알림 | Firebase Cloud Messaging (FCM) |
| API 문서 | Swagger (SpringDoc OpenAPI) |

---

## API 엔드포인트 요약

| 컨트롤러 | Base URL | 주요 기능 |
|----------|----------|-----------|
| UserApiController | `/api/v1/users` | 회원가입, 로그인, 팔로우, 그룹 목록 |
| GroupsController | `/api/v1/group` | 그룹 CRUD, 가입·탈퇴, 게시물 조회 |
| PostController | `/api/v1/posts` | 게시물·댓글 CRUD, 좋아요 |
| AlarmController | `/api/v1/alarm` | 알람 CRUD, 상태 변경 |
| ChallengeController | `/api/v1/challenge` | 챌린지 CRUD, 상세 조회 |
| CalendarController | `/api/v1/calendar` | 캘린더 기록 조회 |
| SearchController | `/api/v1/search` | 유저·그룹 검색, 검색 기록 |
| YOLO 서버 | `/api/predict_images` | 이미지 업로드 → 활동 인증 결과 반환 |

---

## 참여 인원

| 이름 | 역할 | 전공 |
|------|------|------|
| 이재형 | 팀장 / 프론트엔드 | 컴퓨터공학전공 |
| 노선우 | 팀원 / 프론트엔드 | 컴퓨터공학전공 |
| 류경록 | 팀원 / 백엔드 | 컴퓨터공학전공 |
| 백염용 | 팀원 / 프론트엔드 | 컴퓨터공학전공 |
| 윤은진 | 팀원 / 디자인, PM, 백엔드 | 컴퓨터공학전공 |
