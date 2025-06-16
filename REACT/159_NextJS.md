---
layout: post
title: NextJS
author: haeran
date: 2025-05-27 21:07:00 +0900 
categories: [JAVASCRIPT]
banner:
  image:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [NextJS]
---

## 1. **CSR (Client-Side Rendering)**

**💡 모든 것을 브라우저가 처리하는 방식**

### 렌더링 과정:

1. 사용자가 웹사이트에 접속하면 브라우저는 HTML 파일을 요청합니다.
2. 서버는 `<div id="__next"></div>` 만 있는 **빈 HTML 껍데기**를 반환합니다.
3. 브라우저는 JavaScript 번들 파일을 다운로드합니다.
4. 번들 안의 React 코드가 실행되어, 브라우저 내에서 컴포넌트를 렌더링합니다.
5. 최종적으로 사용자 화면이 구성됩니다.

### 특징:

* HTML이 처음엔 비어 있음 (SEO에 불리)
* 최초 로딩이 느릴 수 있음
* 이후 라우팅은 빠름 (SPA처럼 작동)

## 2. **SSR (Server-Side Rendering)**

**💡 HTML을 서버에서 즉석으로 렌더링**

### 렌더링 과정:

1. 사용자가 특정 페이지에 접속하면, 브라우저는 서버에 요청을 보냅니다.
2. 서버는 `getServerSideProps`를 실행하여 필요한 데이터를 **실시간으로 fetch**합니다.
3. 그 데이터를 기반으로 React 컴포넌트를 서버에서 실행합니다.
4. 렌더링된 **완성된 HTML**을 브라우저에 전달합니다.
5. 브라우저는 이 HTML을 곧바로 보여주고, 이후 React가 hydrate 됩니다.

### 특징

* HTML에 데이터가 들어 있어 **SEO에 매우 유리**
* 요청 시마다 서버에서 새 HTML을 만들어야 하므로 서버 부하 ↑
* 실시간 데이터에 적합

## 3. **SSG (Static Site Generation)**

**💡 HTML을 미리 만들어서 빠르게 제공**

### 렌더링 과정

1. 프로젝트를 `next build`할 때 `getStaticProps`가 실행됩니다.
2. 서버는 데이터를 fetch하여 HTML을 **빌드 타임에 미리 생성**합니다.
3. 이 HTML은 정적으로 저장되고, CDN에 배포됩니다.
4. 사용자가 페이지를 요청하면 즉시 정적 HTML을 전달합니다.
5. React가 hydrate 되며 인터랙션 기능을 추가합니다.

### 특징

* 렌더링 속도가 가장 빠름 (HTML을 즉시 전달)
* **변하지 않는 콘텐츠**에 적합 (ex: 문서, 블로그)
* 데이터가 자주 바뀌면 부적합

## 4. **ISR (Incremental Static Regeneration)**

**💡 정적 페이지를 일정 주기로 갱신**

### 렌더링 과정

1. 초기에는 SSG처럼 정적 HTML을 미리 생성합니다.
2. 사용자가 해당 페이지를 요청하면 기존 정적 HTML이 전달됩니다.
3. 설정된 `revalidate` 시간 후 **새 요청이 들어오면**, 서버는 백그라운드에서 최신 데이터를 받아 새 HTML을 생성합니다.
4. 이후 요청부터는 이 새 HTML을 사용합니다.

### 특징

* SSG의 속도 + SSR의 최신성
* **상품 상세, 뉴스 등 자주는 아니지만 갱신이 필요한 페이지**에 적합
* `revalidate`로 재생성 주기 조절 가능

## 한눈에 정리 🗂

| 렌더링 방식  | HTML 생성 시점   | 주 대상             | SEO | 특징                |
| ------- | ------------ | ---------------- | --- | ----------------- |
| **CSR** | 브라우저 실행 후    | 대화형 SPA          | ❌   | 초기 로딩 느림, 이후 빠름   |
| **SSR** | 요청 시 서버에서    | 최신 데이터           | ✅   | 실시간 응답, 서버 부하 ↑   |
| **SSG** | 빌드 시         | 정적 콘텐츠           | ✅   | 속도 빠름, 자주 바뀌면 부적합 |
| **ISR** | 빌드 + 주기적 재생성 | 자주 갱신되지만 실시간은 아님 | ✅   | 균형잡힌 성능과 최신성      |

1. **왜 CSR 방식은 SEO에 불리한가요?**
   → 초기 HTML이 비어 있기 때문에 검색 엔진이 콘텐츠를 제대로 인식하지 못할 수 있습니다. Googlebot이 JavaScript 실행을 기다려야 하는데, 그 과정에서 누락될 수 있어요.

2. **SSR과 SSG 중 어떤 걸 선택해야 할까요?**
   → 데이터의 변경 빈도와 SEO 중요도에 따라 결정합니다. 자주 바뀌지 않고 검색에 노출이 중요하면 SSG, 자주 바뀌고 개인화가 필요하면 SSR이 적합해요.

3. **SSR과 ISR은 어떤 상황에서 선택해야 하나요?**
   → 실시간 데이터가 필요한 경우 SSR, 비교적 느리게 갱신되어도 되는 경우 ISR이 적합합니다. 예: 상품 재고는 SSR, 블로그 게시물은 ISR

4. **ISR은 revalidate 시간보다 자주 바뀌는 데이터를 어떻게 처리하나요?**
   → 그런 경우는 SSR로 전환하거나, ISR과 CSR을 섞어 사용하는 하이브리드 방식도 고려해야 해요.

## Next의 장점

* **파일 기반 라우팅**: `app/` 디렉토리만 있으면 자동 라우팅
* **렌더링 전략 다양**: SSR, SSG, ISR, CSR 모두 지원
* **SEO 최적화**: 메타 태그, OG 태그 등 서버 렌더링이 가능
* **API Routes**: 백엔드 없이도 서버리스 함수 작성 가능
* **이미지 최적화**: `next/image` 컴포넌트로 성능 향상
* **타입스크립트 지원**: out-of-the-box로 TS 설정 가능
* **배포 최적화**: Vercel과 찰떡궁합

Next는 프론트엔드와 백엔드를 통합적으로 관리하면서도 성능 최적화까지 고려한 **현대적 웹 프레임워크**입니다.

## Next를 구성하는 기본 설정 파일

## 1. `next.config.js`

Next.js 전체 설정을 담당하는 파일입니다.
대표적으로 다음과 같은 설정을 합니다:

```js
module.exports = {
  reactStrictMode: true,
  images: {
    domains: ['example.com'],
  },
  rewrites: async () => [
    {
      source: '/about',
      destination: '/company/about',
    },
  ],
};
```

* **reactStrictMode**: 리액트 엄격 모드 활성화
* **images.domains**: 외부 이미지 도메인 설정
* **rewrites**: URL 재작성 (브라우저 주소는 유지, 내부 요청만 변경)

## 2. `tsconfig.json`

타입스크립트를 사용하는 경우 타입 설정을 정의합니다.

## 3. `.env.local`

환경변수를 정의하는 파일입니다. 예: API 키

```env
NEXT_PUBLIC_API_KEY=12345
```

`NEXT_PUBLIC_` 접두사를 붙이면 클라이언트에서도 접근 가능해요.
