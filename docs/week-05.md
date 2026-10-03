# 5주차 활동지 / Week 5 Worksheet

**1-page 기획서 / One-page plan**

- 작성일 / Date: 2026-09-30
- 참여자 / Present: 김준석, 김명준, 정찬영

---

## ① 주제 확정 / Confirm topic

- 확정 주제 / Topic: 한 화면에서 수강 편람, 강의평, 이수 체계도와 같은 정보를 한번에 볼 수 있는 프로그램
- 이유 / Reason: 실제 수강 신청을 하면서 불편함을 느꼈던 부분이기 때문에 선정했다.

---

## ② 태스크 분해와 의존 관계 / Tasks and dependencies

### 태스크 목록 / Task list

4주차 사용자 스토리와 완료 조건을 태스크로 나눕니다.
*Break down your Week 4 user stories and acceptance criteria into tasks.*

각 태스크는 따로 끝내도 맞는지 확인할 수 있어야 합니다. 담당에 '다 같이'는 쓰지 않습니다.
*Each task must be checkable on its own. Do not write "everyone" as owner.*

| # | 태스크 Task | 완료 조건 Done when | 선행 태스크 Depends on | 담당 Owner |
|---|---|---|---|---|
| 1 | 대상 학과 확정 및 실제 인터뷰 진행 | 학과 1개가 확정되고, 팀원 외 3명 이상(문제 당사자 포함)의 인터뷰 기록이 정리됨 |  | 전원 |
| 2 | DB 스키마 설계 |  | 1 |  |
| 3 | DB 생성 기능 구현 | 실행하면 DB 파일이 만들어져야함 | 2 |  |
| 4 | 과목 정보 수동 수집 | 대상 학과의 과목 전체가 CSV-UTF8로 저장해야 함 | 2 |  |
| 5 | CSV 데이터를 DB에 반영 | 여러 번 실행해도 중복 없이 반영되어야 함 | 3, 4 |  |
| 6 | 수동 입력 데이터 작성 (강의평 더미, 이수체계도 트랙·선수과목) | 강의평과 선수과목 연계 데이터가 DB에 들어가 조회됨 | 3 |  |
| 7 | 검색·필터 기능과 화면 연동 | 과목명·요일·학점 등으로 검색하면 결과가 화면의 비교 표에 표시됨 | 5 |  |
| 8 | 시간표 겹침 판별 기능 + 테스트 | 시간이 겹치는 과목을 담으려 하면 담기가 막히고 경고 문구가 떠야 함 | 5 |  |
| 9 | 과목 상세·강의평 연동 + 내 시간표 화면 연동 | 과목 클릭 시 상세 정보와 강의평이 뜨고, 담은 과목이 시간표에 색으로 표시됨 | 6, 7, 8 | 전원 |
| 10 | 사용성 테스트 + 시연 준비 | 팀 내에서 테스트후 피드백 반영까지 | 9 | 전원 |
### 의존 관계 그래프 / Dependency graph (DAG)

화살표는 "앞 태스크가 끝나야 뒤 태스크를 할 수 있다"는 뜻입니다.
*An arrow means the first task must finish before the second can start.*

**그리는 방법 / How to draw**
- 아래 예시에서 상자 이름을 바꾸고, 선후 관계 하나마다 화살표(`-->`) 줄을 하나씩 추가합니다. GitHub에서 파일을 열면 그림으로 보입니다. 미리 보려면 mermaid.live에 붙여 넣으세요.
  *Rename the boxes and add one `-->` line per dependency. GitHub shows it as a diagram. Preview at mermaid.live.*
- 태스크 표를 AI에게 주고 "Mermaid 그래프로 바꿔 줘"라고 요청해도 됩니다.
  *You can also give the task table to AI and ask "Convert this into a Mermaid graph."*
- 어려우면 종이에 그려 사진을 `docs/images/`에 올리고 `![DAG](images/week-05-dag.jpg)`로 넣어도 됩니다.
  *Or draw it on paper, upload the photo to `docs/images/` and link it with `![DAG](images/week-05-dag.jpg)`.*


graph LR
  # 프로젝트 태스크 의존성

```mermaid
flowchart TD
    T1["1. 대상 학과 확정 및 실제 인터뷰 진행<br/>담당: 전원"]
    T2["2. DB 스키마 설계"]
    T3["3. DB 생성 기능 구현"]
    T4["4. 과목 정보 수동 수집"]
    T5["5. CSV 데이터를 DB에 반영"]
    T6["6. 수동 입력 데이터 작성<br/>강의평 더미, 이수체계도 트랙·선수과목"]
    T7["7. 검색·필터 기능과 화면 연동"]
    T8["8. 시간표 겹침 판별 기능 + 테스트"]
    T9["9. 과목 상세·강의평 연동 + 내 시간표 화면 연동<br/>담당: 전원"]
    T10["10. 사용성 테스트 + 시연 준비<br/>담당: 전원"]

    T1 --> T2
    T2 --> T3
    T2 --> T4
    T3 --> T5
    T4 --> T5
    T3 --> T6
    T5 --> T7
    T5 --> T8
    T6 --> T9
    T7 --> T9
    T8 --> T9
    T9 --> T10

    classDef team fill:#e3f2e8,stroke:#2f8f6b,color:#1a1a1a
    classDef task fill:#dbe6f2,stroke:#2f5496,color:#1a1a1a

    class T1,T9,T10 team
    class T2,T3,T4,T5,T6,T7,T8 task
```

- 지금 착수 가능 (진입 차수 0) / Can start now (in-degree 0): 
- 작업 순서 (위상정렬) / Work order (topological sort): 
- 사이클이 있었다면 어떻게 풀었는가 / If there was a cycle, how did you fix it?: 

---

## ③ 범위 결정 / Scope

### Must — 없으면 성립 안 됨 / essential

핵심 시나리오 1개가 끝까지 동작하는 데 필요한 것만 / *Only what the core scenario needs to work end-to-end*

- 핵심 시나리오 / Core scenario: 수강정보 (검색 화면), 내 시간표(홈 화면), 커리큘럼(이수 체계도 화면) 구현
- 개인시간표 구성 (시간표에 강의 등록 가능한지, 중복 강의, 시간 겹치는 강의 등에서 오류 메시지를 반환하는지)
- 강의명 전체 또는 일부를 입력하면 선택 가능한 강의 반환



### Should (없을 경우에는 작성하지 마세요)
 
 - 시나리오



### Could (없을 경우에는 작성하지 마세요)

 - #11,#12

### **Won't — 이번 학기에 안 함 / not this semester**

| Won't 항목 Item | 포기한 이유 Why |
|---|---|
| 로그인 기능 | 자체 데이터 제작하여 사용, 기간이 길었다면, 로그인 구현 했을 것 |
| 학과 1개로 제한 | 직접 데이터를 입력해야 하기 때문에 1개 학과로 범위 제한  |

### 실행 가능성 확인 / Feasibility check

- 특수 장비·유료 API·실제 개인정보가 필요한가? 필요하다면 대안은?
  필요하지 않음. 유료 api사용하지 않음. 로그인 기능 없음(세션 기반), 강의평에 실제 실명,학번을 넣지 않음.
- 15주차에 발표장에서 시연할 수 있는 형태인가?
  발표용 노트북으로 flask run, 데이터를 DB를 넣어 둔 상태이므로 인터넷 없어도 돌아감. 

---

## ④ 가장 먼저 동작시킬 흐름 (Walking Skeleton) / First end-to-end flow

예 / Example: 과제 ID를 입력하면 → LMS에서 제출 기록을 받아 와서 → 화면에 제출 인원 숫자 하나가 뜬다

> [무엇을 입력하면] → [무엇을 처리해서] → [화면에 무엇이 나온다]
> 강의를 검색하면 -> DB에서 기록을 가져와서 -> 화면에 강의가 뜬다

---

> 수업 종료 시 커밋하세요 / Commit this at the end of class
> `git add docs/week-05.md && git commit -m "docs: 5주차 활동지 작성"`
