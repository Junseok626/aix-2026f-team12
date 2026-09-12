# AI 협업 기록 / AI Collaboration Log

작성 원칙: 프롬프트 나열이 아니라 **판단 근거**를 남긴다.
Principle: record your **reasoning**, not just prompts.

---

## [이슈 #__] 제목 / Title

**목표(스펙) / Spec**
- 입력 Input:
- 처리 Processing:
- 출력 Output:
- 실패 조건 Failure:

**요청한 프롬프트 요지 / Prompt (summary)**

**결과에 대한 판단 / Decisions**
- 채택한 부분과 이유 / Accepted, because:
- 수정한 부분과 이유 / Changed, because:
- 폐기한 부분과 이유 / Rejected, because:

**검증 방법 / How it was verified**

---
(이슈 단위로 반복 / repeat per issue)

## 2026-09-09 · 메모 검색 기능 (2주차 활동)

**지시**
A조: 
메모 검색 기능 만들어줘. 제목이랑 본문에서 키워드로 찾을 수 있게.
B조: 
[지시]
메모 검색 기능을 추가해줘. 제목과 본문에서 키워드로 검색된다.

[규약]
(CONVENTIONS.md 내용 전체를 붙여넣기)

[근거]
(schema.sql, service.js, routes.js 내용 전체를 붙여넣기)

[종료조건]
- GET /memos/search?q=키워드 로 호출된다
- 제목 또는 본문에 키워드가 포함된 메모만 반환한다
- 본인 메모만 반환한다
- q가 비어 있으면 400과 { ok: false, error } 를 반환한다

**채택 여부**
전체 채택

**참고**
(있으면)
