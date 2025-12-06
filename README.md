# LMS (Learning Management System)

## 프로젝트 소개

채용 연계형 국비 교육 과정에서 사용하기 위해 개발한 자사 서버 기반 학습 관리 시스템입니다.
교수님의 요청으로 실제 국비 교육생들이 사용할 수 있는 실용적인 LMS를 목표로 개발되었습니다.

**개발 기간**: 2025년 9월 (1개월)
**사용 현황**: 국비 교육생 대상 실제 운영

## 주요 기능

### 사용자 역할별 기능

#### 학생 (Student)
- 수강 강좌 대시보드
- 강의 자료 시청 및 다운로드
- 과제 제출 및 채점 확인
- 토론 게시판 참여
- 공지사항 확인
- ArUco 마커 기반 출석 체크
- 멘토링 팀 활동 및 프로젝트 관리

#### 교수/강사 (Instructor)
- 강좌 생성 및 관리
- 강의 자료 업로드 (비디오, 파일, 외부 링크)
- 과제 출제 및 채점
- 학생 제출물 관리
- 출석 세션 생성 및 관리
- 학습 분석 및 리포트

#### 관리자 (Admin)
- 사용자 관리
- 강좌 전체 관리
- 시스템 설정
- 통계 및 분석

### 핵심 기능

#### 1. 강좌 관리
- 강좌 생성, 수정, 삭제
- 수강생 등록 관리
- 주차별 강의 자료 구성

#### 2. 학습 자료 시스템
- 다양한 형식 지원 (비디오, PDF, 문서, 외부 링크)
- 비디오 스트리밍 및 재생 추적
- YouTube 임베드 지원
- 다운로드 권한 설정

#### 3. 과제 관리
- 과제 출제 및 제출 마감일 설정
- 파일 첨부 지원 (BLOB 저장)
- 자동 지각 제출 판정
- 채점 및 피드백 시스템

#### 4. 출석 관리 (ArUco 기반)
- ArUco 마커를 활용한 자동 출석 체크
- 교수 카메라 또는 학생 셀프 태깅
- 출석 세션 생성 및 시간 관리
- 출석 현황 통계

#### 5. 커뮤니케이션
- 공지사항 시스템
- 토론 게시판 (댓글, 대댓글, 좋아요)
- 개인 메시지 시스템
- 자유 게시판

#### 6. 멘토링 시스템
- 팀 생성 및 관리
- 프로젝트 관리
- 태스크 할당 및 추적
- 멘토링 리포트 작성
- 공모전 참가 관리

## 기술 스택

### Backend
- **Framework**: Flask 3.0.3
- **ORM**: SQLAlchemy 2.0.41
- **Database**: MySQL/MariaDB (PyMySQL)
- **Migration**: Alembic, Flask-Migrate
- **Authentication**: Flask-JWT-Extended, bcrypt

### Computer Vision
- **OpenCV**: opencv-contrib-python 4.12
- **ArUco 마커**: 출석 체크 시스템

### Others
- **Redis**: 캐싱 및 세션 관리
- **Pillow**: 이미지 처리
- **python-dotenv**: 환경 변수 관리

## 설치 및 실행

### 1. 요구사항
- Python 3.9+
- MySQL 5.7+ 또는 MariaDB 10.2+
- Redis (선택)

### 2. 설치

```bash
# 저장소 클론
git clone <repository-url>
cd lms

# 가상환경 생성 및 활성화
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 의존성 설치
pip install -r requirements.txt
```

### 3. 환경 변수 설정

`.env` 파일 생성:

```env
# Database
DB_USER=wsuser
DB_PASS=wsuser!
DB_HOST=127.0.0.1
DB_PORT=3306
DB_NAME=lms_db

# Flask
FLASK_SECRET_KEY=your-secret-key-here

# Upload
MAX_CONTENT_MB=512

# ArUco
ATTENDANCE_QR_SESSION_MINUTES=10
```

### 4. 데이터베이스 초기화

```bash
# 데이터베이스 생성
mysql -u root -p
CREATE DATABASE lms_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

# 마이그레이션 실행
flask db upgrade
```

### 5. 실행

```bash
python app.py
```

서버가 `http://localhost:5004`에서 실행됩니다.

## 프로젝트 구조

```
lms/
├── app.py                      # 앱 팩토리 및 블루프린트 등록
├── config.py                   # 설정
├── models.py                   # 데이터베이스 모델
├── extensions.py               # Flask 확장
├── blueprints/                 # 기능별 블루프린트
│   ├── auth/                  # 인증
│   ├── dashboard/             # 대시보드
│   ├── courses/               # 강좌 관리
│   ├── course_detail/         # 강좌 상세
│   ├── assignments/           # 과제
│   ├── materials/             # 강의 자료
│   ├── notices/               # 공지사항
│   ├── discussion/            # 토론
│   ├── board/                 # 게시판
│   ├── messages/              # 메시지
│   ├── mentoring/             # 멘토링
│   ├── attendance/            # 출석
│   ├── users/                 # 사용자 관리
│   ├── profile/               # 프로필
│   └── uploads/               # 파일 업로드
├── services/                   # 비즈니스 로직
│   ├── aruco_engine.py        # ArUco 마커 생성/인식
│   ├── aruco_identity_system.py  # 출석 신원 확인
│   └── metrics.py             # 학습 분석
├── helpers/                    # 헬퍼 함수
│   ├── auth.py                # 인증 헬퍼
│   ├── roles.py               # 권한 관리
│   ├── storage.py             # 파일 저장
│   └── utils.py               # 유틸리티
├── templates/                  # Jinja2 템플릿
├── static/                     # 정적 파일
│   ├── uploads/               # 업로드된 파일
│   └── markers/               # ArUco 마커 이미지
└── data/                       # 데이터 파일
    └── aruco_identities.json  # ArUco 마커-사용자 매핑
```

## 주요 모델

### 사용자 및 강좌
- `User`: 사용자 (학생, 교수, 관리자)
- `Course`: 강좌
- `Enrollment`: 수강 신청

### 학습 자료
- `Material`: 강의 자료 (비디오, 파일, 링크)
- `MaterialBlob`: 비디오 BLOB 저장
- `MaterialEvent`: 자료 재생/다운로드 로그

### 과제
- `Assignment`: 과제
- `Submission`: 과제 제출물

### 커뮤니케이션
- `Notice`: 공지사항
- `DiscussionThread`: 토론 스레드
- `DiscussionComment`: 토론 댓글
- `Message`: 개인 메시지
- `Board`, `Post`: 게시판

### 멘토링
- `MentoringTeam`: 멘토링 팀
- `Project`: 프로젝트
- `ProjectTask`: 프로젝트 태스크
- `Competition`: 공모전

### 출석
- `AttendanceSession`: 출석 세션
- `UserMarker`: 사용자 ArUco 마커
- `AttendanceDetection`: 마커 인식 기록
- `AttendanceRecord`: 출석 기록

## 주요 특징

### ArUco 마커 기반 출석 시스템
- OpenCV ArUco 라이브러리를 활용한 자동 출석 체크
- 각 학생에게 고유 마커 할당
- 교수 카메라 또는 학생 셀프 촬영으로 출석 인증
- 출석 세션별 시간 제한 및 상태 관리

### 학습 분석
- 강의 자료 재생 시간 추적
- 과제 제출 현황 및 성적 통계
- 주차별 학습 진도 모니터링

### 파일 관리
- 다중 업로드 방식 지원 (로컬 저장, 외부 URL)
- 대용량 파일 지원 (최대 512MB)
- 안전한 파일명 처리 및 MIME 타입 검증

## 스크린샷

### 로그인
![로그인](ex/로그인.png)
<img width="640" height="732" alt="image" src="https://github.com/user-attachments/assets/04b72dda-fea4-409c-8a81-473d8c9f9f5b" />

### 메인 페이지
<img width="1878" height="881" alt="image" src="https://github.com/user-attachments/assets/8788f23d-1caa-475a-a525-2350cff1c84d" />

![메인 페이지](ex/메인%20페이지(1).png)
![메인 페이지 2](ex/메인%20페이지(2).png)

### 강좌 관리
![강좌 목록](ex/강좌%20페이지.png)
<img width="1512" height="843" alt="image" src="https://github.com/user-attachments/assets/8771b37d-7fa3-4f43-979d-c9ccbdbebed2" />

![강의 자료](ex/강좌%20페이지%20-%20강좌%20보기(강의%20자료).png)
![과제](ex/강좌%20페이지%20-%20강좌%20보기(과제).png)
![토론](ex/강좌%20페이지%20-%20강좌%20보기(토론).png)

### 멘토링 관리
![분석 1](ex/분석%20및%20리포트(1).png)
![분석 2](ex/분석%20및%20리포트(2).png)
<img width="1586" height="443" alt="image" src="https://github.com/user-attachments/assets/764098d5-67d4-46cc-9afb-86ef76eaff61" />


### 사용자 관리
![사용자 관리](ex/사용자%20관리%20페이지.png)
<img width="1552" height="717" alt="image" src="https://github.com/user-attachments/assets/fc234d1a-7f4e-4a03-a9e5-d9c4a02f9237" />

### 프로필
![프로필](ex/프로필.png)

## 보안

- 비밀번호 해싱 (bcrypt)
- 세션 기반 인증
- CSRF 보호
- SQL Injection 방지 (SQLAlchemy ORM)
- 파일 업로드 검증
- 역할 기반 접근 제어 (RBAC)

