# eduChatBot

Anthropic Claude API를 활용한 교육용 AI 챗봇 애플리케이션입니다. Google Sheets와 연동하여 학생들과의 대화 내용을 자동으로 기록하고, 종합 평가를 생성합니다.

## 주요 기능

- **AI 대화**: Anthropic Claude API를 사용한 실시간 대화
- **자동 기록**: Google Sheets에 모든 대화 내용 자동 저장
- **사용자 관리**: 학생별 개별 워크시트 자동 생성
- **스트리밍 응답**: 실시간으로 AI 응답 표시
- **평가 기능**: 대화 종료 시 자동으로 종합 평가 및 평어 생성
- **관리자 설정**: Google Sheets를 통한 중앙 집중식 설정 관리

## 시스템 요구사항

- Python 3.8 이상
- Streamlit
- Anthropic API 키
- Google Service Account (Google Sheets API 접근용)

## 설치 방법

1. 저장소 클론
```bash
git clone <repository-url>
cd ddd02
```

2. 필요한 패키지 설치
```bash
pip install -r requirements.txt
```

3. Streamlit secrets 설정
`.streamlit/secrets.toml` 파일을 생성하고 다음 정보를 입력하세요:

```toml
# Google Service Account 정보
type = "service_account"
project_id = "your-project-id"
private_key_id = "your-private-key-id"
private_key = "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
client_email = "your-service-account@your-project.iam.gserviceaccount.com"
client_id = "your-client-id"
auth_uri = "https://accounts.google.com/o/oauth2/auth"
token_uri = "https://oauth2.googleapis.com/token"
auth_provider_x509_cert_url = "https://www.googleapis.com/oauth2/v1/certs"
client_x509_cert_url = "https://www.googleapis.com/robot/v1/metadata/x509/..."
universe_domain = "googleapis.com"

# 설정 시트 URL
sheet_url = "https://docs.google.com/spreadsheets/d/YOUR_SETUP_SHEET_ID"
```

## Google Sheets 설정

### 1. 설정 시트 구조
설정 시트에는 "정보" 워크시트가 있어야 하며, 다음 정보를 B열에 포함해야 합니다:

| 행 | 항목 | 설명 |
|---|------|------|
| 1 | 수업 시트 URL | 학생 대화를 저장할 시트 URL |
| 2 | 서비스 상태 | "on" 또는 "off" |
| 3 | AI 서비스 | "Anthropic" |
| 4 | API 키 | Anthropic API 키 |
| 5 | 모델명 | 예: "claude-3-5-sonnet-20241022" |
| 6 | Max Tokens | 예: 4096 |
| 7 | Temperature | 예: 0.7 |
| 8 | Select | 선택 옵션 |
| 9 | System Prompt | AI 시스템 프롬프트 |
| 10 | 종합 평가 프롬프트 | 평가 생성용 프롬프트 |
| 11 | 평어 프롬프트 | 평어 생성용 프롬프트 |
| 12 | Stream | "true" 또는 "false" |

### 2. 수업 시트 구조
수업 시트에는 다음 워크시트가 필요합니다:
- **템플릿**: 새 학생 시트 생성 시 복사할 템플릿
- **수업요약**: 모든 학생의 요약 정보 및 평가

## 실행 방법

```bash
streamlit run app.py
```

브라우저에서 자동으로 애플리케이션이 열립니다.

## 사용 방법

1. **대화명 입력**: 사이드바에서 학생의 대화명(닉네임)을 입력합니다.
2. **대화 시작**: 채팅 입력창에 메시지를 입력하여 AI와 대화합니다.
3. **대화 종료**: "대화 종료" 버튼을 클릭하면 자동으로 종합 평가가 생성됩니다.

## 프로젝트 구조

```
ddd02/
├── app.py              # 메인 애플리케이션
├── requirements.txt    # Python 의존성
├── README.md          # 프로젝트 문서
└── utils/
    └── gs.py          # Google Sheets 유틸리티
```

## 주요 함수 설명

### app.py
- `initialize()`: API 클라이언트 및 Google Sheets 초기화
- `execute_prompt()`: AI에 프롬프트 전송 및 응답 받기
- `message_processing()`: 스트리밍 응답 처리
- `end_conversation()`: 대화 종료 및 평가 생성

### utils/gs.py
- `get_authorize()`: Google Sheets API 인증
- `get_setup_info()`: 설정 정보 가져오기
- `get_worksheet()`: 학생별 워크시트 가져오기/생성
- `add_Content()`: 대화 내용 저장

## 보안 고려사항

- API 키와 인증 정보는 절대 코드에 하드코딩하지 마세요.
- `.streamlit/secrets.toml` 파일은 `.gitignore`에 추가하세요.
- Google Service Account는 필요한 최소 권한만 부여하세요.

## 라이선스

MIT License

## 기여

버그 리포트 및 기능 제안은 이슈로 등록해주세요.