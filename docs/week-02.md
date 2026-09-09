# 2주차 활동지 — 코딩 에이전트와 컨텍스트

| | |
| :-- | :-- |
| 팀명 | AI융합 12팀 |
| 작성일 | 2026-09-09 |
| 참여자 | 김준석, 김명준, 정찬영 |

---

## 0. 준비

 `memo-seed` 저장소를 엽니다. 다음 파일이 있는지 확인하세요.

- [ ] `schema.sql`
- [ ] `service.js`
- [ ] `routes.js`
- [ ] `CONVENTIONS.md`

---

## 1. 조 나누기

팀을 두 조로 나눕니다. (4인 → 2:2 / 3인 → 1:2)

| 조 | 참여자 |
| :-- | :-- |
| A조 | 김준석, 정찬영 |
| B조 | 김명준 |

**두 조는 같은 과제를 동시에 수행합니다.** 서로의 화면을 보지 마세요.

### 오늘의 과제 (두 조 공통)

> 메모 검색 기능을 추가하라. 제목과 본문에서 키워드로 찾을 수 있어야 한다.

---

## 2. 에이전트에게 준 것

### A조 — 이것만 붙여넣습니다

```
메모 검색 기능 만들어줘. 제목이랑 본문에서 키워드로 찾을 수 있게.
```

파일은 **하나도 주지 않습니다.**

### B조 — 네 칸을 모두 채웁니다

```
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
```

### 실제로 붙여넣은 것 (원문 그대로, 요약 금지)

```
A조: 

class Memo:
    def __init__(self, memo_id: int, title: str, content: str):
        self.id = memo_id
        self.title = title
        self.content = content

class MemoManager:
    def __init__(self):
        self.memos: list[Memo] = []

    def add_memo(self, memo_id: int, title: str, content: str):
        """메모 추가"""
        self.memos.append(Memo(memo_id, title, content))

    def search_memos(self, keyword: str, case_sensitive: bool = False) -> list[Memo]:
        """
        제목이나 본문에 키워드가 포함된 메모를 검색합니다.
        
        :param keyword: 검색할 단어
        :param case_sensitive: 대소문자 구분 여부 (기본값: False)
        :return: 검색 조건에 맞는 Memo 객체 리스트
        """
        if not keyword or not keyword.strip():
            return []

        search_keyword = keyword if case_sensitive else keyword.lower()
        results = []

        for memo in self.memos:
            title = memo.title if case_sensitive else memo.title.lower()
            content = memo.content if case_sensitive else memo.content.lower()

            if search_keyword in title or search_keyword in content:
                results.append(memo)

        return results

# --- 사용 예시 ---
if __name__ == "__main__":
    manager = MemoManager()
    
    # 샘플 메모 등록
    manager.add_memo(1, "장보기 목록", "사과, 바나나, 우유 사기")
    manager.add_memo(2, "파이썬 공부", "리스트와 딕셔너리 복습하기")
    manager.add_memo(3, "여행 계획", "제주도 비행기 표 예매 및 숙소 예약")

    # 검색 실행
    keyword = "비행기"
    search_results = manager.search_memos(keyword)

    print(f"'{keyword}' 검색 결과 ({len(search_results)}건):")
    for memo in search_results:
        print(f"[{memo.id}] 제목: {memo.title} | 본문: {memo.content}")

B조: 

const db = require('./db');

/**
 * 사용자의 메모 목록을 최신순으로 조회한다.(service)
 */
function listMemos(userId) {
  return db.all(
    `SELECT id, title, created_at
       FROM memos
      WHERE user_id = ?
      ORDER BY created_at DESC`,
    [userId]
  );
}

/**
 * 메모 한 건을 조회한다. 본인 메모가 아니면 null을 반환한다.
 */
function getMemo(userId, memoId) {
  return db.get(
    `SELECT id, title, body, created_at
       FROM memos
      WHERE id = ? AND user_id = ?`,
    [memoId, userId]
  );
}

/**
 * 메모를 생성한다.
 */
function createMemo(userId, title, body) {
  return db.run(
    `INSERT INTO memos (user_id, title, body, created_at)
     VALUES (?, ?, ?, datetime('now'))`,
    [userId, title, body]
  );
}

/**
 * 제목 또는 본문에 키워드가 포함된 메모를 최신순으로 조회한다.
 * LIKE 패턴의 %, _ 를 이스케이프해 리터럴 검색어로 취급한다.
 */
function searchMemos(userId, keyword) {
  const escaped = keyword.replace(/[\\%_]/g, '\\$&');
  const pattern = `%${escaped}%`;

  return db.all(
    `SELECT id, title, created_at
       FROM memos
      WHERE user_id = ?
        AND (title LIKE ? ESCAPE '\\' OR body LIKE ? ESCAPE '\\')
      ORDER BY created_at DESC`,
    [userId, pattern, pattern]
  );
}

module.exports = { listMemos, getMemo, createMemo, searchMemos };

/**
* router
*/
const express = require('express');
const service = require('./service');

const router = express.Router();

// 메모 검색 (반드시 /memos/:id 보다 먼저 선언)
router.get('/memos/search', async (req, res) => {
  const q = req.query.q;

  if (!q || !q.trim()) {
    return res.status(400).json({ ok: false, error: 'QUERY_REQUIRED' });
  }

  const memos = await service.searchMemos(req.user.id, q.trim());
  res.json({ ok: true, data: memos });
});

// 메모 목록 조회
router.get('/memos', async (req, res) => {
  const memos = await service.listMemos(req.user.id);
  res.json({ ok: true, data: memos });
});

// 메모 단건 조회
router.get('/memos/:id', async (req, res) => {
  const memo = await service.getMemo(req.user.id, req.params.id);

  if (!memo) {
    return res.status(404).json({ ok: false, error: 'MEMO_NOT_FOUND' });
  }

  res.json({ ok: true, data: memo });
});

// 메모 생성
router.post('/memos', async (req, res) => {
  const { title, body } = req.body;

  if (!title || !body) {
    return res.status(400).json({ ok: false, error: 'TITLE_AND_BODY_REQUIRED' });
  }

  const result = await service.createMemo(req.user.id, title, body);
  res.status(201).json({ ok: true, data: { id: result.lastID } });
});

module.exports = router;
```

> 요약하지 마세요. 나중에 이 기록이 무엇이 결과를 만들었는지 확인하는 근거가 됩니다.

---

## 3. 결과 확인

| | 확인 항목 | 결과 |
| :-: | :-- | :-- |
| ① | 실행 성공까지 걸린 시간 | 30분 |
| ② | `CONVENTIONS.md` 위반 개수 | A조 3개, B조 1개 |
| | → A조: 계층 단일(python), 응답 형식, 권한 위반, B조: 허용 동사를 사용하지 않음 | |
| ③ | **본인 메모만 반환되는가** | A조 아니오, B조 예 |

### ③번을 반드시 확인하세요

생성된 SQL에 `user_id` 조건이 들어 있는지 보세요.

없다면 **코드는 정상 동작하지만 남의 메모까지 검색됩니다.** 에러도 나지 않습니다.

---

## 4. 두 조의 결과 비교

작업이 끝나면 두 조가 만든 코드를 나란히 놓고 함께 답하세요.

**4-1. 두 결과의 가장 큰 차이는 무엇입니까?**

A조는 파이썬 배열에 저장하고, B조는 DB를 이용해 저장함.
B조는 내부 SQL쿼리에서 user_id를 필수적으로 요구하고 있어서 내 메모만 검색할 수 있는 반면, 
A조는 리스트 전체를 순회하기 때문에 여러사람의 메모가 등록되었을 경우, 내 메모가 아닌 다른 사람의 메모 또한 볼 수 있다.

**4-2. A조의 실패는 모델 탓입니까, 우리가 주지 않은 탓입니까? 근거를 들어 적으세요.**

```

```

**4-3. B조가 준 자료 중 결과를 가장 크게 바꾼 것 하나를 꼽는다면 무엇입니까? 왜 그렇게 생각합니까?**

```

```

---

## 5. PROMPTS.md 기록

위 2번의 프롬프트 원문을 저장소의 `PROMPTS.md`에 추가하고 커밋하세요.

```markdown
## 2026-__-__ · 메모 검색 기능 (2주차 활동)

**지시**
(붙여넣은 프롬프트 원문)

**채택 여부**
(전체 채택 / 일부 채택 — 무엇을 어떻게 수정했는지 / 미채택)

**참고**
(있으면)
```

- [ ] `PROMPTS.md`에 추가하고 커밋했습니다

---

## 6. 제출 확인

- [ ] 이 활동지를 저장소에 커밋했습니다
- [ ] `PROMPTS.md`를 커밋했습니다
