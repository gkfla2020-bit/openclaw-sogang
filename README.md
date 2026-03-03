# OpenClaw × 서강대 API 설정 가이드

서강대학교에서 제공하는 AI API(MindLogic 플랫폼)를 [OpenClaw](https://openclaws.io)에 연결하는 설정 파일입니다.

텔레그램 봇 하나로 내 맥북을 AI 에이전트로 만들 수 있습니다. 파일 관리, 코드 실행, 웹 검색, 브라우저 제어까지 텔레그램 채팅으로 명령할 수 있습니다.

서강대 학생이라면 학교에서 제공하는 크레딧으로 **GPT-5.1, Claude Sonnet/Opus 4.5, Grok 4** 를 무료로 사용할 수 있습니다.

---

## 어떻게 동작하나요?

```
텔레그램 메시지 → OpenClaw 게이트웨이 (내 맥북) → AI 모델 → 응답
```

OpenClaw는 내 맥북에서 백그라운드로 실행되는 AI 에이전트입니다. 텔레그램에서 메시지를 보내면 AI가 직접 내 컴퓨터에서 작업을 수행합니다.

**할 수 있는 것들:**
- 파일 읽기/쓰기/정리
- Python 스크립트 실행
- 웹 검색 및 브라우저 제어
- 코드 작성 및 수정
- 일반 질문/대화

---

## 사전 준비

### 1. OpenClaw 설치

```bash
# Homebrew가 없다면 먼저 설치
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Node.js 설치
brew install node@22
echo 'export PATH="/opt/homebrew/opt/node@22/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# OpenClaw 설치
npm install -g openclaw
```

### 2. 서강대 API 키 발급

1. 서강대 AI 플랫폼 접속 (학교 포털에서 확인)
2. 개발자 설정 → API 키 생성
3. 발급된 키 복사

### 3. 텔레그램 봇 생성

1. 텔레그램에서 **@BotFather** 검색
2. `/newbot` 입력
3. 봇 이름 입력 (예: `My AI Assistant`)
4. 봇 username 입력 (예: `myai_bot`) — 반드시 `_bot`으로 끝나야 함
5. 발급된 토큰 복사 (예: `123456789:ABCdef...`)

---

## 설치 방법

### 1. 설정 파일 복사

```bash
mkdir -p ~/.openclaw
curl -o ~/.openclaw/openclaw.json https://raw.githubusercontent.com/gkfla2020-bit/openclaw-sogang/main/openclaw.json
```

또는 이 레포를 클론:

```bash
git clone https://github.com/gkfla2020-bit/openclaw-sogang.git
cp openclaw-sogang/openclaw.json ~/.openclaw/openclaw.json
```

### 2. API 키 입력

`~/.openclaw/openclaw.json` 파일을 텍스트 에디터로 열고 아래 두 항목을 교체합니다.

```json
"apiKey": "YOUR_SOGANG_API_KEY"       ← 서강대 API 키로 교체 (2곳)
"botToken": "YOUR_TELEGRAM_BOT_TOKEN" ← 텔레그램 봇 토큰으로 교체
```

### 3. 게이트웨이 시작

```bash
openclaw gateway install
```

맥 재시작 후에도 자동으로 실행됩니다.

### 4. 텔레그램 연결

텔레그램에서 본인이 만든 봇을 검색 → `/start` 입력

봇이 응답하면 연결 완료입니다.

---

## 사용 가능한 모델

| 모델 | 특징 | 추천 용도 |
|------|------|----------|
| `sogang-gpt/gpt-5.1-chat-latest` (기본) | 빠르고 범용적 | 일반 대화, 코딩 |
| `sogang-gpt/gpt-5-mini` | 가볍고 빠름 | 간단한 질문 |
| `sogang-gpt/grok-4` | 추론 특화 | 복잡한 분석 |
| `sogang-claude/claude-sonnet-4-5-20250929` | 코딩/분석 강함 | 코드 작성, 리서치 |
| `sogang-claude/claude-opus-4-5-20251101` | 최고 성능 | 복잡한 작업 |

### 모델 전환 방법

텔레그램 채팅에서:
```
/model sogang-claude/claude-sonnet-4-5-20250929
```

터미널에서:
```bash
openclaw models set sogang-gpt/grok-4
openclaw models list  # 현재 모델 확인
```

---

## 자주 쓰는 명령어

```bash
openclaw gateway install   # 게이트웨이 시작 (자동 실행 등록)
openclaw gateway stop      # 게이트웨이 중지
openclaw channels status   # 텔레그램 연결 상태 확인
openclaw models list       # 사용 가능한 모델 목록
openclaw approvals allowlist add --agent "*" "*"  # 모든 도구 자동 승인
```

---

## 주의사항

- 맥북이 켜져 있어야 텔레그램 봇이 응답합니다 (잠자기 모드 제외)
- API 키는 절대 공개 레포에 올리지 마세요
- 파일 삭제 등 되돌릴 수 없는 작업은 신중하게 요청하세요
- 서강대 API 크레딧은 계정별로 부여되므로 본인 키를 직접 발급받아야 합니다
