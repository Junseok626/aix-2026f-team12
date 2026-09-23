# 4주차 활동지 / Week 4 Worksheet

**주제 선택과 요구 명세 / Choosing a problem & writing the spec**

- 작성일 / Date: 2026-09-23
- 참여자 / Present: 김준석, 김명준, 정찬영

---

## ① 주제 선택 / Choosing one problem

| 항목 Item | 내용 |
|---|---|
| 선택한 주제 Chosen | 후보A |
| 선택 근거 Why | 실제 수강신청 하면서 우리가 공통적으로 느꼈던 불편함 해소를 위해  |

## ② 성공 기준 가져오기 / Success criteria from Week 3

| 3주차 성공 기준 원문 Original (Week 3) | 모호한 표현 Vague words |
|---|---|
| 학생들이 한 화면에서 수강시간,커리큘럼,강의평까지 확인할 수 있다. | 학생들이 수강신청의 정보를 잘 확인할 수 있다. |


## ③ Acceptance Criteria

최소 정상 경로 2개 + 실패 경로 1개. **판정 방법** 칸이 비면 아직 명세가 아닙니다.
At least two normal paths + one failure path. If "How to check" is empty, it is not yet a spec.

| # | 경로 Path | EARS 문장 Sentence | 판정 방법 How to check |
|---|---|---|---|
| *예시* | *정상* | *WHEN 학생이 과제 목록을 열면 THE 시스템은 SHALL 과목별 미제출 과제를 마감일 순으로 표시한다* | *미제출 과제 3건을 만든 뒤 목록을 열어 마감일 순으로 나오는지 확인* |
| AC-1 | 정상 Normal | WHEN 학생이 수강 할 과목을 선택하면 THE 시스템은 SHALL 커리큘럼과 강의평, 수강시간을 표시한다.  | 선택 시 과목을 3개 만든 뒤 강의평,커리큘럼,수강시간이 나오는지 확인 |
| AC-2 | 정상 Normal | WHEN 수강 할 과목을 개인 시간표에 넣게 되면 THE 시스템은 SHALL 요일과 시간을 표시하고 중복을 점검한다   | 시간이 겹치는 과목을 2개 만든 뒤 중복화면을 표시하는지 확인 |
| AC-3 | 정상 Normal | WHEN 학생이 커리큘럼 데이터가 등록된 학과를 선택하고 커리큘럼 탭을 열면 THE 시스템은 SHALL 해당 학과의 커리큘럼을 학년별로 구분하여 표시한다. | 학과 A·B의 학년별 기준 데이터를 준비한다. 각 학과를 선택해 탭을 열고, 학과 일치·학년 구분·과목 목록의 일치를 확인한다. |
| AC-4 | 실패 Failure | IF DB에 없는 강좌라면  THEN THE 시스템은 SHALL "개설되지 않은 강좌입니다" 를 반환한다.  | DB에 없는 과목을 검색창에 검색해보고 오류화면을 표시하는지 확인 |

> 확인할 동작이 더 있으면 AC-4부터 행을 추가해 쓰십시오.
> If there are more behaviors to check, add rows from AC-4.

- [ ] 이번 활동에서 AI를 사용했다면 `PROMPTS.md`에 기록했습니다 / Logged any AI use in `PROMPTS.md`

---

> 수업 종료 시 커밋하세요 / Commit this at the end of class
> `git add docs/week-04.md && git commit -m "docs: 4주차 활동지 작성"`
