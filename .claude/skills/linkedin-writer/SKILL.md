# LinkedIn Writer Skill

LinkedIn 글을 사용자의 스타일로 작성하는 에이전트 팀입니다.

## 설명

이 스킬은 4개의 에이전트가 협업하여 글을 작성합니다:
- **Style Analyzer**: 스타일 분석 및 가이드 생성
- **Content Writer**: 글 작성
- **Content Reviewer**: 품질 검토
- **Style Learner**: 피드백 학습

## 사용법

```
/linkedin-writer [주제]
```

예시:
- `/linkedin-writer AI 시대의 개발자 역량`
- `/linkedin-writer 원격근무 3년차 회고`

## 워크플로우

```
┌─────────────────────────────────────────────────────────────┐
│                    LinkedIn Writer Flow                      │
└─────────────────────────────────────────────────────────────┘

1. 초기화
   └─→ style-guide.md 확인 (없으면 샘플 분석 먼저 안내)
   └─→ feedback-log.md 로드

2. 글 작성 (Content Writer)
   └─→ style-guide.md 참조
   └─→ 초안 생성

3. 검토 (Content Reviewer)
   └─→ 점수 산정 (100점 만점)
   │
   ├─→ [80점 미만] Writer에게 수정 요청 → 2번으로
   │
   └─→ [80점 이상] 사용자에게 제출

4. 사용자 피드백
   │
   ├─→ 수정 요청 → Style Learner가 기록 → 2번으로
   │
   └─→ 승인 → Style Learner가 저장 → 완료

5. 학습 (Style Learner)
   └─→ feedback-log.md 업데이트
   └─→ approved-posts/에 저장
   └─→ Style Analyzer 트리거 (선택적)
```

## 실행 단계

### Step 1: 스타일 가이드 확인

먼저 `data/style-guide.md`를 확인합니다.

- **가이드가 있는 경우**: Step 2로 진행
- **가이드가 없거나 비어있는 경우**:
  ```
  ⚠️ 스타일 가이드가 비어있습니다.

  먼저 data/samples/ 폴더에 기존 글 3-5개를 추가해주세요.
  그 후 "/analyze-style" 명령으로 스타일을 분석합니다.
  ```

### Step 2: 글 작성

Content Writer 에이전트를 호출합니다.

참조: `.claude/agents/content-writer.md`

입력:
- 주제: 사용자가 제공한 주제
- style-guide.md의 지침
- feedback-log.md의 피드백 이력

### Step 3: 검토

Content Reviewer 에이전트를 호출합니다.

참조: `.claude/agents/content-reviewer.md`

**80점 미만**:
- 수정 피드백을 Writer에게 전달
- Step 2로 돌아가 수정

**80점 이상**:
- 사용자에게 글 제시
- 피드백 대기

### Step 4: 사용자 피드백 처리

사용자 응답에 따라:

**"좋아" / "승인" / "완성"**:
- Style Learner 호출
- approved-posts/에 저장
- 종료

**수정 요청**:
- Style Learner 호출 (피드백 기록)
- Writer에게 수정 지시
- Step 2로 복귀

**"이건 별로야" / 부정적 피드백**:
- Style Learner 호출 (피드백 기록)
- 해당 표현/스타일을 피해야 할 목록에 추가
- Writer에게 재작성 지시

## 데이터 파일 위치

```
.claude/skills/linkedin-writer/data/
├── style-guide.md      # 스타일 가이드
├── feedback-log.md     # 피드백 로그
├── samples/            # 학습용 샘플 글
├── approved-posts/     # 승인된 글 저장
└── style-history/      # 스타일 가이드 버전 이력
```

## 관련 명령어

- `/linkedin-writer [주제]` - 글 작성 (이 스킬)
- `/analyze-style` - 스타일 분석 (Style Analyzer 단독 호출)
- `/learn [피드백]` - 피드백 학습 (Style Learner 단독 호출)

## 팁

1. **샘플 글은 많을수록 좋습니다**: 최소 5개, 권장 10개 이상
2. **피드백은 구체적으로**: "별로야" 보다 "너무 딱딱해" 가 학습에 효과적
3. **승인된 글이 쌓일수록**: 스타일 가이드가 정교해집니다
