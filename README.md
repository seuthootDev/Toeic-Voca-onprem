# TOEIC 단어장 (온 프레미스 배포용)

TOEIC 단어 학습을 위한 웹 애플리케이션입니다. <br>사용자별 학습 이력을 기반으로 효율적인 단어 학습을 제공합니다.<br>
본 저장소는 온 프레미스 배포용입니다.

Vercel 배포용 저장소 : https://github.com/seuthootDev/Toeic-Voca <br>
AWS 클라우드 배포용 저장소 : https://github.com/seuthootDev/Toeic-Voca-AWS

## 주요 기능

- 사용자별 맞춤형 단어 학습
- 학습 이력 기반의 가중치 시스템
- 반응형 디자인으로 모바일/데스크톱 지원
- 사용자 인증 시스템 (로그인/회원가입)

## 기술 스택

- **프론트엔드**
  - Next.js 13 (App Router)
  - TypeScript
  - Tailwind CSS
  - React Hooks
  - Shadcn/ui 컴포넌트

- **백엔드**
  - Next.js API Routes
  - MongoDB
  - JWT 인증

- **인프라**
  - Docker
  - Docker Compose
  - Nginx (리버스 프록시)

## 프로젝트 구조

```
toeic-voca-onprem/
├── app/                    # Next.js 13 App Router
│   ├── login/             # 로그인 페이지
│   ├── register/          # 회원가입 페이지
│   └── dashboard/         # 대시보드 페이지
├── components/            # 재사용 가능한 컴포넌트
├── lib/                   # 유틸리티 함수
├── pages/                 # 서버리스 API
├── public/                # 정적 파일
├── docker-compose.yml     # Docker Compose 설정
└── Dockerfile            # 애플리케이션 Dockerfile
```

## Docker 구조

### 컨테이너 구성
- **app**: Next.js 애플리케이션 서버
- **nginx**: 리버스 프록시 서버
- **mongodb**: 데이터베이스 서버

### 네트워크 구성
- 모든 컨테이너는 동일한 Docker 네트워크에 연결
- Nginx가 80/443 포트를 통해 외부 접근을 처리
- 내부 통신은 Docker 네트워크를 통해 이루어짐

## 시작하기

### 필수 조건
- Docker
- Docker Compose

### 설치 및 실행

1. 저장소 클론
```bash
git clone https://github.com/seuthootDev/Toeic-Voca-onprem.git
cd Toeic-Voca-onprem
```

2. Docker 이미지 빌드 및 컨테이너 실행
```bash
docker-compose build
docker-compose up -d
```

2.1. MongoDB 데이터 초기화
- MongoDB Compass 또는 MongoDB CLI를 사용하여 데이터를 초기화합니다.
- MongoDB Compass 사용 시:
  1. MongoDB Compass를 설치하고 실행
  2. 연결 문자열: `mongodb://localhost:27017`
  3. `toeic-voca` 데이터베이스 생성
  4. 필요한 컬렉션(users, words 등) 생성 및 초기 데이터 입력

- MongoDB CLI 사용 시:
```bash
# MongoDB CLI 접속
mongosh mongodb://localhost:27017

# 데이터베이스 생성 및 사용
use toeic-voca

# 컬렉션 생성 및 데이터 입력 예시
db.words.insertMany([
  { word: "example", meaning: "예시", level: 1 },
  { word: "test", meaning: "시험", level: 2 }
])
```

3. 애플리케이션 접속
- http://localhost 또는 https://localhost (SSL 설정 시)

### 환경 변수 설정
`.env` 파일을 생성하고 다음 변수들을 설정합니다:
```
MONGODB_URI=mongodb://mongodb:27017/toeic-voca
JWT_SECRET=your_jwt_secret
```

## API 엔드포인트

- `POST /api/login` - 사용자 로그인
- `POST /api/register` - 사용자 회원가입
- `GET /api/words` - 학습할 단어 목록 조회 