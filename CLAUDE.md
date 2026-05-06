# productivity-app

개인 생산성 관리 앱. 오버씽킹하는 사람을 위해 만들어진 하루 계획 & 기록 도구.

## 아키텍처

- **단일 파일 앱**: 모든 HTML, CSS, JS가 `index.html` 하나에 들어있다. 빌드 도구 없음.
- **Firebase 동기화**: Firestore(데이터) + Firebase Auth(Google 로그인)로 기기 간 동기화.
- **로컬 우선**: 전역 `D` 객체(line ~575)가 앱 상태 전체를 담는다. Firebase 미연결 시에도 로컬에서 동작.
- **자동저장**: CSV/MD 파일 자동저장(IndexedDB로 파일 핸들 보관), Firebase 실시간 sync.

## 탭 구성

| 탭 | id | 주요 기능 |
|---|---|---|
| 오늘 | `tab-today` | 브레인 덤프 & 우선순위, 루틴, 타임테이블, 할일, 집중 타이머 |
| 프로젝트 | `tab-projects` | 프로젝트별 할일 관리 |
| 아이디어 | `tab-ideas` | 아이디어 메모 |
| 성장기록 | `tab-growth` | 주간/월간/전체뷰, 날짜 클릭 상세 모달 |
| 리포트 | `tab-report` | 통계 및 패턴 분석 |
| AI코치 | `tab-coach` | AI 기반 생산성 코칭 |

## 핵심 패턴

- 상태 변경 후 `saveData()` 호출로 로컬 + Firebase 모두 저장
- `renderXxx()` 함수로 UI 갱신 (직접 DOM 조작)
- Firebase 함수: `initFirebase()`, `startFbSync()`, `fbSave()`
- 날짜는 `today()` / `yesterday()` 헬퍼 사용 (로컬 시간 기준)

## 수정 시 주의사항

- 단일 파일이므로 CSS/HTML/JS 모두 `index.html`에서 편집
- Firebase config는 line ~1447에 하드코딩되어 있음
- `D` 객체 구조 변경 시 Firebase 저장 schema도 함께 고려
