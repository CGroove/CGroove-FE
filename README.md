# 🕺 C.Groove - Dance Community Platform (FE)

> 대학 댄스동아리 커뮤니티 · 행사 신청 플랫폼 **C.Groove**의 웹 클라이언트
> 프레임워크 없이 **Vanilla JS(ES Modules)** 와 CSS로 만든 멀티 페이지 앱

- 서비스: https://cgroove.vercel.app
- 백엔드 · 프로젝트 전체 설명: [CGroove-BE](https://github.com/CGroove/CGroove-BE)
- 카카오테크 부트캠프 3기 개인 프로젝트 (2025.09 ~ 2025.12)

---

## 🖥️ 화면

<img width="1512" height="862" alt="메인" src="https://github.com/user-attachments/assets/8a5e7da1-a447-4de0-ba24-5ac6267bd3ac" />
<img width="1512" height="862" alt="화면 2" src="https://github.com/user-attachments/assets/5b61a9ca-e472-440f-9c43-7a8aee35078d" />
<img width="1512" height="862" alt="화면 3" src="https://github.com/user-attachments/assets/d1396f42-e145-4fed-aad5-28006c6a9c18" />
<img width="1512" height="862" alt="화면 4" src="https://github.com/user-attachments/assets/eb542d0f-4999-4acb-93ea-6e7e3ea3013e" />

## 🛠️ Tech Stack

| 구분 | 기술 |
|---|---|
| Core | HTML5, CSS3, JavaScript (ES Modules) |
| Styling | 직접 작성한 CSS (Flexbox · Grid, 미디어 쿼리) — UI 라이브러리 없음 |
| Build · Dev | Vite 6 (모든 HTML을 빌드 입력으로 자동 등록, 개발 서버 `/api` 프록시) |
| Deploy | Vercel (`/api/*`를 백엔드로 rewrite) |

## 🌟 주요 기능

| 영역 | 화면 |
|---|---|
| 인증 · 회원 | 로그인, 회원가입(프로필 이미지), 프로필 · 비밀번호 수정 |
| 메인 | 인기글(Hot Groove), 내 동아리 소식, 다가오는 행사, 장르 · 캘린더 |
| 동아리 | 목록 · 상세 · 생성 · 수정, 가입 신청, 멤버 · 신청 관리(승인 · 거절 · 역할 변경) |
| 게시글 | 목록(동아리 · 유형 필터, 최신 · 좋아요 · 조회수 정렬), 상세 · 작성 · 수정, 좋아요 · 댓글 |
| 행사 | 상세 · 등록 · 수정, 선착순 신청 · 취소, 참가자 관리 |

HTML 페이지 21개, 백엔드 API 53개를 모두 사용합니다.

## ⚡ 구현 포인트

### 1. API 요청 래퍼 (`scripts/common/api/core.js`)
모든 요청이 `apiRequest()` 한 곳을 거칩니다.
- Access Token을 `Authorization` 헤더에 자동으로 붙이고, 쿠키(Refresh Token)를 함께 보내도록 `credentials: 'include'` 설정
- **401이면 `/auth/refresh`로 재발급 후 원래 요청을 한 번 재시도**, 재발급도 실패하면 로그인 페이지로 이동
- 여러 요청이 동시에 401을 받아도 **재발급 요청은 한 번만** 보내도록 진행 중인 재발급 Promise를 공유
- `FormData` 요청은 `Content-Type`을 브라우저에 맡겨 multipart 경계를 자동 설정
- 상태 코드별 기본 메시지를 두고, 서버 에러는 `ApiError`로 통일해 화면에서 처리

### 2. 이미지 업로드 전 압축 (`scripts/common/util/image_util.js`)
업로드 전에 `canvas`로 긴 변 1024px · 품질 0.8로 줄여 전송 용량을 낮춥니다.

### 3. 도메인별 모듈 구조
```
scripts/
├── common/
│   ├── api/        # core(요청 래퍼 · 토큰) + auth · user · club · event · post · comment
│   ├── component/  # 헤더, 커스텀 셀렉트
│   └── util/       # 포맷, 이미지, 검증, 공통 함수
├── auth/ · users/ · clubs/ · posts/ · events/   # 페이지별 로직
└── main/ · pages/                               # 메인 · 목록 페이지
```

## 🚀 로컬 실행

**요구 사항**: Node.js 18+, 로컬에서 실행 중인 [백엔드](https://github.com/CGroove/CGroove-BE) (`localhost:8080`)

```bash
git clone https://github.com/CGroove/CGroove-FE.git
cd CGroove-FE
npm install
npm run dev     # http://localhost:5173 — /api 요청은 localhost:8080으로 프록시
```

프론트 코드는 항상 `/api`로 요청합니다. 로컬에서는 Vite가, 배포 환경에서는 Vercel이 백엔드로 전달합니다.

## ☁️ 배포

`main` 브랜치에 push하면 Vercel이 자동으로 빌드 · 배포합니다. `vercel.json`의 rewrite가 `/api/*`를 백엔드 서버로 프록시해, 브라우저는 HTTPS인 Vercel 도메인과만 통신합니다.

## 🗺 로드맵

- [ ] 백엔드 목록 페이지네이션 연동 (현재는 전체를 받아 클라이언트에서 10개씩 표시)
- [ ] 서버 에러 메시지(`detail`) 표시 개선
- [ ] TypeScript 도입
- [ ] 컴포넌트 단위 구조 정리 (React 이전 검토)

## 👨‍💻 개발자

**남진** · GitHub [@baggytsk](https://github.com/baggytsk) · jinoj0423@gmail.com
