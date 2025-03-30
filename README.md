# 🧠 LLM 기반 캐릭터 챗봇

> 사용자가 가상의 캐릭터와 자연스러운 대화를 나눌 수 있는 웹 챗봇 애플리케이션입니다. 각 캐릭터는 고유한 말투와 성격을 가지고 있으며, 대화 맥락과 이전 세션을 기억하여 몰입감 있는 상호작용이 가능합니다.

## 📌 주요 기능
- WebSocket 기반 실시간 채팅
- 캐릭터별 프롬프트 구성 (LangChain PromptTemplate)
- 세션 단위 대화 로그 저장 (PostgreSQL)
- React 기반 사용자 인터페이스

## 🛠 사용 기술
<a href="https://fastapi.tiangolo.com/" target="_blank" rel="noreferrer"><img src="https://raw.githubusercontent.com/danielcranney/readme-generator/main/public/icons/skills/fastapi-colored.svg" width="36" height="36" alt="Fast API" /></a>
<a href="https://www.postgresql.org/" target="_blank" rel="noreferrer"><img src="https://raw.githubusercontent.com/danielcranney/readme-generator/main/public/icons/skills/postgresql-colored.svg" width="36" height="36" alt="PostgreSQL" /></a>
<a href="https://reactjs.org/" target="_blank" rel="noreferrer"><img src="https://raw.githubusercontent.com/danielcranney/readme-generator/main/public/icons/skills/react-colored.svg" width="36" height="36" alt="React" /></a>
- langchain
- websocket
---

## 📁 프로젝트 구조
```
gganbu-chatbot/
├── back/                             # FastAPI 기반 메인 백엔드
│   ├── app/
│   │   ├── main.py                  # FastAPI 진입점
│   │   ├── routers/                 # API 라우터 (chat, user 등)
│   │   ├── models/                  # SQLAlchemy 모델 정의
│   │   ├── schemas/                 # Pydantic 스키마 (요청/응답용)
│   │   ├── database/                # DB 연결 및 세션 관리
│   │   ├── core/                    # 환경 설정, 의존성 주입 등 핵심 설정
│   │   ├── utils/                   # 공통 유틸 함수
│   │   └── init_db.py              # DB 초기화 스크립트
│   ├── Dockerfile                  # 백엔드용 도커 설정
│   └── requirements.txt            # 백엔드 의존성 목록
│
├── back-langchain/                  # LangChain 기반 LLM 처리 서버
│   ├── app/
│   │   ├── main.py                 # LangChain 진입점
│   │   ├── openai_api.py          # 프롬프트 구성 및 OpenAI 호출 로직
│   │   └── database.py            # 간단한 세션/로그 저장용 DB 연결
│   ├── Dockerfile                 # LangChain 서버 도커 설정
│   └── requirements.txt           # LangChain 서버 의존성 목록
│
├── frontend/                        # React 기반 사용자 인터페이스
│   ├── public/                     # 정적 파일 (HTML, favicon 등)
│   ├── src/
│   │   ├── components/             # 재사용 가능한 UI 컴포넌트
│   │   ├── pages/                  # 주요 페이지 컴포넌트
│   │   ├── assets/                 # 이미지 및 정적 리소스
│   │   ├── App.js                  # 루트 컴포넌트
│   │   └── index.js                # 엔트리포인트
│   ├── Dockerfile                 # 프론트엔드용 도커 설정
│   └── nginx.conf                 # Nginx 설정 (프론트 배포용)
│
└── README.md                       # 프로젝트 설명 문서
```

## 메인 백엔드 서버 시작하는 방법

- 1. venv 가상환경 생성 (python 버전 3.10 사용)<br>
`python3.10 -m venv gganbu`

- 2. 가상환경 활성화<br>
`source gganbu/bin/activate`

- 3. .env 파일 설정<br>
```
DB_HOST=<your_db_host>
DB_USER=<your_db_user>
DB_PASS=<your_db_password>
DB_PORT=<your_db_port>
DB_NAME=<your_db_name>

OPEN_AI_KEY=<your-openai-key>

CLIENT_DOMAIN=http://localhost:3000
WS_SERVER_DOMAIN=ws://localhost:8001
```

- 4. pip update 및 필수 라이브러리 설치<br>
`pip install --upgrade pip`
`pip install -r requirements.txt`

- 6. DB 생성 (초기 한번 실행)<br>
`python app/init_db.py`

- 7. 백엔드 실행(uvicorn 이용)<br>
`uvicorn app.main:app --reload --port 8000`

---

## 랭체인 서버 시작하는 방법
- 1. venv 가상환경 생성 (python 버전 3.12 사용)<br>
`python3 -m venv gganbu_lang`

- 2. 가상환경 활성화<br>
`source gganbu_lang/bin/activate`

- 3. .env 파일 설정<br>
```
DB_HOST=<your_db_host>
DB_USER=<your_db_user>
DB_PASS=<your_db_password>
DB_PORT=<your_db_port>
DB_NAME=<your_db_name>

OPEN_AI_KEY=<your-openai-key>

CLIENT_DOMAIN=http://localhost:3000
```

- 4. pip update 및 필수 라이브러리 설치<br>
`pip install --upgrade pip`
`pip install -r requirements.txt`

- 5. app 디렉토리로 이동<br>
`cd app`

- 6. 백엔드 실행(uvicorn 이용)<br>
`uvicorn main:app --reload --port 8001`

---

## 프론트 시작하는 방법

- 1. env파일 설정
```
REACT_APP_SERVER_DOMAIN=http://localhost:8000
REACT_APP_LANGCHAIN_SERVER_DOMAIN=http://localhost:8001
REACT_APP_WS_SERVER_DOMAIN=ws://localhost:8001
```

- 2. 필수 모듈 설치<br>
`npm -i`

- 3. react 실행<br>
`npm start`
