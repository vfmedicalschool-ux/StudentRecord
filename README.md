# 27 VF 풀케어반 학습 리포트 — 배포 안내
분석 시점 2026년 8월 30일

## 1. 이 폴더를 GitHub 저장소에 올립니다
```
site/
  index.html          학생용 (구글 로그인 → 본인 리포트만)
  admin.html          선생님용 업로드 페이지
  data/cohort.json    반 집계 — 실명 없음, 공개돼도 무방
  firestore.rules     보안 규칙 (Firebase 콘솔에 붙여넣기)
```
GitHub Pages를 켜고 Settings → Pages → Source를 이 폴더로 지정하면 됩니다.

## 2. Firebase 콘솔에서 두 가지를 하세요
### (1) 승인된 도메인 추가
Authentication → Settings → 승인된 도메인에 GitHub Pages 도메인을 추가합니다.
예: `vfmedicalschool-ux.github.io`

### (2) 보안 규칙 적용
Firestore → 규칙 탭에 `firestore.rules` 내용을 붙여넣고 게시합니다.
**이 규칙이 실제 접근 통제입니다.** 규칙 없이는 로그인한 누구나 남의 리포트를 읽을 수 있습니다.

## 3. 개인 리포트 업로드
1. `admin.html`을 GitHub Pages 주소로 엽니다 (로컬 파일은 구글 로그인이 막힙니다)
2. 선생님 계정(vfmedicalschool@gmail.com)으로 로그인
3. `_업로드용_개인리포트.json`을 선택 → 업로드
4. 이메일이 등록되지 않은 학생은 건너뛰므로, 로그에 뜨는 명단을 확인해 이메일을 채우고 다시 올리세요

## 4. 보안 설계 — 무엇이 지켜지고 무엇이 안 지켜지나
**지켜지는 것**
- 개인 리포트는 GitHub에 올라가지 않습니다. Firestore에만 있습니다.
- 보안 규칙이 서버에서 검사하므로, 다른 학생이 개발자도구로 시도해도 읽히지 않습니다.
- index.html에는 개인 데이터가 심어져 있지 않습니다. 로그인 후 본인 문서 1건만 가져옵니다.

**주의할 것**
- 규칙을 적용하지 않으면 아무 의미가 없습니다. 2-(2)를 반드시 하세요.
- 선생님 계정이 뚫리면 전부 열립니다. 2단계 인증을 켜두세요.
- `data/cohort.json`은 공개입니다. 반평균·단원 통계만 들어 있고 실명은 없습니다.
- 명단(config/roster)은 로그인한 사용자면 읽을 수 있습니다. 이름·이메일이 들어 있으니
  더 엄격히 하려면 규칙에서 config 읽기도 선생님으로 제한하고, 이 앱은 명단을 읽지 않으므로 그래도 됩니다.

## 5. 매달 갱신하는 법
1. 새 데이터로 파이프라인 실행 → `_업로드용_개인리포트.json`, `site/data/cohort.json` 갱신
2. `site/` 를 저장소에 push (cohort.json만 바뀝니다)
3. `admin.html`에서 새 JSON 업로드
