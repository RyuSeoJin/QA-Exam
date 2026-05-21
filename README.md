# Certification Exam

자격증 시험 랜덤 문제 풀이 사이트. 현재 **ISTQB CTFL**(Foundation Level v4.0) 탭이 들어 있고,
다른 자격증은 탭으로 추가할 수 있는 구조입니다.

## 파일 구성

```
index.html       사이트 전체 (UI / 로직 / 자격증·모드 설정)
istqb-ctfl-kor-question.json   ISTQB CTFL 문제 데이터 (186문제)
```

`index.html`이 같은 폴더의 `istqb-ctfl-kor-question.json`을 `fetch`로 읽어옵니다.

## 띄우는 방법

### GitHub Pages (권장)
1. 두 파일을 GitHub 저장소에 올립니다.
2. **Settings → Pages → Branch: main / root** 로 설정합니다.
3. `https://<아이디>.github.io/<저장소>/` 로 접속합니다.

### 로컬 확인
`index.html`을 더블클릭으로 열면 브라우저 보안정책(CORS)으로 JSON을 못 읽습니다.
아래처럼 서버로 띄우세요.

```bash
cd 이폴더
python -m http.server 8000
# http://localhost:8000 접속
```

## 폰트

Pretendard(가변 다이나믹 서브셋)를 jsDelivr CDN으로 불러옵니다.
(orioncactus/pretendard v1.3.9) — 인터넷 연결 시 자동 적용되고, 오프라인이면
시스템 한글 폰트로 대체됩니다.

## 풀이 모드

상단 자격증 탭을 고른 뒤, 아래 3가지 모드 중 하나로 풉니다.

1. **세트별 풀기** — 샘플문제 A·B·C·D 버전을 골라서 풀기 (여러 세트 동시 선택 가능)
2. **섞어서 풀기** — 모든 세트를 한데 섞어 랜덤 출제
3. **AI 문제 풀기** — AI가 생성한 문제 풀기 *(문제 추가 전까지 "준비 중")*

세트별·섞어서 모드 모두 **챕터 필터**(1~6장)와 **문제 수/순서**를 함께 지정할 수 있습니다.
틀린 문제는 자동으로 **오답 노트**(자격증별로 분리 저장)에 쌓이고, 다시 풀어 맞히면 빠집니다.

### 채점 방식

풀이 시작 전에 두 가지 중 하나를 고를 수 있습니다.

- **즉시 채점** — 한 문제 풀 때마다 정답과 해설을 바로 보여줍니다.
- **모아서 채점** — 끝까지 다 푼 뒤(풀이 중에는 정답이 보이지 않음) 결과 화면에서 전체 문제의
  내 답·정답·해설을 한 번에 나열합니다. 실제 시험처럼 풀어보기 좋습니다.

두 방식 모두 결과 화면 하단에 **전체 해설**이 아코디언으로 제공되며, 오답은 기본으로 펼쳐집니다.

## 새 자격증 추가하기

`index.html` 상단의 `CERTS` 배열에 항목을 추가하면 탭이 생깁니다.

```js
const CERTS = [
  {
    id:'istqb-ctfl',          // 내부 식별자 (고유)
    name:'ISTQB CTFL',         // 탭에 표시될 이름
    file:'istqb-ctfl-kor-question.json',     // 문제 JSON 경로
    ready:true,                // false면 "준비 중" 탭
    chapters:{ '1':'1. ...', ... },  // 챕터 필터용 (없으면 챕터 필터 숨김)
    title:'... <em>강조</em> ...',   // 홈 제목
    desc:'설명',
  },
  // 새 자격증:
  { id:'csts', name:'CSTS', file:'csts.json', ready:true, chapters:null,
    title:'CSTS <em>문제 풀이</em>', desc:'...' },
];
```

`ready:false` 로 두면 데이터가 준비되기 전까지 "준비 중" 탭으로만 표시됩니다.

## 새 문제(AI 포함) 추가하기

`istqb-ctfl-kor-question.json`은 아래 형식의 객체 배열입니다. 같은 형식으로 항목을 추가하면
코드 수정 없이 자동 반영됩니다.

```json
{
  "id": "AI-001",            // 고유 ID
  "set": "AI",               // 세트 라벨. "AI"로 넣으면 'AI 문제 풀기' 모드에 들어감
  "num": "1",                // 문제 번호
  "extra": false,            // 추가문제 여부 (표시용)
  "stem": "문제 지문...",
  "options": { "a":"...", "b":"...", "c":"...", "d":"..." },
  "answer": ["c"],           // 정답. 복수정답이면 ["a","e"]
  "lo": "FL-1.1.1",          // 학습목표 (태그 표시용, 임의 가능)
  "chapter": "1",            // 챕터 1~6 (챕터 필터에 사용)
  "klevel": "K2",            // K-레벨 (표시용)
  "explanation": "해설..."
}
```

- `set`을 `"A"`/`"B"`/`"C"`/`"D"`로 넣으면 **세트별 풀기**에 자동으로 세트 버튼이 생깁니다.
- `set`을 `"AI"`로 넣으면 **AI 문제 풀기** 모드가 자동으로 활성화됩니다.
- 핵심 필드는 `stem`, `options`, `answer`, `chapter` 입니다.

## 출처

문제·해설: (사)KSTQB 공개 샘플 문제 및 실러버스. 학습 용도로만 사용하세요.
