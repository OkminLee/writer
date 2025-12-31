# LinkedIn Writer Agent System

Claude Code 기반의 글쓰기 에이전트 시스템입니다. 사용자의 글쓰기 스타일을 학습하고, 피드백을 자동으로 축적하여 점점 더 정확한 글을 작성합니다.

## 특징

- **스타일 학습**: 기존 글을 분석하여 스타일 가이드 자동 생성
- **피드백 루프**: 피드백이 파일에 저장되어 세션 간 학습 유지
- **품질 검증**: 80점 미만 글은 자동으로 수정 요청
- **4개 에이전트 협업**: 분석, 작성, 검토, 학습 역할 분리

## 구조

```
.claude/
├── agents/
│   ├── style-analyzer.md      # 스타일 분석
│   ├── content-writer.md      # 글 작성
│   ├── content-reviewer.md    # 품질 검토
│   └── style-learner.md       # 피드백 학습
│
└── skills/
    └── linkedin-writer/
        ├── SKILL.md           # 메인 스킬
        └── data/
            ├── style-guide.md     # 스타일 가이드
            ├── feedback-log.md    # 피드백 로그
            ├── samples/           # 학습용 샘플
            ├── approved-posts/    # 승인된 글
            └── style-history/     # 가이드 버전 이력
```

## 워크플로우

```
Writer 글 작성 → Reviewer 검토 → [80점 미만: 수정] → 사용자 확인
                                  [80점 이상: 제출]
     ↑                                                    │
     └──── Learner가 피드백 학습 ←── 사용자 피드백 ←──────┘
```

## 시작하기

### 1. 샘플 글 추가

`.claude/skills/linkedin-writer/data/samples/` 폴더에 기존 글 추가:

```
samples/
├── 01-제목.md
├── 02-제목.md
└── ...
```

최소 3개, 권장 10개 이상

### 2. 스타일 분석

```
스타일 분석해줘
```

### 3. 글 작성

```
/linkedin-writer AI 시대의 개발자 역량
```

### 4. 피드백

- `"좋아"` / `"승인"` → 글 저장, 학습 데이터로 활용
- `"이 표현 별로야"` → 피해야 할 표현으로 기록
- `"더 구체적으로"` → 수정 후 다시 제출

## 에이전트 역할

| 에이전트 | 역할 | 입력 | 출력 |
|---------|------|------|------|
| Style Analyzer | 스타일 패턴 추출 | samples/ | style-guide.md |
| Content Writer | 글 작성 | 주제, style-guide.md | 초안 |
| Content Reviewer | 품질 검토 | 초안, style-guide.md | 점수, 피드백 |
| Style Learner | 피드백 학습 | 사용자 피드백 | feedback-log.md 업데이트 |

## 팁

- **피드백은 구체적으로**: "별로야" 보다 "너무 딱딱해"가 효과적
- **샘플은 일관되게**: 비슷한 톤의 글들로 구성
- **승인된 글이 쌓일수록**: 스타일 가이드가 정교해짐

## 모바일 사용

이 저장소를 GitHub에 push하고, Claude 모바일 앱에서 연결하면 동일한 시스템 사용 가능. 모바일에서의 피드백도 GitHub에 반영되어 학습이 이어집니다.

## License

MIT
