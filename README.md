# OpenClaw × 서강대 API 설정 가이드

서강대학교 AI API(MindLogic)를 OpenClaw에 연결하는 설정 파일입니다.
텔레그램 봇으로 GPT-5.1, Claude Sonnet/Opus, Grok 4를 무료(서강대 크레딧)로 사용할 수 있습니다.

## 사전 준비

1. [OpenClaw 설치](https://openclaws.io)
2. 서강대 AI API 키 발급 (서강대 포털 → 개발자 설정)
3. 텔레그램 봇 생성 (@BotFather → /newbot)

## 설치 방법

### 1. 설정 파일 복사
```bash
cp openclaw.json ~/.openclaw/openclaw.json
```

### 2. API 키 입력
`~/.openclaw/openclaw.json` 파일에서 아래 두 곳을 교체:

| 항목 | 교체할 값 |
|------|----------|
| `YOUR_SOGANG_API_KEY` | 서강대 포털에서 발급받은 API 키 |
| `YOUR_TELEGRAM_BOT_TOKEN` | @BotFather에서 발급받은 봇 토큰 |

### 3. 게이트웨이 시작
```bash
openclaw gateway install
```

### 4. 텔레그램 연결
텔레그램에서 본인 봇 검색 → `/start`

## 사용 가능한 모델

| 모델 | 특징 |
|------|------|
| GPT-5.1 (기본) | 빠르고 범용적 |
| GPT-5 Mini | 가볍고 빠름 |
| Grok 4 | 추론 특화 |
| Claude Sonnet 4.5 | 코딩/분석 강함 |
| Claude Opus 4.5 | 최고 성능 |

## 모델 전환
```bash
# 텔레그램에서
/model sogang-claude/claude-sonnet-4-5-20250929

# 터미널에서
openclaw models set sogang-gpt/grok-4
```
