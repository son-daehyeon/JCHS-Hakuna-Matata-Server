# JCHS-Hakuna-Matata-Server

> 교내 굿즈 쇼핑몰 하쿠나마타타 서버, 토스페이먼츠 결제 (Express · MongoDB)

제천고등학교 학생이 교내 굿즈를 주문하고 결제하는 쇼핑몰 "하쿠나 마타타"의 API 서버다. 제천고 코딩 동아리 "코딩연구소" 시절인 2022년에 만들었다. 결제는 토스페이먼츠로 처리하고, 빌드된 웹 클라이언트를 같은 서버에서 정적 파일로 함께 내보낸다.

## 주요 기능
- 회원가입·로그인: 이름, 전화번호, 학교, 학년·반·번호로 가입하고 JWT로 인증한다
- 상품 관리: 상품 목록·상세 조회, 관리자 전용 상품 등록과 사진 업로드(multer, 최대 50MB)
- 결제: 장바구니로 결제 건을 만든 뒤 토스페이먼츠 카드 결제 승인 또는 가상계좌(유효 24시간) 발급
- 웹훅: 토스페이먼츠 웹훅으로 주문을 생성하고 결제 상태를 갱신한다
- 주문 조회: 내 주문 목록·상세, 관리자 전용 전체 주문·회원 목록

| 메서드 | 경로 | 설명 |
|---|---|---|
| POST / PUT | `/api/account` | 로그인 / 회원가입 |
| GET | `/api/account`, `/api/account/list` | 내 정보 / 회원 목록(관리자) |
| GET / POST / PUT | `/api/product/list`, `/api/product`, `/api/product/photo` | 상품 목록 / 상세 / 등록·사진(관리자) |
| PUT / POST | `/api/billing`, `/api/billing/virtualAccount`, `/api/billing/card` | 결제 건 생성, 가상계좌 발급, 카드 결제 승인 |
| GET / POST | `/api/order`, `/api/order/list` | 내 주문·상세 / 전체 주문(관리자) |
| POST | `/api/webhook` | 토스페이먼츠 웹훅 |

## 기술 스택
| 영역 | 기술 |
|---|---|
| 언어 | JavaScript (Node.js) |
| 프레임워크 | Express 4, jsonwebtoken, multer |
| 데이터 | MongoDB (Mongoose 6) |
| 인프라 | 토스페이먼츠 API |

## 구조
```
src/
  index.js        HTTP 서버 시작(80번 포트)
  modules/        Express 앱, MongoDB 연결, 토스페이먼츠 axios 클라이언트
  routes/         account · product · order · webhook 라우터
  controllers/    요청 처리
  services/       비즈니스 로직
  schema/         account · product · billing · order 모델
  middlewares/    로그인·관리자 확인
routes/billing.js 결제 라우터
```

## 실행
```bash
yarn install
cp .env.template .env
yarn start
```
환경 변수: `TOSS_SECRET_KEY`, `JWT_SECRET_KEY`, `MONGODB_HOST`, `MONGODB_PORT`, `MONGODB_DATABASE`, `MONGODB_USERNAME`, `MONGODB_PASSWORD`

## 관련 저장소
- [JCHS-Hakuna-Matata-Client](https://github.com/son-daehyeon/JCHS-Hakuna-Matata-Client) — 쇼핑몰 웹 (React)

---
2022–2023 · 개인 프로젝트 · 보관됨
