# 🎯 로또 AI 플랫폼 - 개인용 데스크탑 버전 완전 기술명세서 v1.1.1

**프로젝트명**: Lotto AI Desktop (로또 AI 데스크탑)  
**버전**: 1.1.1 (Personal Edition - Complete)  
**타겟**: 단일 사용자, 데스크탑 전용  
**최종 업데이트**: 2024년 12월 26일  
**문서 타입**: Full Stack Complete Technical Specification (기초분석 개선 완료)

---

## 📋 목차

### Part 1: 기초 설계 (1-5장)
1. [프로젝트 개요](#1-프로젝트-개요)
2. [기술 스택](#2-기술-스택)
3. [시스템 아키텍처](#3-시스템-아키텍처)
4. [핵심 개선 사항](#4-핵심-개선-사항)
5. [폴더 구조](#5-폴더-구조)

### Part 2: 상세 설계 (6-10장)
6. [UI/UX 설계](#6-uiux-설계)
7. [핵심 컴포넌트](#7-핵심-컴포넌트)
8. [데이터베이스 설계](#8-데이터베이스-설계)
9. [API 명세](#9-api-명세)
10. [전체 화면 상세 명세](#10-전체-화면-상세-명세)

### Part 3: 구현 가이드 (11-15장)
11. [개발 일정](#11-개발-일정)
12. [개발 환경 설정](#12-개발-환경-설정)
13. [성능 & 안정성](#13-성능--안정성)
14. [트러블슈팅](#14-트러블슈팅)
15. [부록](#15-부록)

---

## ⚠️ v1.1.1 주요 개선 사항

### v1.1 검토 의견 반영 완료 ✅

1. **브라우저 메모리 과부하 해결**
   - TensorFlow.js LSTM → Web Worker 격리
   - 차트 → Lazy Loading 적용
   - 메모리 모니터링 시스템 추가

2. **SQLite Lock 에러 방지**
   - WAL (Write-Ahead Logging) 모드 활성화
   - Write Queue 시스템 도입
   - 크롤링 중 UI 자동 잠금

3. **크롤링 IP 차단 방지**
   - Rate Limiting: 요청당 2초 대기
   - Debounce: 1분 쿨다운
   - 403 에러 시 자동 중단

4. **이미지 생성 안정성 확보**
   - Week 1에 PoC 테스트 추가
   - Canvas API 직접 사용 (html2canvas 대안)
   - 조기 검증으로 리스크 제거

5. **레이아웃 유연성 향상**
   - min-width 1280px + max-width 1920px
   - 창 크기에 맞게 자동 확장
   - 해상도 경고 시스템

### v1.1.1 기초분석 개선 완료 ⭐ 신규

6. **회차 범위 슬라이더 추가**
   - 10회 ~ 전체 회차까지 선택 가능
   - 프리셋 버튼 (50/100/500/전체)
   - 실시간 차트/통계 업데이트

7. **주요 통계 카드 추가**
   - 최소값/최대값/평균/중앙값/최빈값/표준편차
   - 색상으로 구분된 시각화
   - 분석별 맞춤 통계 제공

8. **전체 회차 팝업 구현**
   - 다이얼로그 형태의 상세 데이터 뷰
   - 검색 기능 (회차, 날짜)
   - CSV 내보내기
   - 페이지네이션


### v1.1.1.1 UI/UX 개선 완료 ⭐ 최신 (2024-12-26)

9. **페이지별 당첨번호 표시 방식 차별화**
   - 셀 형태: 당첨번호 관리, 커스텀분석 (연한 배경 + 진한 텍스트)
   - 원형 공 + 컨텍스트 컬러: 기초분석 17종 (분석별 색상 규칙)
   - 사용자 경험 개선: 분석별 시각적 구분 명확화

10. **메뉴 구조 개선**
   - AI분석 메뉴 추가 (커스텀분석과 필터 사이)
   - 전체 메뉴: 대시보드│당첨번호│기초분석│커스텀분석│AI분석│필터│조합│AI조합│출력│검증

11. **대시보드 UI 실제 디자인 반영**
   - 환영 섹션 추가
   - 4개 통계 카드 레이아웃
   - 2열 구조: 조합 결과 + Top 10 테이블

12. **당첨번호 관리 페이지 개선**
   - 상단 탭 네비게이션 (Lotto Lab)
   - 표 형태 셀로 번호 표시
   - 분석데이터 컬럼 추가 (합계, 평수합, AC값, 홀, 짝, 연번)

13. **LottoBall 공통 컴포넌트 설계**
   - variant: 'cell' | 'ball'
   - contextType: 분석별 색상 규칙
   - 재사용 가능한 유연한 구조

---

## 1. 프로젝트 개요

### 1.1 프로젝트 목적

로또 당첨번호의 **통계적 분석과 패턴 인사이트**를 제공하여 데이터 기반의 조합을 생성할 수 있도록 지원하는 **개인용 데스크탑 웹 애플리케이션**입니다.

**v1.1.1 개선 포인트**:
- 🚀 **브라우저 안정성 우선**: 무거운 작업은 Web Worker로 격리
- 🔒 **데이터 무결성 보장**: SQLite Write Queue로 잠금 방지
- 🛡️ **크롤링 안전성**: Rate Limiting으로 IP 차단 방지
- 🎨 **이미지 생성 검증**: PoC 테스트로 조기 검증
- 📱 **유연한 레이아웃**: 고정 너비지만 확장 가능
- 📊 **강화된 기초분석**: 회차 범위 선택, 통계, 전체 회차 팝업 ⭐

### 1.2 핵심 가치

- **데이터 투명성**: 모든 분석은 실제 역대 당첨번호 기반
- **직관적 시각화**: 복잡한 통계를 누구나 이해할 수 있는 차트로 표현
- **맞춤형 분석**: 직접 필터를 설정하여 조합 생성
- **AI 딥러닝**: LSTM 기반 패턴 학습 및 조합 추천
- **간편한 백업**: SQLite 파일 하나로 완전한 백업
- **안정적 동작**: Web Worker와 Queue로 브라우저 크래시 방지
- **유연한 분석**: 회차 범위를 선택하여 다양한 시점 분석 ⭐

### 1.3 프로덕션 vs 개인용 비교

| 항목 | 프로덕션 버전 | 개인용 v1.1.1 |
|------|--------------|---------------|
| **사용자** | 다중 (회원가입) | 단일 (본인만) |
| **인증** | NextAuth.js | ❌ 제거 |
| **데이터베이스** | PostgreSQL | SQLite (WAL 모드) ⭐ |
| **호스팅** | Vercel 클라우드 | localhost:3000 |
| **반응형** | 모바일/태블릿/데스크탑 | 데스크탑 (1280-1920px) ⭐ |
| **보안** | CSRF, Rate Limit, HTTPS | 최소 (로컬만) |
| **캐싱** | Redis, CDN | 메모리 캐시만 |
| **배포** | CI/CD, 스테이징 | ❌ 불필요 |
| **AI 연산** | 서버 | Web Worker ⭐ |
| **차트 로딩** | SSR | Lazy Loading ⭐ |
| **DB 쓰기** | 동시 실행 | Write Queue ⭐ |
| **크롤링** | 제한 없음 | Rate Limiting ⭐ |
| **기초분석** | 고정 범위 | 회차 범위 선택 ⭐ 신규 |
| **개발 기간** | 16주 | **8주** |
| **복잡도** | 높음 | **낮음** |
| **비용** | $20/월 | **무료** |

### 1.4 전체 화면 구조 (39+ Screens)

```
로또 AI 데스크탑 v1.1.1
│
├── 🏠 1. 대시보드 (1 screen)
│   └── 홈 화면 ((환영 메시지, KPI 그리드, 조합 결과 요약, 당첨 등수 현황)
│
├── 🎱 2. 당첨번호 (1 screen)
│   └── 당첨번호 목록 + 수동 추가 + 크롤링 (Rate Limited ⭐)
│
├── 📊 3. 기초분석 (17 screens) - 회차 범위 선택 + 통계 + 팝업 ⭐⭐⭐
│   ├── 3.1 기본 통계 (3)
│   │   ├── 총합 분석 (회차 슬라이더 + 통계 카드 + 전체보기 팝업)
│   │   ├── 끝수합 분석
│   │   └── AC값 분석
│   ├── 3.2 비율 분석 (3)
│   │   ├── 홀짝 비율
│   │   ├── 저고 비율
│   │   └── 소수/합성수 비율
│   ├── 3.3 수학적 분석 (2)
│   │   ├── 제곱수
│   │   └── 배수 (3/5의 배수)
│   ├── 3.4 패턴 분석 (2)
│   │   ├── 번호대별 분포
│   │   └── 연번 (연속번호)
│   ├── 3.5 데이터 분석 (4)
│   │   ├── 이월수
│   │   ├── 끝수 분석
│   │   ├── 동형수
│   │   └── 핫콜드
│   └── 3.6 고급 분석 (3)
│       ├── 미출현 그룹
│       ├── 회귀분석
│       └── (추가 확장 가능)
│
├── ⚙️ 4. 커스텀분석 (5 screens)
│   ├── 커스텀분석 목록
│   ├── 새 분석 만들기
│   ├── 직접 입력형 (번호 직접 입력)
│   ├── 그룹형 (번호 그룹 설정)
│   └── 수식형 (조건식 작성)
│
├── 🤖 5. AI 분석 (10 screens) - Web Worker ⭐
│   ├── AI 고정수 설정
│   ├── AI 제외수 설정
│   └── AI 추천수 (8 screens)
│       ├── 5개 추천
│       ├── 10개 추천
│       ├── 15개 추천
│       ├── 20개 추천
│       ├── 25개 추천
│       ├── 30개 추천
│       ├── 35개 추천
│       └── 40개 추천
│
├── 🔍 6. 필터 (1 screen) - Web Worker ⭐
│   └── 통합 필터 화면 (17종 필터 + 실시간 카운팅)
│
├── 🎲 7. 조합 (1 screen) - Write Queue ⭐
│   └── 필터 기반 조합 생성
│
├── 🤖 8. AI 조합 (1 screen) - Web Worker ⭐
│   └── AI 기반 조합 생성
│
├── 📄 9. 디지털 출력 (1 screen) - Canvas API ⭐
│   └── PDF/PNG 다운로드 (PoC 검증 완료)
│
├── ✅ 10. 검증 (1 screen)
│   └── 조합 자동 검증
│
└── 🧪 11. PoC 테스트 (1 screen) ⭐
    └── 이미지 생성 테스트 (Week 1)

총 39 screens (PoC 포함)
```

### 1.5 주요 기능

#### 1.5.1 제거된 기능 (개인용)
- ❌ 회원가입/로그인
- ❌ 프로필 관리
- ❌ 소셜 인증
- ❌ 결제 시스템
- ❌ 구독 관리
- ❌ 멀티유저 권한
- ❌ 모바일/태블릿 지원
- ❌ 클라우드 동기화

#### 1.5.2 유지된 기능 (개인용)
- ✅ 당첨번호 관리 (크롤링 + 수동)
- ✅ 기초분석 17종 (회차 범위 선택 + Lazy Loading) ⭐
- ✅ 커스텀분석 3종
- ✅ AI 분석 (LSTM - Web Worker)
- ✅ 필터 (17종 조건 - Web Worker)
- ✅ 조합 생성 (Write Queue)
- ✅ AI 조합 생성 (Web Worker)
- ✅ PDF/PNG 출력 (Canvas API)
- ✅ 자동 검증
- ✅ 로컬 백업/복원

#### 1.5.3 신규 추가 기능 (v1.1.1) ⭐⭐⭐
- ⭐ 회차 범위 슬라이더 (10~전체)
- ⭐ 주요 통계 카드 (6가지 통계)
- ⭐ 전체 회차 팝업 (검색, CSV 내보내기)
- ⭐ 비율 통계 (홀짝, 저고 등)
- ⭐ 실시간 차트 업데이트

---

## 2. 기술 스택

### 2.1 Frontend

```typescript
// ===== Core Framework =====
- Next.js 14.2+ (App Router, Server Components)
- React 18.3+
- TypeScript 5.3+

// ===== UI & Styling =====
- Tailwind CSS 3.4+ (유연한 레이아웃: 1280-1920px)
- shadcn/ui (Radix UI primitives)
- lucide-react 0.344+ (아이콘)

// ===== State Management =====
- Zustand 4.5+ (클라이언트 상태)
  * dashboardStore
  * analysisStore
  * customStore
  * aiStore
  * filterStore
  * combinationStore
  * outputStore

// ===== Data Visualization (Lazy Loading ⭐) =====
- Recharts 2.12+ (차트)
- react-intersection-observer 3.0+ ⭐ (Lazy Loading)
- D3.js 7.x (고급 시각화, 선택사항)

// ===== File Export (검증 완료 ⭐) =====
- jsPDF 2.5+ (PDF 생성)
- Canvas API (PNG - html2canvas 대체)

// ===== Performance (신규 ⭐) =====
- Web Worker API (AI 연산, 필터)
- p-queue 8.0+ ⭐ (Write Queue)
- IndexedDB (브라우저 캐시)

// ===== Utilities =====
- date-fns 3.3+ (날짜 처리)
- clsx 2.1+ (클래스 조합)
- tailwind-merge 2.2+ (Tailwind 병합)
- lodash-es 4.17+ ⭐ (청크 처리)
- axios 1.6+ (HTTP)
```

### 2.2 Backend

```typescript
// ===== Server =====
- Next.js API Routes (서버리스 함수)

// ===== Database (안정성 강화 ⭐) =====
- SQLite 3 (파일 기반)
- better-sqlite3 9.4+ (WAL 모드 ⭐)
- Prisma 5.10+ (ORM)

// ===== AI/ML (Web Worker 격리 ⭐) =====
- TensorFlow.js 4.15+ (Worker 전용)
- @tensorflow/tfjs-node (사전 학습용, 선택)
```

### 2.3 개발 도구

```typescript
// ===== Development =====
- ESLint 8.56+ (린트)
- Prettier 3.2+ (코드 포맷팅)
- TypeScript ESLint (타입 체크)
- tsx 4.7+ (스크립트 실행)

// ===== Tools =====
- Prisma Studio (DB 관리)
- Chrome DevTools (디버깅)
- Performance Monitor (성능 측정)
```

### 2.4 제거된 패키지

```diff
프로덕션에서 제거:
- ❌ next-auth (인증)
- ❌ @supabase/supabase-js (클라우드 DB)
- ❌ swr (fetch로 대체)
- ❌ @vercel/analytics (배포 불필요)
- ❌ ioredis (Redis 캐싱)
- ❌ @sentry/nextjs (에러 추적)
```

### 2.5 신규 추가 패키지 (v1.1 ⭐)

```bash
# 성능 & 안정성
npm install p-queue                      # Write Queue
npm install react-intersection-observer  # Lazy Loading
npm install lodash-es                    # 청크 처리

# 타입 정의
npm install -D @types/lodash-es
```

---

**(나머지 내용은 원본 명세서와 동일하게 유지하되, 5장 폴더 구조, 7장 핵심 컴포넌트, 10장 화면 명세만 수정)**

---

## 5. 폴더 구조

### 5.1 전체 구조 (v1.1.1 개선)

```
lotto-ai-desktop/
│
├── src/
│   │
│   ├── app/                           # Next.js App Router
│   │   │
│   │   ├── layout.tsx                 # 루트 레이아웃 (GNB + 해상도 경고 ⭐)
│   │   ├── page.tsx                   # 대시보드 (홈)
│   │   ├── globals.css                # 글로벌 스타일
│   │   │
│   │   ├── winning-numbers/           # 당첨번호
│   │   │   └── page.tsx
│   │   │
│   │   ├── analysis/                  # 기초분석 (17종) ⭐⭐⭐
│   │   │   ├── layout.tsx             # LNB
│   │   │   ├── sum/page.tsx           # 회차 슬라이더 + 통계 + 팝업
│   │   │   ├── end-sum/page.tsx
│   │   │   ├── ac-value/page.tsx
│   │   │   ├── odd-even/page.tsx
│   │   │   ├── low-high/page.tsx
│   │   │   ├── prime-composite/page.tsx
│   │   │   ├── square/page.tsx
│   │   │   ├── multiple/page.tsx
│   │   │   ├── section/page.tsx
│   │   │   ├── consecutive/page.tsx
│   │   │   ├── carryover/page.tsx
│   │   │   ├── ending-digit/page.tsx
│   │   │   ├── homomorph/page.tsx
│   │   │   ├── hot-cold/page.tsx
│   │   │   ├── unappeared-group/page.tsx
│   │   │   └── regression/page.tsx
│   │   │
│   │   ├── custom-analysis/           # 커스텀분석
│   │   │   ├── page.tsx
│   │   │   ├── new/page.tsx
│   │   │   └── [id]/page.tsx
│   │   │
│   │   ├── ai-analysis/               # AI 분석
│   │   │   ├── layout.tsx
│   │   │   ├── fixed/page.tsx
│   │   │   ├── excluded/page.tsx
│   │   │   ├── recommend-5/page.tsx
│   │   │   ├── recommend-10/page.tsx
│   │   │   ├── recommend-15/page.tsx
│   │   │   ├── recommend-20/page.tsx
│   │   │   ├── recommend-25/page.tsx
│   │   │   ├── recommend-30/page.tsx
│   │   │   ├── recommend-35/page.tsx
│   │   │   └── recommend-40/page.tsx
│   │   │
│   │   ├── filter/                    # 필터
│   │   │   └── page.tsx
│   │   │
│   │   ├── combination/               # 조합
│   │   │   └── page.tsx
│   │   │
│   │   ├── ai-combination/            # AI 조합
│   │   │   └── page.tsx
│   │   │
│   │   ├── output/                    # 출력
│   │   │   └── page.tsx
│   │   │
│   │   ├── verification/              # 검증
│   │   │   └── page.tsx
│   │   │
│   │   ├── test-poc/                  # ⭐ PoC 테스트 (신규)
│   │   │   └── image-export/
│   │   │       └── page.tsx
│   │   │
│   │   └── api/                       # API Routes
│   │       ├── winning-numbers/
│   │       │   ├── route.ts           # GET, POST (Queue ⭐)
│   │       │   ├── [id]/route.ts      # GET, PUT, DELETE
│   │       │   └── crawl/route.ts     # POST (Rate Limiting ⭐)
│   │       │
│   │       ├── analysis/              # ⭐ range 파라미터 추가
│   │       │   ├── sum/route.ts       # + statistics 계산
│   │       │   ├── end-sum/route.ts
│   │       │   ├── ac-value/route.ts
│   │       │   ├── odd-even/route.ts
│   │       │   ├── low-high/route.ts
│   │       │   ├── prime-composite/route.ts
│   │       │   ├── square/route.ts
│   │       │   ├── multiple/route.ts
│   │       │   ├── section/route.ts
│   │       │   ├── consecutive/route.ts
│   │       │   ├── carryover/route.ts
│   │       │   ├── ending-digit/route.ts
│   │       │   ├── homomorph/route.ts
│   │       │   ├── hot-cold/route.ts
│   │       │   ├── unappeared-group/route.ts
│   │       │   └── regression/route.ts
│   │       │
│   │       ├── custom-analysis/
│   │       │   ├── route.ts
│   │       │   └── [id]/route.ts
│   │       │
│   │       ├── ai/
│   │       │   ├── fixed/route.ts
│   │       │   ├── excluded/route.ts
│   │       │   └── recommend/route.ts
│   │       │
│   │       ├── filter/
│   │       │   ├── route.ts
│   │       │   └── count/route.ts
│   │       │
│   │       ├── combination/
│   │       │   ├── route.ts
│   │       │   └── [id]/route.ts
│   │       │
│   │       └── verification/
│   │           └── route.ts
│   │
│   ├── components/                    # 컴포넌트
│   │   │
│   │   ├── layout/
│   │   │   ├── GlobalNav.tsx
│   │   │   ├── AnalysisNav.tsx
│   │   │   ├── CustomAnalysisNav.tsx
│   │   │   ├── AIAnalysisNav.tsx
│   │   │   └── ResolutionWarning.tsx  # ⭐ 신규
│   │   │
│   │   ├── ui/                        # shadcn/ui (20+ 컴포넌트)
│   │   │   ├── button.tsx
│   │   │   ├── card.tsx
│   │   │   ├── dialog.tsx
│   │   │   ├── select.tsx
│   │   │   ├── slider.tsx
│   │   │   ├── table.tsx
│   │   │   ├── tabs.tsx
│   │   │   ├── checkbox.tsx
│   │   │   ├── input.tsx
│   │   │   ├── label.tsx
│   │   │   ├── dropdown-menu.tsx
│   │   │   ├── toast.tsx
│   │   │   ├── progress.tsx
│   │   │   ├── badge.tsx
│   │   │   ├── separator.tsx
│   │   │   ├── skeleton.tsx
│   │   │   ├── alert.tsx              # ⭐ 추가
│   │   │   └── ...
│   │   │
│   │   ├── lotto/                     # 로또 전용 컴포넌트 ⭐
│   │   │   ├── LottoBall.tsx
│   │   │   ├── LottoBallGroup.tsx
│   │   │   └── NumberPicker.tsx
│   │   │
│   │   ├── dashboard/
│   │   │   ├── StatCard.tsx
│   │   │   ├── RecentWinnings.tsx
│   │   │   └── QuickActions.tsx
│   │   │
│   │   ├── winning-numbers/
│   │   │   ├── WinningNumberList.tsx
│   │   │   ├── WinningNumberCard.tsx
│   │   │   ├── AddNumberForm.tsx
│   │   │   └── CrawlDialog.tsx        # ⭐ 개선 (쿨다운)
│   │   │
│   │   ├── analysis/                  # ⭐⭐⭐ v1.1.1 대폭 강화
│   │   │   ├── FrequencyChart.tsx     # ⭐ Lazy Loading
│   │   │   ├── RegressionChart.tsx
│   │   │   ├── DetailTable.tsx
│   │   │   ├── FilterSyncCheckbox.tsx
│   │   │   ├── RangeSlider.tsx
│   │   │   ├── DrawRangeSlider.tsx    # ⭐⭐⭐ 신규 (회차 범위)
│   │   │   ├── StatisticsCard.tsx     # ⭐⭐⭐ 신규 (통계)
│   │   │   └── AllDrawsDialog.tsx     # ⭐⭐⭐ 신규 (전체 팝업)
│   │   │
│   │   ├── custom-analysis/
│   │   │   ├── CustomList.tsx
│   │   │   ├── DirectInputForm.tsx
│   │   │   ├── GroupForm.tsx
│   │   │   └── FormulaEditor.tsx
│   │   │
│   │   ├── ai/
│   │   │   ├── FixedNumberSelector.tsx
│   │   │   ├── ExcludedNumberSelector.tsx
│   │   │   ├── AIRecommendation.tsx
│   │   │   └── AIScoreCard.tsx
│   │   │
│   │   ├── filter/
│   │   │   ├── FilterPanel.tsx
│   │   │   ├── FilterItem.tsx
│   │   │   ├── FilterCounter.tsx      # ⭐ 실시간
│   │   │   └── FilterPreset.tsx
│   │   │
│   │   ├── combination/
│   │   │   ├── CombinationList.tsx
│   │   │   ├── CombinationCard.tsx
│   │   │   └── SaveButton.tsx
│   │   │
│   │   ├── ai-combination/
│   │   │   ├── AICombinationList.tsx
│   │   │   ├── AIScoreVisualization.tsx
│   │   │   └── GenerateButton.tsx
│   │   │
│   │   ├── output/
│   │   │   ├── PDFPreview.tsx
│   │   │   ├── PNGPreview.tsx
│   │   │   ├── DownloadButton.tsx
│   │   │   └── PrintSettings.tsx
│   │   │
│   │   └── verification/
│   │       ├── VerificationResult.tsx
│   │       ├── MatchVisualization.tsx
│   │       └── PrizeCalculation.tsx
│   │
│   ├── lib/                           # 라이브러리
│   │   │
│   │   ├── api/                       # API 클라이언트
│   │   │   ├── winning-numbers.ts
│   │   │   ├── analysis.ts            # ⭐ range 파라미터 지원
│   │   │   ├── custom-analysis.ts
│   │   │   ├── ai.ts
│   │   │   ├── filter.ts
│   │   │   ├── combination.ts
│   │   │   └── verification.ts
│   │   │
│   │   ├── store/                     # Zustand 스토어
│   │   │   ├── dashboardStore.ts
│   │   │   ├── analysisStore.ts
│   │   │   ├── customStore.ts
│   │   │   ├── aiStore.ts
│   │   │   ├── filterStore.ts
│   │   │   ├── combinationStore.ts
│   │   │   └── outputStore.ts
│   │   │
│   │   ├── hooks/                     # Custom Hooks
│   │   │   ├── useWinningNumbers.ts
│   │   │   ├── useAnalysis.ts         # ⭐ range 상태 관리
│   │   │   ├── useFilter.ts
│   │   │   └── useCombination.ts
│   │   │
│   │   ├── utils/                     # 유틸리티
│   │   │   ├── statistics.ts          # ⭐⭐⭐ 신규 (통계 계산)
│   │   │   ├── combination.ts
│   │   │   ├── regression.ts
│   │   │   ├── formatters.ts
│   │   │   ├── validation.ts
│   │   │   ├── date.ts
│   │   │   ├── canvasExport.ts        # ⭐ 신규 (Canvas API)
│   │   │   └── performanceMonitor.ts  # ⭐ 신규
│   │   │
│   │   ├── ai/
│   │   │   ├── lstm-model.ts
│   │   │   ├── scoring.ts
│   │   │   ├── ensemble.ts
│   │   │   └── prediction.ts
│   │   │
│   │   ├── constants/
│   │   │   ├── lotto.ts
│   │   │   ├── analysis.ts
│   │   │   └── colors.ts
│   │   │
│   │   ├── db/                        # 데이터베이스
│   │   │   ├── prisma.ts              # ⭐ WAL 모드
│   │   │   ├── writeQueue.ts          # ⭐ 신규
│   │   │   └── healthCheck.ts         # ⭐ 신규
│   │   │
│   │   └── workers/                   # Web Workers
│   │       └── filterWorker.ts        # (클라이언트 사이드)
│   │
│   └── types/                         # TypeScript 타입
│       ├── winning-number.ts
│       ├── analysis.ts                # ⭐ Statistics 타입 추가
│       ├── custom-analysis.ts
│       ├── ai.ts
│       ├── filter.ts
│       ├── combination.ts
│       └── index.ts
│
├── prisma/                            # Prisma (SQLite)
│   ├── schema.prisma                  # 스키마 정의 (WAL ⭐)
│   ├── dev.db                         # SQLite 파일 ⭐
│   ├── dev.db-wal                     # Write-Ahead Log ⭐
│   ├── dev.db-shm                     # Shared Memory ⭐
│   ├── migrations/                    # 마이그레이션
│   │   ├── 20241225_init/
│   │   └── 20241225_enable_wal.sql    # ⭐ WAL 활성화
│   └── backups/                       # 백업 (수동)
│       └── dev_20241225.db
│
├── public/                            # 정적 파일
│   ├── favicon.ico
│   ├── logo.png
│   │
│   └── workers/                       # ⭐ Web Workers (신규)
│       ├── aiWorker.js                # AI 연산
│       ├── filterWorker.js            # 필터 연산
│       └── chartWorker.js             # 차트 연산 (선택)
│
├── scripts/                           # 유틸리티 스크립트
│   ├── reset-db.ts
│   ├── seed.ts
│   ├── backup.ts
│   └── validate-data.ts               # ⭐ 신규
│
├── .env.local                         # 환경 변수
├── .gitignore
├── next.config.js
├── tailwind.config.js                 # ⭐ 유연한 레이아웃
├── tsconfig.json
├── package.json
├── package-lock.json
└── README.md
```

---

# 로또 AI v1.1.1 완전 명세서 - 2부

**(1부에서 계속)**

---

## 7. 핵심 컴포넌트

### 7.1 LottoBall (가장 중요) ⭐

```tsx
// src/components/lotto/LottoBall.tsx
import { cn } from '@/lib/utils'
import { getBallColor, BALL_COLORS } from '@/lib/constants/colors'

interface LottoBallProps {
  number: number                    // 1-45
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl'
  dimmed?: boolean                  // 흐림 처리
  isMatched?: boolean               // 매칭 상태 (애니메이션)
  isBonus?: boolean                 // 보너스볼
  contextColor?: string             // 분석별 컨텍스트 컬러
  onClick?: () => void
  className?: string
}

export function LottoBall({
  number,
  size = 'lg',
  dimmed = false,
  isMatched = false,
  isBonus = false,
  contextColor,
  onClick,
  className,
}: LottoBallProps) {
  // 기본 컬러 결정 (범위별)
  const getDefaultColor = (num: number) => {
    if (num <= 10) return 'bg-yellow-400 border-yellow-600 text-yellow-900'
    if (num <= 20) return 'bg-blue-400 border-blue-600 text-blue-900'
    if (num <= 30) return 'bg-red-400 border-red-600 text-red-900'
    if (num <= 40) return 'bg-gray-400 border-gray-600 text-gray-900'
    return 'bg-green-400 border-green-600 text-green-900'
  }
  
  // 보너스볼 컬러
  const bonusColor = 'bg-cyan-400 border-cyan-600 text-cyan-900'
  
  // 최종 컬러 결정
  const finalColor = isBonus 
    ? bonusColor 
    : contextColor || getDefaultColor(number)
  
  // 크기 클래스
  const sizeClasses = {
    xs: 'w-6 h-6 text-xs',      // 24px
    sm: 'w-8 h-8 text-sm',      // 32px
    md: 'w-10 h-10 text-base',  // 40px
    lg: 'w-12 h-12 text-lg',    // 48px (기본)
    xl: 'w-16 h-16 text-2xl',   // 64px
  }
  
  return (
    <div
      className={cn(
        // 기본 스타일
        'rounded-full border-2 font-bold',
        'flex items-center justify-center',
        'transition-all duration-200',
        
        // 크기
        sizeClasses[size],
        
        // 컬러
        finalColor,
        
        // 상태별 스타일
        dimmed && 'opacity-30 grayscale',
        isMatched && 'ring-4 ring-green-500 ring-offset-2 scale-110 animate-pulse',
        
        // 인터랙션
        onClick && 'cursor-pointer hover:scale-110 active:scale-95',
        
        // 추가 클래스
        className
      )}
      onClick={onClick}
      role={onClick ? 'button' : undefined}
      tabIndex={onClick ? 0 : undefined}
    >
      {number}
    </div>
  )
}
```

### 7.2 LottoBallGroup

```tsx
// src/components/lotto/LottoBallGroup.tsx
import { LottoBall } from './LottoBall'
import { cn } from '@/lib/utils'

interface LottoBallGroupProps {
  numbers: number[]               // 6개 (정렬됨)
  bonusNumber?: number            // 보너스 (선택)
  matchedNumbers?: number[]       // 매칭된 번호들
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl'
  showBonus?: boolean             // 보너스 표시 여부
  contextColors?: Record<number, string>  // 번호별 컨텍스트 컬러
  vertical?: boolean              // 세로 배치
  className?: string
}

export function LottoBallGroup({
  numbers,
  bonusNumber,
  matchedNumbers = [],
  size = 'lg',
  showBonus = true,
  contextColors,
  vertical = false,
  className,
}: LottoBallGroupProps) {
  return (
    <div className={cn(
      'flex items-center gap-3',
      vertical && 'flex-col',
      className
    )}>
      {/* 메인 번호 6개 */}
      {numbers.map((num) => (
        <LottoBall
          key={num}
          number={num}
          size={size}
          isMatched={matchedNumbers.includes(num)}
          contextColor={contextColors?.[num]}
        />
      ))}
      
      {/* 보너스 구분자 */}
      {showBonus && bonusNumber && (
        <>
          <span className={cn(
            'font-bold text-gray-400',
            size === 'xs' && 'text-sm',
            size === 'sm' && 'text-lg',
            size === 'md' && 'text-xl',
            size === 'lg' && 'text-2xl',
            size === 'xl' && 'text-3xl',
          )}>
            +
          </span>
          <LottoBall
            number={bonusNumber}
            size={size}
            isBonus
            isMatched={matchedNumbers.includes(bonusNumber)}
          />
        </>
      )}
    </div>
  )
}
```

### 7.3 FrequencyChart (Lazy Loading ⭐)

```tsx
// src/components/analysis/FrequencyChart.tsx
'use client'

import { useInView } from 'react-intersection-observer'
import { Skeleton } from '@/components/ui/skeleton'
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  Legend,
  ReferenceLine,
} from 'recharts'

interface FrequencyChartProps {
  data: Array<{
    name: string
    frequency: number
  }>
  averageValue?: number
  color?: string
  height?: number
}

export function FrequencyChart({
  data,
  averageValue,
  color = '#22c55e',
  height = 600,
}: FrequencyChartProps) {
  // ⭐ Lazy Loading
  const { ref, inView } = useInView({
    triggerOnce: true,
    threshold: 0.1,
    rootMargin: '100px',
  })
  
  return (
    <div ref={ref} className="w-full">
      {inView ? (
        <BarChart
          width={1200}
          height={height}
          data={data}
          margin={{ top: 20, right: 30, left: 20, bottom: 80 }}
        >
          <CartesianGrid strokeDasharray="3 3" />
          <XAxis 
            dataKey="name" 
            angle={-45} 
            textAnchor="end"
            height={100}
          />
          <YAxis label={{ value: '출현 횟수', angle: -90, position: 'insideLeft' }} />
          <Tooltip />
          <Legend />
          
          {averageValue && (
            <ReferenceLine 
              y={averageValue} 
              stroke="#ef4444" 
              strokeDasharray="3 3"
              label={{ value: `평균: ${averageValue.toFixed(1)}`, fill: '#ef4444' }}
            />
          )}
          
          <Bar 
            dataKey="frequency" 
            fill={color}
            name="출현 횟수"
          />
        </BarChart>
      ) : (
        <ChartSkeleton height={height} />
      )}
    </div>
  )
}

function ChartSkeleton({ height }: { height: number }) {
  return (
    <div className="space-y-4 animate-pulse" style={{ height }}>
      <Skeleton className="h-8 w-48" />
      <Skeleton className="h-full w-full rounded-lg" />
      <div className="flex justify-center gap-4">
        <Skeleton className="h-4 w-32" />
        <Skeleton className="h-4 w-32" />
      </div>
    </div>
  )
}
```

### 7.4 DrawRangeSlider (⭐⭐⭐ v1.1.1 신규)

```tsx
// src/components/analysis/DrawRangeSlider.tsx
'use client'

import { useState } from 'react'
import { Slider } from '@/components/ui/slider'
import { Button } from '@/components/ui/button'
import { Badge } from '@/components/ui/badge'

interface DrawRangeSliderProps {
  totalDraws: number                    // 전체 회차 수 (예: 1095)
  defaultRange?: number                 // 기본 범위 (예: 100)
  onChange: (range: number) => void     // 범위 변경 콜백
}

export function DrawRangeSlider({
  totalDraws,
  defaultRange = 100,
  onChange,
}: DrawRangeSliderProps) {
  const [range, setRange] = useState(defaultRange)
  
  // 슬라이더 값 변경
  const handleChange = (value: number[]) => {
    setRange(value[0])
  }
  
  // 적용 버튼
  const handleApply = () => {
    onChange(range)
  }
  
  // 프리셋 버튼
  const handlePreset = (value: number) => {
    setRange(value)
    onChange(value)
  }
  
  return (
    <div className="bg-white p-6 rounded-xl border border-gray-200 space-y-4">
      <div className="flex items-center justify-between">
        <h3 className="text-lg font-semibold">회차 범위 설정</h3>
        <Badge variant="outline">
          최근 {range === totalDraws ? '전체' : range}회
        </Badge>
      </div>
      
      {/* 슬라이더 */}
      <div className="space-y-3">
        <Slider
          value={[range]}
          onValueChange={handleChange}
          min={10}
          max={totalDraws}
          step={10}
          className="w-full"
        />
        
        {/* 범위 표시 */}
        <div className="flex justify-between text-sm text-gray-600">
          <span>10회</span>
          <span className="font-semibold text-blue-600">
            {range}회 선택됨
          </span>
          <span>전체 {totalDraws}회</span>
        </div>
      </div>
      
      {/* 프리셋 버튼 */}
      <div className="flex gap-2">
        <Button
          variant="outline"
          size="sm"
          onClick={() => handlePreset(50)}
        >
          최근 50회
        </Button>
        <Button
          variant="outline"
          size="sm"
          onClick={() => handlePreset(100)}
        >
          최근 100회
        </Button>
        <Button
          variant="outline"
          size="sm"
          onClick={() => handlePreset(500)}
        >
          최근 500회
        </Button>
        <Button
          variant="outline"
          size="sm"
          onClick={() => handlePreset(totalDraws)}
        >
          전체
        </Button>
      </div>
      
      {/* 적용 버튼 */}
      <Button 
        onClick={handleApply}
        className="w-full"
      >
        범위 적용하여 다시 분석
      </Button>
    </div>
  )
}
```

### 7.5 StatisticsCard (⭐⭐⭐ v1.1.1 신규)

```tsx
// src/components/analysis/StatisticsCard.tsx
'use client'

interface StatisticsCardProps {
  statistics: {
    min: number
    max: number
    mean: number
    median: number
    mode: number
    stdDev: number
  }
  label?: string  // 값의 단위 (예: '총합', '개수')
}

export function StatisticsCard({ statistics, label = '값' }: StatisticsCardProps) {
  const stats = [
    { 
      label: '최소값', 
      value: statistics.min,
      color: 'text-blue-600',
      bg: 'bg-blue-50',
    },
    { 
      label: '최대값', 
      value: statistics.max,
      color: 'text-red-600',
      bg: 'bg-red-50',
    },
    { 
      label: '평균', 
      value: statistics.mean.toFixed(1),
      color: 'text-green-600',
      bg: 'bg-green-50',
    },
    { 
      label: '중앙값', 
      value: statistics.median,
      color: 'text-purple-600',
      bg: 'bg-purple-50',
    },
    { 
      label: '최빈값', 
      value: statistics.mode,
      color: 'text-orange-600',
      bg: 'bg-orange-50',
    },
    { 
      label: '표준편차', 
      value: statistics.stdDev.toFixed(1),
      color: 'text-gray-600',
      bg: 'bg-gray-50',
    },
  ]
  
  return (
    <div className="bg-white p-6 rounded-xl border border-gray-200">
      <h3 className="text-lg font-semibold mb-4">주요 통계</h3>
      
      <div className="grid grid-cols-3 gap-4">
        {stats.map((stat) => (
          <div 
            key={stat.label}
            className={`${stat.bg} p-4 rounded-lg`}
          >
            <p className="text-sm text-gray-600 mb-1">{stat.label}</p>
            <p className={`text-2xl font-bold ${stat.color}`}>
              {stat.value}
            </p>
          </div>
        ))}
      </div>
      
      {/* 추가 정보 */}
      <div className="mt-4 p-3 bg-blue-50 rounded-lg">
        <p className="text-sm text-blue-800">
          💡 <strong>평균 {statistics.mean.toFixed(1)}</strong>을 기준으로 
          ±{statistics.stdDev.toFixed(1)} 범위가 
          약 68%의 회차를 포함합니다.
        </p>
      </div>
    </div>
  )
}
```

### 7.6 AllDrawsDialog (⭐⭐⭐ v1.1.1 신규)

```tsx
// src/components/analysis/AllDrawsDialog.tsx
'use client'

import { useState } from 'react'
import { Dialog, DialogContent, DialogHeader, DialogTitle } from '@/components/ui/dialog'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import { LottoBallGroup } from '@/components/lotto/LottoBallGroup'
import { ChevronLeft, ChevronRight, Download, FileText } from 'lucide-react'

interface AllDrawsDialogProps {
  open: boolean
  onOpenChange: (open: boolean) => void
  data: Array<{
    drawNo: number
    date: string
    numbers: number[]
    bonusNumber: number
    value: number | string
    contextColors?: Record<number, string>
  }>
  valueLabel: string  // '총합', '홀짝 비율' 등
}

export function AllDrawsDialog({
  open,
  onOpenChange,
  data,
  valueLabel,
}: AllDrawsDialogProps) {
  const [currentPage, setCurrentPage] = useState(1)
  const [searchDrawNo, setSearchDrawNo] = useState('')
  const [searchDate, setSearchDate] = useState('')
  
  const itemsPerPage = 10
  const totalPages = Math.ceil(data.length / itemsPerPage)
  
  // 검색 필터링
  const filteredData = data.filter((row) => {
    if (searchDrawNo && !row.drawNo.toString().includes(searchDrawNo)) {
      return false
    }
    if (searchDate && !row.date.includes(searchDate)) {
      return false
    }
    return true
  })
  
  // 페이지네이션
  const startIndex = (currentPage - 1) * itemsPerPage
  const endIndex = startIndex + itemsPerPage
  const currentData = filteredData.slice(startIndex, endIndex)
  
  // CSV 내보내기
  const handleExportCSV = () => {
    const headers = ['회차', '추첨일', '번호1', '번호2', '번호3', '번호4', '번호5', '번호6', '보너스', valueLabel]
    const rows = filteredData.map((row) => [
      row.drawNo,
      row.date,
      ...row.numbers,
      row.bonusNumber,
      row.value,
    ])
    
    const csv = [
      headers.join(','),
      ...rows.map(row => row.join(',')),
    ].join('\n')
    
    const blob = new Blob(['\uFEFF' + csv], { type: 'text/csv;charset=utf-8;' })
    const url = URL.createObjectURL(blob)
    const link = document.createElement('a')
    link.href = url
    link.download = `분석결과_${new Date().toISOString().split('T')[0]}.csv`
    link.click()
  }
  
  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent className="max-w-4xl max-h-[80vh]">
        <DialogHeader>
          <DialogTitle>
            전체 회차 상세 데이터 ({filteredData.length}개)
          </DialogTitle>
        </DialogHeader>
        
        {/* 검색 */}
        <div className="flex gap-4 mb-4">
          <Input
            placeholder="회차 번호 검색"
            value={searchDrawNo}
            onChange={(e) => setSearchDrawNo(e.target.value)}
            className="w-40"
          />
          <Input
            placeholder="날짜 검색 (YYYY-MM-DD)"
            value={searchDate}
            onChange={(e) => setSearchDate(e.target.value)}
            className="w-60"
          />
          <Button
            variant="outline"
            size="sm"
            onClick={() => {
              setSearchDrawNo('')
              setSearchDate('')
            }}
          >
            초기화
          </Button>
        </div>
        
        {/* 테이블 */}
        <div className="border rounded-lg overflow-hidden">
          <div className="overflow-y-auto max-h-[400px]">
            <table className="w-full">
              <thead className="bg-gray-50 sticky top-0">
                <tr className="border-b">
                  <th className="px-4 py-3 text-left text-sm font-semibold">회차</th>
                  <th className="px-4 py-3 text-left text-sm font-semibold">추첨일</th>
                  <th className="px-4 py-3 text-left text-sm font-semibold">당첨번호</th>
                  <th className="px-4 py-3 text-right text-sm font-semibold">{valueLabel}</th>
                </tr>
              </thead>
              <tbody>
                {currentData.map((row) => (
                  <tr key={row.drawNo} className="border-b hover:bg-gray-50">
                    <td className="px-4 py-3 font-medium">{row.drawNo}</td>
                    <td className="px-4 py-3 text-sm text-gray-600">{row.date}</td>
                    <td className="px-4 py-3">
                      <LottoBallGroup
                        numbers={row.numbers}
                        bonusNumber={row.bonusNumber}
                        size="sm"
                        contextColors={row.contextColors}
                      />
                    </td>
                    <td className="px-4 py-3 text-right font-semibold text-lg">
                      {row.value}
                    </td>
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
        </div>
        
        {/* 페이지네이션 */}
        <div className="flex items-center justify-between mt-4">
          <div className="text-sm text-gray-600">
            {filteredData.length}개 중 {startIndex + 1}-{Math.min(endIndex, filteredData.length)}
          </div>
          
          <div className="flex items-center gap-2">
            <Button
              variant="outline"
              size="sm"
              onClick={() => setCurrentPage(p => Math.max(1, p - 1))}
              disabled={currentPage === 1}
            >
              <ChevronLeft className="h-4 w-4" />
            </Button>
            
            <span className="text-sm px-4">
              {currentPage} / {totalPages}
            </span>
            
            <Button
              variant="outline"
              size="sm"
              onClick={() => setCurrentPage(p => Math.min(totalPages, p + 1))}
              disabled={currentPage === totalPages}
            >
              <ChevronRight className="h-4 w-4" />
            </Button>
          </div>
        </div>
        
        {/* 액션 버튼 */}
        <div className="flex justify-end gap-2 pt-4 border-t">
          <Button
            variant="outline"
            onClick={handleExportCSV}
          >
            <Download className="h-4 w-4 mr-2" />
            CSV 내보내기
          </Button>
          <Button onClick={() => onOpenChange(false)}>
            닫기
          </Button>
        </div>
      </DialogContent>
    </Dialog>
  )
}
```

---

# 로또 AI v1.1.1 완전 명세서 - 3부

**(2부에서 계속)**

---

## 10. 전체 화면 상세 명세 (39+ Screens)

**중요**: 당첨번호 표시 방식은 **페이지/분석별로 다릅니다**.

### 당첨번호 표시 방식 가이드

| 페이지/분석 | 표시 방식 | 설명 |
|------------|----------|------|
| **당첨번호 관리** | ✅ 셀 형태 | 연한 배경 + 진한 텍스트 (범위별 색상) |
| **커스텀분석** | ✅ 셀 형태 | 당첨번호 관리와 동일 |
| **기초분석 - 이월수** | 🔴 원형 공 | 이월수=빨강공, 그 외=회색공 |
| **기초분석 - 홀짝** | 🟡🟢 원형 공 | 홀수=노랑공, 짝수=연두공 |
| **기초분석 - 소수/합성수** | 🟡🟢⚪ 원형 공 | 1=회색공, 소수=노랑공, 합성수=연두공 |
| **기초분석 - 저고** | 🔵🔴 원형 공 | 저(1-22)=파랑공, 고(23-45)=빨강공 |
| **기초분석 - 총합/끝수합/AC값 등** | 🎨 원형 공 | 범위별 기본 색상 (1-10 노랑, 11-20 파랑 등) |
| **대시보드** | 🎨 원형 공 | 범위별 기본 색상 |

---

### 10.1 대시보드 (1 screen)

#### 10.1.1 화면 구성 (실제 디자인 기반)

```
┌─────────────────────────────────────────────────────────────────────┐
│ 로또 AI         │ 대시보드 개요          🔍 추첨,전략 검색... 🔔 + 새 분석 │
│ 분석 플랫폼     │                                                      │
├─────────────────┼──────────────────────────────────────────────────┤
│                 │                                                      │
│ 🟢 대시보드     │ Alex님, 환영합니다!                                 │
│                 │ AI 모델이 최신 추첨 패턴을 분석했습니다.            │
│ 📊 당첨번호     │                                                      │
│                 │ [🕐 마지막 업데이트: 방금 전] [🔄 최근 당첨번호 업데이트] [📊 분석 업데이트] │
│ 📈 기초분석     │                                                      │
│                 │ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│ 🎯 커스텀분석   │ │📍+2 이번주│ │🔷생성됨 │ │📊+5%정확도│ │🕐       │ │
│                 │ │활성 분석  │ │필터조합수│ │AI예측정확도│ │2022회차│ │
│ 🤖 AI분석       │ │    12    │ │  1,450  │ │    68%   │ │추첨까지 │ │
│                 │ │          │ │         │ │          │ │04h 30m │ │
│ 🔍 필터         │ └──────────┘ └──────────┘ └──────────┘ └──────────┘ │
│                 │                                                      │
│ 🎲 조합         │ ┌─────────────────────┐ ┌─────────────────────────┐│
│                 │ │2021회차 조합 결과    │ │2021회차 추첨 결과 상세  ││
│ 🤖 AI조합       │ │생성된 조합의 전체    │ │(Top 10)  전체 기록 보기→││
│                 │ │당첨 현황입니다.      │ │                         ││
│ 📄 출력         │ │                     │ │순번│선택번호    │결과   ││
│                 │ │총조합수              │ │────┼───────────┼───── ││
│ ✅ 검증         │ │  100개              │ │ 1  │⚪⚪⚪⚪⚪⚪│ 3등  ││
│                 │ │                     │ │    │06 12 23 34 38 42│    ││
│                 │ │┌──────────────┐    │ │────┼───────────┼───── ││
│                 │ ││1등      0개  │    │ │ 2  │⚪⚪⚪⚪⚪⚪│ 4등  ││
│                 │ │└──────────────┘    │ │────┼───────────┼───── ││
│                 │ │┌──────────────┐    │ │ 3  │⚪⚪⚪⚪⚪⚪│ 4등  ││
│                 │ ││2등      0개  │    │ │────┼───────────┼───── ││
│                 │ │└──────────────┘    │ │ 4  │⚪⚪⚪⚪⚪⚪│ 4등  ││
│                 │ │┌──────────────┐    │ │────┼───────────┼───── ││
│                 │ ││3등      1개  │    │ │ 5  │⚪⚪⚪⚪⚪⚪│ 5등  ││
│                 │ │└──────────────┘    │ │────┼───────────┼───── ││
│                 │ │┌──────────────┐    │ │ 6  │⚪⚪⚪⚪⚪⚪│ 5등  ││
│                 │ ││4등     10개  │    │ │────┼───────────┼───── ││
│                 │ │└──────────────┘    │ │ 7  │⚪⚪⚪⚪⚪⚪│ 5등  ││
│                 │ │┌──────────────┐    │ │────┼───────────┼───── ││
│                 │ ││5등     25개  │    │ │ ... (더보기)          ││
│                 │ │└──────────────┘    │ └─────────────────────────┘│
│                 │ │┌──────────────┐    │                             │
│                 │ ││낙첨    64개  │    │                             │
│                 │ │└──────────────┘    │                             │
│                 │ └─────────────────────┘                             │
└─────────────────┴──────────────────────────────────────────────────┘

볼 컬러 (원형 공 - 범위별 기본 색상):
- 녹색 볼: #34D399 (41-45)
- 회색 볼: #9CA3AF (31-40)
- 노랑 볼: #FCD34D (1-10)
- 파랑 볼: #60A5FA (11-20)
- 빨강 볼: #F87171 (21-30)

등수 뱃지:
- 3등: #F97316 (주황)
- 4등: #3B82F6 (파랑)
- 5등: #10B981 (녹색)
```

#### 10.1.2 컴포넌트 구조

```tsx
// src/app/page.tsx (대시보드 - 실제 디자인 기반)
'use client'

import { useState, useEffect } from 'react'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import { Badge } from '@/components/ui/badge'
import { LottoBallGroup } from '@/components/lotto/LottoBallGroup'
import { 
  PinIcon, 
  FilterIcon, 
  TrendingUpIcon, 
  ClockIcon,
  RefreshCwIcon,
  BarChartIcon,
  BellIcon,
  PlusIcon,
  SearchIcon
} from 'lucide-react'

export default function DashboardPage() {
  const [stats, setStats] = useState<any>(null)
  const [recentDraws, setRecentDraws] = useState<any[]>([])
  const [prizeResults, setPrizeResults] = useState<any>(null)
  const [topCombinations, setTopCombinations] = useState<any[]>([])

  useEffect(() => {
    loadDashboardData()
  }, [])

  const loadDashboardData = async () => {
    // API 호출
    const statsData = await fetch('/api/dashboard/stats').then(r => r.json())
    const drawsData = await fetch('/api/winning-numbers?limit=5').then(r => r.json())
    const prizeData = await fetch('/api/dashboard/prize-results').then(r => r.json())
    const topData = await fetch('/api/dashboard/top-combinations').then(r => r.json())
    
    setStats(statsData)
    setRecentDraws(drawsData)
    setPrizeResults(prizeData)
    setTopCombinations(topData)
  }

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Top Bar */}
      <header className="bg-white border-b border-gray-200 px-8 py-4">
        <div className="flex items-center justify-between">
          <h1 className="text-2xl font-semibold text-gray-900">대시보드 개요</h1>
          
          <div className="flex items-center gap-4">
            {/* Search */}
            <div className="relative">
              <Input
                placeholder="추첨, 전략 검색..."
                className="w-80 pl-10"
              />
              <SearchIcon className="absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 text-gray-400" />
            </div>
            
            {/* Notification */}
            <Button variant="ghost" size="icon">
              <BellIcon className="h-5 w-5 text-gray-600" />
            </Button>
            
            {/* New Analysis */}
            <Button className="bg-emerald-500 hover:bg-emerald-600">
              <PlusIcon className="h-4 w-4 mr-2" />
              새 분석
            </Button>
          </div>
        </div>
      </header>

      {/* Main Content */}
      <main className="p-8 space-y-8">
        {/* Welcome Section */}
        <Card>
          <CardContent className="pt-6">
            <h2 className="text-3xl font-bold mb-2">Alex님, 환영합니다!</h2>
            <p className="text-gray-600 mb-6">
              AI 모델이 최신 추첨 패턴을 분석했습니다.
            </p>
            
            <div className="flex gap-3">
              <Button variant="outline" size="sm">
                <ClockIcon className="h-4 w-4 mr-2" />
                마지막 업데이트: 방금 전
              </Button>
              <Button variant="outline" size="sm">
                <RefreshCwIcon className="h-4 w-4 mr-2" />
                최근 당첨번호 업데이트
              </Button>
              <Button variant="outline" size="sm">
                <BarChartIcon className="h-4 w-4 mr-2" />
                분석 업데이트
              </Button>
            </div>
          </CardContent>
        </Card>

        {/* Statistics Cards */}
        <div className="grid grid-cols-4 gap-6">
          {/* Card 1 - 활성 분석 */}
          <Card>
            <CardContent className="pt-6">
              <div className="flex items-start justify-between mb-2">
                <PinIcon className="h-6 w-6 text-emerald-500" />
                <Badge className="bg-emerald-100 text-emerald-700">
                  +2 이번 주
                </Badge>
              </div>
              <p className="text-sm text-gray-600 mb-1">활성 분석</p>
              <p className="text-4xl font-bold text-gray-900">12</p>
            </CardContent>
          </Card>

          {/* Card 2 - 필터조합수 */}
          <Card>
            <CardContent className="pt-6">
              <div className="flex items-start justify-between mb-2">
                <FilterIcon className="h-6 w-6 text-blue-500" />
                <p className="text-sm text-gray-600">생성됨</p>
              </div>
              <p className="text-sm text-gray-600 mb-1">필터조합수</p>
              <p className="text-4xl font-bold text-gray-900">1,450</p>
            </CardContent>
          </Card>

          {/* Card 3 - AI 예측 정확도 */}
          <Card>
            <CardContent className="pt-6">
              <div className="flex items-start justify-between mb-2">
                <TrendingUpIcon className="h-6 w-6 text-purple-500" />
                <Badge className="bg-emerald-100 text-emerald-700">
                  +5% 정확도
                </Badge>
              </div>
              <p className="text-sm text-gray-600 mb-1">AI 예측 정확도</p>
              <p className="text-4xl font-bold text-gray-900">68%</p>
            </CardContent>
          </Card>

          {/* Card 4 - 추첨까지 */}
          <Card>
            <CardContent className="pt-6">
              <ClockIcon className="h-6 w-6 text-orange-500 mb-2" />
              <p className="text-sm text-gray-600 mb-1">2022회차 추첨까지</p>
              <p className="text-4xl font-bold text-gray-900 font-mono">04h 30m</p>
            </CardContent>
          </Card>
        </div>

        {/* Bottom Section: 2 Columns */}
        <div className="grid grid-cols-2 gap-6">
          {/* Left - 조합 결과 */}
          <Card>
            <CardHeader>
              <CardTitle>2021회차 조합 결과</CardTitle>
              <p className="text-sm text-gray-600">
                생성된 조합의 전체 당첨 현황입니다.
              </p>
            </CardHeader>
            <CardContent>
              <div className="mb-4">
                <p className="text-sm font-medium mb-2">총조합수</p>
                <p className="text-2xl font-bold">100개</p>
              </div>

              <div className="space-y-3">
                {/* 1등 */}
                <div className="bg-yellow-50 border border-yellow-200 rounded-lg p-4 flex justify-between items-center">
                  <span className="text-sm font-medium text-yellow-800">1등</span>
                  <span className="text-xl font-bold text-yellow-900">0개</span>
                </div>

                {/* 2등 */}
                <div className="bg-blue-50 border border-blue-200 rounded-lg p-4 flex justify-between items-center">
                  <span className="text-sm font-medium text-blue-800">2등</span>
                  <span className="text-xl font-bold text-blue-900">0개</span>
                </div>

                {/* 3등 */}
                <div className="bg-orange-50 border border-orange-200 rounded-lg p-4 flex justify-between items-center">
                  <span className="text-sm font-medium text-orange-800">3등</span>
                  <span className="text-xl font-bold text-orange-900">1개</span>
                </div>

                {/* 4등 */}
                <div className="bg-gray-50 border border-gray-200 rounded-lg p-4 flex justify-between items-center">
                  <span className="text-sm font-medium text-gray-800">4등</span>
                  <span className="text-xl font-bold text-gray-900">10개</span>
                </div>

                {/* 5등 */}
                <div className="bg-emerald-50 border border-emerald-200 rounded-lg p-4 flex justify-between items-center">
                  <span className="text-sm font-medium text-emerald-800">5등</span>
                  <span className="text-xl font-bold text-emerald-900">25개</span>
                </div>

                {/* 낙첨 */}
                <div className="bg-red-50 border border-red-200 rounded-lg p-4 flex justify-between items-center">
                  <span className="text-sm font-medium text-red-800">낙첨</span>
                  <span className="text-xl font-bold text-red-900">64개</span>
                </div>
              </div>
            </CardContent>
          </Card>

          {/* Right - 추첨 결과 상세 Top 10 */}
          <Card>
            <CardHeader>
              <div className="flex items-center justify-between">
                <div>
                  <CardTitle>2021회차 추첨 결과 상세 (Top 10)</CardTitle>
                </div>
                <Button variant="link" className="text-emerald-600">
                  전체 기록 보기 →
                </Button>
              </div>
            </CardHeader>
            <CardContent>
              <div className="space-y-1">
                {/* Table Header */}
                <div className="grid grid-cols-12 gap-2 pb-2 border-b text-sm font-semibold text-gray-600">
                  <div className="col-span-1">순번</div>
                  <div className="col-span-8">선택번호</div>
                  <div className="col-span-3 text-right">결과</div>
                </div>

                {/* Table Rows */}
                {topCombinations.map((combo, idx) => (
                  <div 
                    key={idx}
                    className="grid grid-cols-12 gap-2 py-3 border-b hover:bg-gray-50"
                  >
                    <div className="col-span-1 text-sm font-medium">{idx + 1}</div>
                    <div className="col-span-8">
                      <LottoBallGroup
                        numbers={combo.numbers}
                        variant="ball"
                        contextType="range"
                        size="sm"
                        showBonus={false}
                      />
                    </div>
                    <div className="col-span-3 text-right">
                      <Badge 
                        className={
                          combo.rank === '3등' ? 'bg-orange-100 text-orange-700' :
                          combo.rank === '4등' ? 'bg-blue-100 text-blue-700' :
                          'bg-emerald-100 text-emerald-700'
                        }
                      >
                        {combo.rank}
                      </Badge>
                    </div>
                  </div>
                ))}
              </div>
            </CardContent>
          </Card>
        </div>
      </main>
    </div>
  )
}
```

#### 10.1.3 데이터 구조

```typescript
// Dashboard Stats
interface DashboardStats {
  activeAnalyses: {
    count: number
    weeklyIncrease: number
  }
  filterCombinations: {
    count: number
    label: string
  }
  aiAccuracy: {
    percentage: number
    improvement: number
  }
  nextDraw: {
    drawNumber: number
    timeRemaining: string
  }
}

// Prize Results
interface PrizeResults {
  totalCombinations: number
  results: {
    rank: string  // '1등', '2등', etc.
    count: number
    color: string  // 배경색
  }[]
}

// Top Combinations
interface TopCombination {
  rank: number
  numbers: number[]
  prize: string  // '3등', '4등', '5등'
}
```

---

### 10.2 당첨번호 관리 (1 screen)

#### 10.2.1 화면 구성 (실제 디자인 기반 - 셀 형태 표)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ 🎯 Lotto Lab  │대시보드│당첨번호│기초분석│커스텀분석│AI분석│필터│조합│AI조합│출력│검증│  + 새 분석 │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                           │
│ 당첨번호 관리                                    🗒️ 수동업데이트  🔄 업데이트 │
│ 과거 모든 추첨 결과 및 당첨 내역을 확인하고 분석합니다.                  │
│                                                                           │
│ 🔍 회차 검색 (예: 1082)    [전체 기간 ▼]                                │
│                                                                           │
├───┬─────────┬────────────────────────────────┬──────────┬───────────────┤
│회차│ 추첨일  │     당첨번호      │ 보너스 │ 당첨인원 │   분석데이터   │
│   │         │ 1  2  3  4  5  6  │        │당첨인수 당첨금│합계 평수합 AC값 홀 짝 연번│
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1082│2023.10.24│[04][12][18][25][33][45]│ [07] │  7명 50.0억│ 137  35   8  3  3  0 │
│    │         │ Y   B   B   R   G   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1081│2023.10.17│[02][09][15][22][31][40]│ [03] │ 11명 32.0억│ 119  28   9  3  3  1 │
│    │         │ Y   Y   B   R   G   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1080│2023.10.10│[05][11][19][28][35][48]│ [09] │  6명 75.0억│ 146  45   7  4  2  0 │
│    │         │ Y   B   B   R   G   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1079│2023.10.03│[01][08][14][20][29][38]│ [05] │  9명 41.0억│ 110  30  10  2  4  0 │
│    │         │ Y   Y   B   B   R   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1078│2023.09.26│[06][13][21][27][34][42]│ [02] │ 12명 28.0억│ 143  23   6  3  3  0 │
│    │         │ Y   B   R   R   G   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1077│2023.09.19│[03][16][24][26][31][39]│ [11] │  5명 62.0억│ 139  29   7  2  4  0 │
│    │         │ Y   B   R   R   G   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1076│2023.09.12│[07][10][18][23][36][41]│ [06] │  3명 98.0억│ 135  25   8  4  2  1 │
│    │         │ Y   Y   B   R   G   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1075│2023.09.05│[01][23][24][35][44][45]│ [10] │  9명 29.0억│ 172  22  10  4  2  2 │
│    │         │ Y   R   R   G   G   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1074│2023.08.29│[01][06][20][27][28][37]│ [15] │ 12명 21.3억│ 119  29  10  3  3  1 │
│    │         │ Y   Y   B   R   R   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1073│2023.08.22│[06][18][28][30][32][38]│ [15] │ 11명 23.5억│ 152  32   6  0  6  0 │
│    │         │ Y   B   R   R   G   G  │  O   │        │                │
├───┼─────────┼────────────────────────────────┼──────────┼───────────────┤
│1072│2023.08.15│[16][18][20][23][32][43]│ [27] │ 12명 21.8억│ 152  22   6  2  4  0 │
│    │         │ B   B   B   R   G   G  │  O   │        │                │
└───┴─────────┴────────────────────────────────┴──────────┴───────────────┘

번호 색상 (표 셀 형태):
[숫자] = 셀 형태로 표시
- Y (노랑): 1-10 - 연한 노랑 배경(#FEF3C7) + 진한 노랑/주황 텍스트(#D97706) + 굵게
- B (파랑): 11-20 - 연한 파랑 배경(#DBEAFE) + 진한 파랑 텍스트(#1E40AF) + 굵게
- R (빨강): 21-30 - 연한 빨강 배경(#FEE2E2) + 진한 빨강 텍스트(#DC2626) + 굵게
- G (회색): 31-40 - 연한 회색 배경(#F3F4F6) + 진한 회색 텍스트(#4B5563) + 굵게
- G (녹색): 41-45 - 연한 녹색 배경(#D1FAE5) + 진한 녹색 텍스트(#059669) + 굵게
- O (보너스): 흰색 배경(#FFFFFF) + 주황 텍스트(#F97316) + 굵게
```

#### 10.2.2 컴포넌트 구조

```tsx
// src/app/winning-numbers/page.tsx (실제 디자인 기반 - 셀 형태)
'use client'

import { useState, useEffect } from 'react'
import { Input } from '@/components/ui/input'
import { Button } from '@/components/ui/button'
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'
import { SearchIcon, RefreshCwIcon, EditIcon } from 'lucide-react'

// 번호별 색상 헬퍼 (표 셀 형태 - 연한 배경 + 진한 텍스트)
const getNumberColor = (num: number) => {
  if (num <= 10) return 'bg-yellow-100 text-yellow-700'  // 연한 노랑 배경 + 진한 노랑 텍스트
  if (num <= 20) return 'bg-blue-100 text-blue-700'      // 연한 파랑 배경 + 진한 파랑 텍스트
  if (num <= 30) return 'bg-red-100 text-red-700'        // 연한 빨강 배경 + 진한 빨강 텍스트
  if (num <= 40) return 'bg-gray-200 text-gray-700'      // 연한 회색 배경 + 진한 회색 텍스트
  return 'bg-emerald-100 text-emerald-700'               // 연한 녹색 배경 + 진한 녹색 텍스트
}

const getBonusColor = () => 'bg-white text-orange-500 border border-gray-200'  // 흰색 배경 + 주황 텍스트

export default function WinningNumbersPage() {
  const [winningNumbers, setWinningNumbers] = useState<any[]>([])
  const [searchDrawNo, setSearchDrawNo] = useState('')
  const [dateFilter, setDateFilter] = useState('all')

  useEffect(() => {
    loadWinningNumbers()
  }, [])

  const loadWinningNumbers = async () => {
    const response = await fetch('/api/winning-numbers')
    const data = await response.json()
    setWinningNumbers(data)
  }

  return (
    <div className="min-h-screen bg-white">
      {/* Top Navigation Tabs */}
      <header className="bg-white border-b border-gray-200">
        <div className="flex items-center justify-between px-8 py-4">
          <div className="flex items-center gap-8">
            {/* Logo */}
            <div className="flex items-center gap-2">
              <div className="w-10 h-10 bg-emerald-500 rounded-lg flex items-center justify-center">
                <span className="text-white font-bold text-xl">🎯</span>
              </div>
              <span className="text-lg font-bold">Lotto Lab</span>
            </div>

            {/* Tabs */}
            <nav className="flex gap-1">
              {['대시보드', '당첨번호', '기초분석', '커스텀분석', 'AI 분석', '필터', '조합', 'AI 조합', '출력', '검증'].map((tab) => (
                <button
                  key={tab}
                  className={`px-4 py-2 text-sm font-medium rounded-lg transition-colors ${
                    tab === '당첨번호'
                      ? 'text-emerald-600 border-b-2 border-emerald-600'
                      : 'text-gray-600 hover:text-gray-900 hover:bg-gray-50'
                  }`}
                >
                  {tab}
                </button>
              ))}
            </nav>
          </div>

          {/* New Analysis Button */}
          <Button className="bg-emerald-500 hover:bg-emerald-600">
            + 새 분석
          </Button>
        </div>
      </header>

      {/* Main Content */}
      <main className="p-8">
        {/* Page Header */}
        <div className="flex items-start justify-between mb-6">
          <div>
            <h1 className="text-3xl font-bold mb-2">당첨번호 관리</h1>
            <p className="text-gray-600">
              과거 모든 추첨 결과 및 당첨 내역을 확인하고 분석합니다.
            </p>
          </div>

          <div className="flex gap-2">
            <Button variant="outline">
              <EditIcon className="h-4 w-4 mr-2" />
              수동업데이트
            </Button>
            <Button variant="outline">
              <RefreshCwIcon className="h-4 w-4 mr-2" />
              업데이트
            </Button>
          </div>
        </div>

        {/* Search & Filter */}
        <div className="flex gap-4 mb-6">
          <div className="relative flex-1 max-w-md">
            <SearchIcon className="absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 text-gray-400" />
            <Input
              placeholder="회차 검색 (예: 1082)"
              value={searchDrawNo}
              onChange={(e) => setSearchDrawNo(e.target.value)}
              className="pl-10"
            />
          </div>

          <Select value={dateFilter} onValueChange={setDateFilter}>
            <SelectTrigger className="w-48">
              <SelectValue placeholder="전체 기간" />
            </SelectTrigger>
            <SelectContent>
              <SelectItem value="all">전체 기간</SelectItem>
              <SelectItem value="2023">2023년</SelectItem>
              <SelectItem value="2022">2022년</SelectItem>
              <SelectItem value="recent">최근 50회</SelectItem>
            </SelectContent>
          </Select>
        </div>

        {/* Winning Numbers Table */}
        <div className="bg-white border border-gray-200 rounded-lg overflow-hidden">
          <table className="w-full">
            <thead className="bg-gray-50 border-b border-gray-200">
              <tr>
                <th className="px-4 py-3 text-left text-sm font-semibold text-gray-600">
                  회차
                </th>
                <th className="px-4 py-3 text-left text-sm font-semibold text-gray-600">
                  추첨일
                </th>
                <th className="px-4 py-3 text-left text-sm font-semibold text-gray-600" colSpan={7}>
                  당첨번호
                </th>
                <th className="px-4 py-3 text-left text-sm font-semibold text-gray-600">
                  보너스
                </th>
                <th className="px-4 py-3 text-left text-sm font-semibold text-gray-600" colSpan={2}>
                  당첨인원
                </th>
                <th className="px-4 py-3 text-left text-sm font-semibold text-gray-600" colSpan={6}>
                  분석데이터
                </th>
              </tr>
              <tr className="bg-gray-50 border-b border-gray-200">
                <th></th>
                <th></th>
                <th className="px-2 py-2 text-xs text-gray-500">1</th>
                <th className="px-2 py-2 text-xs text-gray-500">2</th>
                <th className="px-2 py-2 text-xs text-gray-500">3</th>
                <th className="px-2 py-2 text-xs text-gray-500">4</th>
                <th className="px-2 py-2 text-xs text-gray-500">5</th>
                <th className="px-2 py-2 text-xs text-gray-500">6</th>
                <th></th>
                <th></th>
                <th className="px-2 py-2 text-xs text-gray-500">당첨인수</th>
                <th className="px-2 py-2 text-xs text-gray-500">당첨금</th>
                <th className="px-2 py-2 text-xs text-gray-500">합계</th>
                <th className="px-2 py-2 text-xs text-gray-500">평수합</th>
                <th className="px-2 py-2 text-xs text-gray-500">AC값</th>
                <th className="px-2 py-2 text-xs text-gray-500">홀</th>
                <th className="px-2 py-2 text-xs text-gray-500">짝</th>
                <th className="px-2 py-2 text-xs text-gray-500">연번</th>
              </tr>
            </thead>
            
            <tbody>
              {winningNumbers.map((draw) => {
                const numbers = JSON.parse(draw.numbers)
                
                return (
                  <tr 
                    key={draw.drawNo}
                    className="border-b border-gray-100 hover:bg-gray-50"
                  >
                    {/* 회차 */}
                    <td className="px-4 py-4 font-bold">{draw.drawNo}</td>
                    
                    {/* 추첨일 */}
                    <td className="px-4 py-4 text-sm text-gray-600">
                      {new Date(draw.drawDate).toLocaleDateString('ko-KR')}
                    </td>
                    
                    {/* 당첨번호 1-6 (셀 형태) */}
                    {numbers.map((num: number, idx: number) => (
                      <td key={idx} className="px-1 py-4">
                        <div className={`px-3 py-2 text-center font-bold text-sm rounded ${getNumberColor(num)}`}>
                          {String(num).padStart(2, '0')}
                        </div>
                      </td>
                    ))}
                    
                    <td></td> {/* 빈 칸 */}
                    
                    {/* 보너스 (셀 형태 - 흰색 배경) */}
                    <td className="px-1 py-4">
                      <div className={`px-3 py-2 text-center font-bold text-sm rounded ${getBonusColor()}`}>
                        {String(draw.bonus).padStart(2, '0')}
                      </div>
                    </td>
                    
                    {/* 당첨인원 */}
                    <td className="px-4 py-4 text-sm">{draw.winnerCount}명</td>
                    <td className="px-4 py-4 text-sm">{draw.totalPrize}억</td>
                    
                    {/* 분석데이터 */}
                    <td className="px-2 py-4 text-sm text-center">{draw.sumVal}</td>
                    <td className="px-2 py-4 text-sm text-center">{draw.endSum}</td>
                    <td className="px-2 py-4 text-sm text-center">{draw.acValue}</td>
                    <td className="px-2 py-4 text-sm text-center">{draw.oddCount}</td>
                    <td className="px-2 py-4 text-sm text-center">{draw.evenCount}</td>
                    <td className="px-2 py-4 text-sm text-center">{draw.consecutiveCount}</td>
                  </tr>
                )
              })}
            </tbody>
          </table>
        </div>
      </main>
    </div>
  )
}
```

#### 10.2.3 데이터 구조

```typescript
// Winning Number (확장)
interface WinningNumber {
  drawNo: number
  drawDate: Date
  numbers: number[]  // [6개]
  bonus: number
  
  // 당첨 정보
  winnerCount: number      // 1등 당첨자 수
  totalPrize: number       // 총 당첨금 (억 단위)
  
  // 분석 데이터
  sumVal: number           // 합계
  endSum: number           // 평수합 (끝자리 합)
  acValue: number          // AC값
  oddCount: number         // 홀수 개수
  evenCount: number        // 짝수 개수
  consecutiveCount: number // 연번 개수
  
  // 메타
  createdAt: Date
  updatedAt: Date
}
```

---

### 10.3 기초분석 17종 - 공통 구조 (회차 범위 + 통계 + 팝업)

#### 10.3.1 공통 패턴

모든 기초분석 화면은 다음 공통 구조를 따릅니다:

```
1. 헤더 (제목 + 전체 회차 보기 버튼)
2. 설명 + 컨텍스트 컬러 범례
3. ⭐ 회차 범위 슬라이더 (신규)
4. ⭐ 주요 통계 카드 (신규)
5. 빈도 차트
6. 필터 동기화 (선택)
7. ⭐ 전체 회차 팝업 (신규)
```

#### 10.3.2 컨텍스트 컬러 적용 기준

| 분석 | 컨텍스트 컬러 | 설명 |
|------|--------------|------|
| **이월수** | 이월수=🔴빨강, 그 외=⚪회색 | 이전 회차 포함 여부 |
| **홀짝** | 홀수=🟡노랑, 짝수=🟢연두 | 숫자의 홀짝 |
| **소수/합성수** | 1=⚪회색, 소수=🟡노랑, 합성수=🟢연두 | 수학적 분류 |
| **저고** | 저(1-22)=🔵파랑, 고(23-45)=🔴빨강 | 범위 구분 |
| **총합/끝수합/AC값** | 범위별 기본 색상 | 1-10🟡, 11-20🔵, 21-30🔴, 31-40⚪, 41-45🟢 |
| **제곱수** | 제곱수=🟠주황, 그 외=⚪회색 | 수학적 속성 |
| **배수** | 3배수=🟡노랑, 5배수=🔵파랑, 그 외=⚪회색 | 배수 여부 |
| **연번** | 연번=🟢녹색, 그 외=⚪회색 | 연속 번호 |
| **끝수** | 끝수별 색상 (10가지) | 0~9 각각 다른 색상 |
| **동형수** | 동형수=🟣보라, 그 외=⚪회색 | 끝자리 동일 |
| **핫콜드** | 핫=🔴빨강, 콜드=🔵파랑, 그 외=⚪회색 | 출현 빈도 기준 |

#### 10.3.3 예시: 홀짝 분석

```tsx
// src/app/analysis/odd-even/page.tsx
'use client'

import { useState, useEffect } from 'react'
import { FrequencyChart } from '@/components/analysis/FrequencyChart'
import { DrawRangeSlider } from '@/components/analysis/DrawRangeSlider'
import { AllDrawsDialog } from '@/components/analysis/AllDrawsDialog'
import { Button } from '@/components/ui/button'
import { LottoBall } from '@/components/lotto/LottoBall'

export default function OddEvenAnalysisPage() {
  const [drawRange, setDrawRange] = useState(100)
  const [data, setData] = useState<any>(null)
  const [showAllDraws, setShowAllDraws] = useState(false)
  
  const loadData = async (range: number) => {
    const response = await fetch(`/api/analysis/odd-even?range=${range}`)
    const result = await response.json()
    setData(result)
  }
  
  return (
    <div className="space-y-8">
      {/* 1. 헤더 */}
      <div className="flex items-center justify-between">
        <h1 className="text-3xl font-bold">📊 홀짝 분석</h1>
        <Button onClick={() => setShowAllDraws(true)}>
          전체 회차 보기
        </Button>
      </div>
      
      {/* 2. 설명 + 컬러 범례 */}
      <div className="bg-blue-50 p-4 rounded-lg">
        <p className="mb-2">홀수와 짝수의 비율을 분석합니다.</p>
        <div className="flex gap-4">
          <div className="flex items-center gap-2">
            <LottoBall number={3} variant="ball" contextType="odd-even" size="xs" />
            <span>홀수 (노랑)</span>
          </div>
          <div className="flex items-center gap-2">
            <LottoBall number={4} variant="ball" contextType="odd-even" size="xs" />
            <span>짝수 (연두)</span>
          </div>
        </div>
      </div>
      
      {/* 3. 회차 범위 슬라이더 */}
      <DrawRangeSlider
        totalDraws={1095}
        defaultRange={drawRange}
        onChange={(range) => {
          setDrawRange(range)
          loadData(range)
        }}
      />
      
      {/* 4. 비율 통계 카드 */}
      <div className="bg-white p-6 rounded-xl border">
        <h3 className="text-lg font-semibold mb-4">비율 통계</h3>
        <div className="grid grid-cols-7 gap-4">
          {Object.entries(data.statistics.ratios).map(([ratio, count]) => (
            <div key={ratio} className="text-center p-4 bg-gray-50 rounded-lg">
              <p className="text-2xl font-bold text-blue-600">{ratio}</p>
              <p className="text-sm text-gray-600">{count}회</p>
              <p className="text-xs text-gray-500">
                ({((count / drawRange) * 100).toFixed(1)}%)
              </p>
            </div>
          ))}
        </div>
      </div>
      
      {/* 5. 빈도 차트 */}
      <FrequencyChart data={data.chartData} />
      
      {/* 7. 전체 회차 팝업 */}
      <AllDrawsDialog
        open={showAllDraws}
        onOpenChange={setShowAllDraws}
        data={data.detailData}
        contextType="odd-even"
      />
    </div>
  )
}
```

---

### 10.4 ~ 10.20 기초분석 나머지 화면들

*(기존 명세서의 각 분석 화면 내용 유지, 단 모두 회차 범위 슬라이더 + 통계 카드 + 전체 회차 팝업 추가)*

---

### 10.21 커스텀분석 (셀 형태 표)

커스텀분석에서는 **당첨번호 관리와 동일한 셀 형태**를 사용합니다.

```tsx
// 당첨번호 표시: 셀 형태
<LottoBall number={7} variant="cell" />
// → 연한 노랑 배경 + 진한 노랑 텍스트
```

---

### 10.22 ~ 10.39 나머지 화면들

*(기존 명세서 내용 유지)*

---

## 부록 A: 분석별 당첨번호 표시 방식 상세

### A-1. 셀 형태 (표 스타일)

**적용 페이지**: 당첨번호 관리, 커스텀분석

```typescript
// 셀 스타일
const getCellStyle = (num: number) => {
  if (num <= 10) return 'bg-yellow-100 text-yellow-700'
  if (num <= 20) return 'bg-blue-100 text-blue-700'
  if (num <= 30) return 'bg-red-100 text-red-700'
  if (num <= 40) return 'bg-gray-200 text-gray-700'
  return 'bg-emerald-100 text-emerald-700'
}

// JSX
<div className="px-3 py-2 rounded font-bold bg-yellow-100 text-yellow-700">
  04
</div>
```

---

### A-2. 원형 공 + 컨텍스트 컬러

#### A-2-1. 기초분석 - 이월수

```typescript
// 이월수 여부 확인
const isCarryOver = (num: number, prevNumbers: number[]) => {
  return prevNumbers.includes(num)
}

// 컨텍스트 컬러
const getContextColor = (num: number, isCarry: boolean) => {
  if (isCarry) return 'bg-red-500 border-red-700'      // 이월수: 빨강공
  return 'bg-gray-400 border-gray-600'                 // 그 외: 회색공
}
```

**시각적 예시**:
- 이월수: 🔴 빨강공
- 비이월수: ⚪ 회색공

---

#### A-2-2. 기초분석 - 홀짝

```typescript
// 홀짝 판단
const isOdd = (num: number) => num % 2 === 1

// 컨텍스트 컬러
const getContextColor = (num: number) => {
  if (isOdd(num)) {
    return 'bg-yellow-400 border-yellow-600'    // 홀수: 노랑공
  }
  return 'bg-lime-400 border-lime-600'          // 짝수: 연두공
}
```

**시각적 예시**:
- 홀수 (1,3,5...): 🟡 노랑공
- 짝수 (2,4,6...): 🟢 연두공

---

#### A-2-3. 기초분석 - 소수/합성수

```typescript
// 소수 판단
const isPrime = (num: number) => {
  if (num === 1) return false
  if (num === 2) return true
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) return false
  }
  return true
}

// 컨텍스트 컬러
const getContextColor = (num: number) => {
  if (num === 1) {
    return 'bg-gray-400 border-gray-600'        // 1: 회색공
  }
  if (isPrime(num)) {
    return 'bg-yellow-400 border-yellow-600'    // 소수: 노랑공
  }
  return 'bg-lime-400 border-lime-600'          // 합성수: 연두공
}
```

**시각적 예시**:
- 1: ⚪ 회색공
- 소수 (2,3,5,7,11...): 🟡 노랑공
- 합성수 (4,6,8,9...): 🟢 연두공

---

#### A-2-4. 기초분석 - 저고

```typescript
// 저고 판단
const isLow = (num: number) => num <= 22

// 컨텍스트 컬러
const getContextColor = (num: number) => {
  if (isLow(num)) {
    return 'bg-blue-400 border-blue-600'        // 저(1-22): 파랑공
  }
  return 'bg-red-400 border-red-600'            // 고(23-45): 빨강공
}
```

**시각적 예시**:
- 저 (1-22): 🔵 파랑공
- 고 (23-45): 🔴 빨강공

---

#### A-2-5. 기초분석 - 총합/끝수합/AC값 (기본 범위 색상)

```typescript
// 범위별 기본 색상
const getRangeColor = (num: number) => {
  if (num <= 10) return 'bg-yellow-400 border-yellow-600'
  if (num <= 20) return 'bg-blue-400 border-blue-600'
  if (num <= 30) return 'bg-red-400 border-red-600'
  if (num <= 40) return 'bg-gray-400 border-gray-600'
  return 'bg-emerald-400 border-emerald-600'
}
```

**시각적 예시**:
- 1-10: 🟡 노랑공
- 11-20: 🔵 파랑공
- 21-30: 🔴 빨강공
- 31-40: ⚪ 회색공
- 41-45: 🟢 녹색공

---

### A-3. 공통 컴포넌트 설계

```typescript
// src/components/lotto/LottoBall.tsx
interface LottoBallProps {
  number: number
  variant: 'cell' | 'ball'
  contextType?: 'range' | 'odd-even' | 'prime' | 'low-high' | 'carryover'
  contextData?: any
  size?: 'xs' | 'sm' | 'md' | 'lg'
}

export function LottoBall({ number, variant, contextType, contextData, size }: LottoBallProps) {
  // variant에 따라 셀 or 공 형태 선택
  // contextType에 따라 색상 결정
  
  if (variant === 'cell') {
    return <CellStyleNumber number={number} size={size} />
  }
  
  return <BallStyleNumber number={number} contextType={contextType} contextData={contextData} size={size} />
}
```

---

### A-4. 사용 예시

#### 당첨번호 관리 페이지
```tsx
<LottoBall number={7} variant="cell" />
// → 연한 노랑 배경 + 진한 노랑 텍스트 셀
```

#### 홀짝 분석 페이지
```tsx
<LottoBall number={7} variant="ball" contextType="odd-even" />
// → 노랑 원형 공 (홀수)
```

#### 이월수 분석 페이지
```tsx
<LottoBall 
  number={7} 
  variant="ball" 
  contextType="carryover"
  contextData={{ prevNumbers: [3, 7, 12, 25, 33, 41] }}
/>
// → 빨강 원형 공 (이월수)
```

---

## 부록 B: 컬러 팔레트 정리

### B-1. 셀 형태 (연한 배경 + 진한 텍스트)
```css
1-10:   bg-yellow-100  text-yellow-700  (#FEF3C7 / #D97706)
11-20:  bg-blue-100    text-blue-700    (#DBEAFE / #1E40AF)
21-30:  bg-red-100     text-red-700     (#FEE2E2 / #DC2626)
31-40:  bg-gray-200    text-gray-700    (#F3F4F6 / #4B5563)
41-45:  bg-emerald-100 text-emerald-700 (#D1FAE5 / #059669)
보너스: bg-white       text-orange-500  (#FFFFFF / #F97316)
```

### B-2. 원형 공 (진한 배경 + 흰색 텍스트)
```css
노랑: bg-yellow-400  border-yellow-600  (#FACC15 / #CA8A04)
파랑: bg-blue-400    border-blue-600    (#60A5FA / #2563EB)
빨강: bg-red-400     border-red-600     (#F87171 / #DC2626)
회색: bg-gray-400    border-gray-600    (#9CA3AF / #4B5563)
녹색: bg-emerald-400 border-emerald-600 (#34D399 / #059669)
연두: bg-lime-400    border-lime-600    (#A3E635 / #65A30D)
주황: bg-orange-400  border-orange-600  (#FB923C / #EA580C)
보라: bg-purple-400  border-purple-600  (#C084FC / #9333EA)
```

---

## 부록 C: 기초분석 17종 컨텍스트 컬러 전체 목록

| 분석 | 컨텍스트 컬러 | 설명 |
|------|--------------|------|
| 1. 총합 | 범위별 기본 색상 | 노/파/빨/회/녹 |
| 2. 끝수합 | 범위별 기본 색상 | 노/파/빨/회/녹 |
| 3. AC값 | 범위별 기본 색상 | 노/파/빨/회/녹 |
| 4. 홀짝 | 홀수=노랑, 짝수=연두 | 🟡🟢 |
| 5. 저고 | 저=파랑, 고=빨강 | 🔵🔴 |
| 6. 소수/합성수 | 1=회색, 소수=노랑, 합성수=연두 | ⚪🟡🟢 |
| 7. 제곱수 | 제곱수=주황, 그 외=회색 | 🟠⚪ |
| 8. 배수 | 3배수=노랑, 5배수=파랑, 그 외=회색 | 🟡🔵⚪ |
| 9. 번호대별 | 범위별 기본 색상 | 노/파/빨/회/녹 |
| 10. 연번 | 연번=녹색, 그 외=회색 | 🟢⚪ |
| 11. 이월수 | 이월수=빨강, 그 외=회색 | 🔴⚪ |
| 12. 끝수 | 끝수별 색상 (10가지) | 🌈 |
| 13. 동형수 | 동형수=보라, 그 외=회색 | 🟣⚪ |
| 14. 핫콜드 | 핫=빨강, 콜드=파랑, 그 외=회색 | 🔴🔵⚪ |
| 15. 미출현 그룹 | 그룹별 색상 | 🌈 |
| 16. 회귀분석 | 예측값 근접도별 색상 | 🌈 |
| 17. 커스텀 | 셀 형태 | 연한 배경 + 진한 텍스트 |

---

## 11. 개발 일정 (8주 + PoC)

### 11.1 수정된 주차별 계획 (v1.1.1 개선)

| 주차 | 작업 내용 | 산출물 | 시간 | 변경 사항 |
|------|----------|--------|------|----------|
| **Week 1** | **프로젝트 초기 설정 + PoC** | | 40h | ⭐ PoC 추가 |
| | - Next.js 14 + TypeScript 프로젝트 생성 | ✅ | 4h | 동일 |
| | - Tailwind CSS + shadcn/ui 설정 | ✅ | 4h | 동일 |
| | - SQLite + Prisma 설정 (WAL 모드) | ✅ | 8h | +2h (WAL) |
| | - Write Queue 시스템 기초 구축 | ✅ | 4h | 신규 |
| | - PoC: 이미지 생성 테스트 | ✅ | 6h | 신규 |
| | - 로또볼 컴포넌트 (LottoBall, LottoBallGroup) | ✅ | 8h | -4h |
| | - 번호선택기 (NumberPicker) | ✅ | 6h | -2h |
| **Week 2** | **당첨번호 + Write Queue** | | 40h | ⭐ Queue 강화 |
| | - 당첨번호 CRUD API | ✅ | 8h | -4h |
| | - Write Queue 완성 | ✅ | 6h | 신규 |
| | - DB 헬스 체크 시스템 | ✅ | 4h | 신규 |
| | - 당첨번호 목록 화면 | ✅ | 6h | -2h |
| | - 수동 추가 폼 | ✅ | 4h | -2h |
| | - 크롤러 (Rate Limiting + Debounce) | ✅ | 8h | +4h |
| | - 대시보드 (통계 카드, 최근 당첨번호) | ✅ | 4h | 동일 |
| **Week 3** | **기초분석 (전반부 8종) ⭐⭐⭐** | | 40h | ⭐ 대폭 강화 |
| | - **통계 계산 유틸리티 (statistics.ts)** | ✅ | 4h | 신규 |
| | - **DrawRangeSlider 컴포넌트** | ✅ | 6h | 신규 |
| | - **StatisticsCard 컴포넌트** | ✅ | 4h | 신규 |
| | - **AllDrawsDialog 컴포넌트** | ✅ | 6h | 신규 |
| | - 분석 공통 컴포넌트 (Lazy Loading) | ✅ | 4h | 동일 |
| | - 총합 분석 (range + statistics) | ✅ | 4h | +1h |
| | - 끝수합 분석 | ✅ | 3h | 동일 |
| | - AC값 분석 | ✅ | 3h | 동일 |
| | - 홀짝 분석 (비율 통계 추가) | ✅ | 4h | +1h |
| | - 저고 분석 | ✅ | 2h | -2h |
| **Week 4** | **기초분석 (후반부 9종) ⭐** | | 40h | ⭐ 강화 |
| | - 소수/합성수 분석 (비율 통계) | ✅ | 4h | 동일 |
| | - 제곱수 분석 (통계) | ✅ | 3h | -1h |
| | - 배수 분석 (통계) | ✅ | 3h | -1h |
| | - 번호대별 분석 (통계) | ✅ | 4h | 동일 |
| | - 연번 분석 (통계) | ✅ | 4h | 동일 |
| | - 이월수 분석 (통계) | ✅ | 4h | 동일 |
| | - 끝수 분석 (통계) | ✅ | 4h | 동일 |
| | - 동형수 분석 (통계) | ✅ | 4h | 동일 |
| | - 핫콜드 분석 (통계) | ✅ | 4h | 동일 |
| | - 미출현 그룹 분석 (통계) | ✅ | 3h | -1h |
| | - 회귀분석 (회귀선 + R²) | ✅ | 3h | -3h |
| **Week 5** | **필터 + 조합 (Web Worker)** | | 40h | 동일 |
| | - 필터 스토어 (Zustand) | ✅ | 4h | 동일 |
| | - 필터 패널 UI (17종) | ✅ | 12h | 동일 |
| | - Filter Worker (청크 처리) | ✅ | 12h | +4h |
| | - 실시간 카운팅 | ✅ | 4h | 동일 |
| | - 조합 목록 화면 | ✅ | 4h | 동일 |
| | - 조합 저장 (Write Queue) | ✅ | 4h | +2h |
| **Week 6** | **커스텀분석 + AI (경량화)** | | 40h | 동일 |
| | - 커스텀분석 CRUD | ✅ | 8h | 동일 |
| | - 직접 입력형 | ✅ | 6h | 동일 |
| | - 그룹형 | ✅ | 6h | 동일 |
| | - 수식형 (간단한 파서) | ✅ | 8h | 동일 |
| | - AI 고정수/제외수 설정 | ✅ | 4h | 동일 |
| | - AI Worker (LSTM 추론만) | ✅ | 8h | +2h |
| **Week 7** | **출력 + 검증 (Canvas API)** | | 40h | 동일 |
| | - Canvas API PDF 생성 | ✅ | 12h | +4h |
| | - Canvas API PNG 생성 | ✅ | 8h | +2h |
| | - 검증 로직 (등수 계산) | ✅ | 8h | 동일 |
| | - 검증 결과 시각화 | ✅ | 8h | 동일 |
| | - AI 조합 생성 | ✅ | 4h | 동일 |
| **Week 8** | **마무리 + 안정성 테스트** | | 40h | 동일 |
| | - 브라우저 메모리 부하 테스트 | ✅ | 6h | 신규 |
| | - SQLite 동시 쓰기 테스트 | ✅ | 6h | 신규 |
| | - 크롤링 안전성 테스트 | ✅ | 4h | 신규 |
| | - 버그 수정 | ✅ | 10h | -6h |
| | - 성능 최적화 | ✅ | 6h | -2h |
| | - 데이터 시드 (최근 100회차) | ✅ | 2h | -2h |
| | - 트러블슈팅 가이드 작성 | ✅ | 4h | 신규 |
| | - 최종 테스트 | ✅ | 2h | -2h |

**총 개발 시간**: 320시간 (동일, 구조 개선)

### 11.2 마일스톤

```
Week 1: ✅ 프로젝트 기반 + PoC 완성
Week 2: ✅ 당첨번호 + Write Queue 완성
Week 3: ✅⭐ 기초분석 전반부 + 신규 컴포넌트 3개 완성
Week 4: ✅⭐ 기초분석 17종 전체 완성 (통계 + 범위 + 팝업)
Week 5: ✅ 필터 + 조합 + Web Worker 완성
Week 6: ✅ 커스텀 + AI Worker 완성
Week 7: ✅ 출력 (Canvas API) + 검증 완성
Week 8: 🎉 프로젝트 완료 + 안정성 검증
```

---

## 15. 부록

### 15.1 버전 히스토리

```markdown
# 버전 히스토리

## v1.1.1 (2024-12-25) - 기초분석 강화 ⭐⭐⭐
- ✅ 회차 범위 슬라이더 추가 (10~전체)
- ✅ 주요 통계 카드 추가 (6가지 통계)
- ✅ 전체 회차 팝업 구현 (검색, CSV 내보내기)
- ✅ 비율 통계 추가 (홀짝, 저고 등)
- ✅ statistics.ts 유틸리티 추가
- ✅ DrawRangeSlider 컴포넌트
- ✅ StatisticsCard 컴포넌트
- ✅ AllDrawsDialog 컴포넌트
- ✅ API에 range 파라미터 추가

## v1.1.0 (2024-12-25) - 안정성 강화
- ✅ Web Worker로 AI 격리
- ✅ Write Queue 시스템
- ✅ SQLite WAL 모드
- ✅ Rate Limiting (크롤링)
- ✅ Lazy Loading (차트)
- ✅ Canvas API (이미지)
- ✅ PoC 테스트 추가
- ✅ 유연한 레이아웃 (1280-1920px)

## v1.0.0 (2024-12-24) - 초기 버전
- ✅ 기초분석 17종
- ✅ 커스텀분석 3종
- ✅ AI 분석 (LSTM)
- ✅ 필터 + 조합
- ✅ PDF/PNG 출력
- ✅ 검증
```

### 15.2 자주 묻는 질문 (FAQ)

**Q1. 회차 범위를 선택하면 어떻게 되나요?**
```
- 선택한 범위만큼 최근 회차 데이터를 기준으로 분석합니다
- 차트와 통계가 즉시 업데이트됩니다
- 예: 최근 100회 선택 → 1095~996회 데이터만 분석
```

**Q2. 전체 회차 팝업에서 CSV 내보내기는?**
```
- 현재 표시된 데이터를 CSV 파일로 저장합니다
- 검색 필터가 적용된 상태로 내보냅니다
- 엑셀에서 바로 열 수 있습니다
```

**Q3. 통계 카드의 표준편차는 무엇인가요?**
```
- 데이터가 평균에서 얼마나 퍼져있는지를 나타냅니다
- ±1 표준편차 범위에 약 68%의 데이터가 포함됩니다
- 예: 평균 132.5 ± 18.2 → 114.3~150.7 범위
```

**Q4. 비율 통계의 최빈값은?**
```
- 가장 많이 나온 비율을 의미합니다
- 예: 홀짝 3:3이 180회로 가장 많음 (60%)
```

**Q5. API에서 range 파라미터는 필수인가요?**
```
- 선택사항입니다
- 미입력 시 기본값 100이 사용됩니다
- GET /api/analysis/sum?range=500
```

### 15.3 참고 자료

```markdown
# 개발 리소스

## 공식 문서
- Next.js: https://nextjs.org/docs
- Prisma: https://www.prisma.io/docs
- Recharts: https://recharts.org
- shadcn/ui: https://ui.shadcn.com

## 새로 추가된 컴포넌트
- DrawRangeSlider: 회차 범위 선택
- StatisticsCard: 통계 표시
- AllDrawsDialog: 전체 회차 팝업

## 새로 추가된 유틸리티
- calculateStatistics: 통계 계산
- calculateRatioStatistics: 비율 통계 계산
```

---

## 🎉 v1.1.1 완료!

**로또 AI 플랫폼 v1.1.1 완전 기술명세서**가 완성되었습니다!

### 📦 v1.1.1 주요 변경사항

**신규 컴포넌트 (3개)**:
1. ✅ DrawRangeSlider - 회차 범위 슬라이더
2. ✅ StatisticsCard - 주요 통계 카드
3. ✅ AllDrawsDialog - 전체 회차 팝업

**신규 유틸리티 (1개)**:
4. ✅ statistics.ts - 통계 계산 함수

**API 개선**:
5. ✅ range 파라미터 추가
6. ✅ statistics 응답 필드 추가
7. ✅ 비율 통계 계산 추가

**UI/UX 개선**:
8. ✅ 모든 기초분석 17종에 적용
9. ✅ 검색, 페이지네이션, CSV 내보내기
10. ✅ 컨텍스트 컬러 지원

---

**문서 버전**: 1.1.1 (Complete)  
**최종 업데이트**: 2024년 12월 25일  
**상태**: ✅ 기초분석 개선 완료