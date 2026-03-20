# 13th-education

IT 연합동아리 '코테이토' 13기 교육팀 발표 자료 폴더입니다.

교육팀은 CS지식, 기술 면접에 자주 출제되는 주제를 매주 공부하고 이를 바탕으로 '코테이토 13기 정규세션'에서 해당 주제들을 발표합니다. 세션간 정규 교육은 6개월간 9회 진행될 계획입니다.

# Goals

1. CS와 기술 면접 주제에 대해 공부를 진행하고 서로 피드백한다.
2. 상호 피드백과 검증을 통해 올바른 지식 전달을 위해 노력한다.
3. 아는 것에 그치지 않고 타인에게 설명할 수 있는 능력을 기른다.
   <br>

# Members

| <img src="https://github.com/hyunj24.png" width=120/> | <img src="https://github.com/lemoncurdyogurt.png" width=120/> | <img src="https://github.com/kangcheolung.png" width=120 /> | <img src="https://github.com/kdhye516.png" width=120 /> | <img src="https://github.com/Baeseona.png" width=120 /> | <img src="https://github.com/Limlim0208.png" width=120 /> |
| :-------: | :-------: | :-------: | :-------: | :-------: | :-------: |
| [박현정](https://github.com/hyunj24) | [신수진](https://github.com/lemoncurdyogurt) | [강철웅](https://github.com/kangcheolung) | [김다혜](https://github.com/kdhye516) | [배선아](https://github.com/Baeseona) | [임유미](https://github.com/Limlim0208) |
| [ 12기 BE ] 교육팀장 | [ 12기 FE ] 교육팀 부팀장 | [ 12기 BE ] 교육팀 부팀장 | [ 13기 FE ] 교육팀원 | [ 13기 BE ] 교육팀원 | [ 13기 FE ] 교육팀원 |

# What we Studied

|       |     주제      | 발표자 |       날짜       |
| :---: | :-----------: | :----- | :--------------: |
| 1회차 | Git | 박현정 | 2026.03.06 |
| 2회차 | WebSocket | 강철웅 | 2026.04.03 |
| 3회차 | 미정 | 신수진 | 2026.05.01 |
| 4회차 | 미정 | 미정 | 2026.05.15 |
| 5회차 | 미정 | 미정 | 2026.06.19 |
| 6회차 | 미정 | 박현정 | 2026.06.26 |
| 7회차 | 미정 | 미정 | 2026.07.03 |
| 8회차 | 미정 | 미정 | 2026.07.31 |
| 9회차 | 미정 | 미정 | 2026.08.07 |
| 10회차 | 골든 포테이토 |  | 2026.08.14 |

# Rules

- 발표자는 발표일 기준 전주 금요일까지 주제를 선정해 팀원들에게 안내한다.
- 발표자는 발표 주차 `화요일 18시`까지 발표자료 초안, 대본을 팀원들에게 필수로 공유한다.
- 교육팀원들은 발표 주차 `수요일 20시` 전까지 발표자료를 검토하고, 관련된 CS퀴즈 각각 2-3문제씩 제작한다.
- 수요일 정규 회의때 CS퀴즈 10문제를 선정한다.

# Directory Structure

```plainText
│
├─ 13th-Educatoin
│     │
│     ├─ Week01 (dir)
│     │     │
│     │     ├─  hyunj24 (dir) // 해당 주차 발표자
│     │     │    ├─ Git-공부_자료.md
│     │     │    ├─ Git-대본.md 
│     │     │    ├─ Git-ppt 
│     │     │    └─ `기타 소스 코드 및 자료들`
│     │     │
│     │     ├─  kangcheolung (dir) // 발표자 외의 팀원들 (Option)
│     │     │    ├─ 주제.md
│     │     │    └─ `기타 자료`//참고한 자료들
│     │     │
│     │     └─  lemoncurdyogurt (dir) // 발표자 외의 팀원들 (Option)
│     │          ├─ 주제.md
│     │          └─ `기타 자료`//참고한 자료들
│     │
│     │
│     ├─ .. 이하 동일
│
│
```

## Git Rule

- git clone <주소>를 통해 Repository를 clone 받는다.
- `git checkout -b 본인 핸들명-주차` 명령어를 통해 본인 브랜치를 생성한다.
- 해당 브랜치에서 작업 후 커밋한다. (절대! main 브랜치에서 작업하지 않는다.)
- 작업 후 main 브랜치로 PR을 날린다.
- 작업 후 로컬에서 `git checkout main`로 브랜치를 돌린 후 `git branch -D 본인 핸들명-주차`을 통해 로컬 브랜치를 삭제한다.
- 팀원들은 해당 PR에 대해 피드백, 리뷰를 남긴다.
- 작업 후 `교육팀 운영진`은 `Squash and Merge`를 통해 PR을 merge하고 원격 브랜치를 삭제한다.
- 다시 작업을 시작하고 싶으면 main 브랜치의 최신 내용을 pull받은 후 `git checkout -b 본인 핸들명-주차`으로 브랜치를 로컬에서 생성해 작업한다.<br>

## Convention

발표자료 업로드: `docs: [000] n주차 발표자료 업로드`<br>
발표외 공부자료 업로드: `docs: [000] n주차 공부자료 업로드`<br>

### Pull request convention

[OOO] n주차 \_\_자료 제출합니다. (발표 or 공부)
