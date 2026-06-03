# e.p.t.q. 유지보수 수정 가이드

이 문서는 Next.js, React, TypeScript에 익숙하지 않은 운영자가 클라이언트 요청을 반영할 때 참고하는 실무용 가이드입니다. HTML과 CSS를 조금 다룰 수 있다는 전제로, 자주 바꾸는 문구, 이미지, 메뉴, SEO, 문의 메일, 배포 방법을 최대한 구체적으로 정리했습니다.

## 먼저 알아둘 것

이 프로젝트는 일반 HTML 파일을 직접 수정하는 사이트가 아닙니다. `src/` 안의 원본 파일을 수정한 뒤 `npm run build`를 실행하면 정적 export 결과가 `out/` 폴더에 만들어집니다. FTP 업로드 전에는 `out/` 내용을 `cafe24/` 폴더로 복사해 최종 업로드본을 정리합니다.

운영 서버에 올릴 때는 `src/`를 올리는 것이 아니라 `cafe24/` 폴더 안의 파일과 폴더 전체를 카페24 웹 루트에 업로드합니다.

작업 순서는 항상 아래처럼 진행하세요.

1. 수정할 원본 파일을 찾습니다.
2. `src/` 또는 `public/` 안의 원본만 수정합니다.
3. 로컬에서 화면을 확인합니다.
4. `npm run lint`를 실행해 코드 오류를 확인합니다.
5. `npm run build`를 실행해 `out/` 배포 파일을 다시 만듭니다.
6. 기존 `cafe24/`를 지우고 `out/`을 `cafe24/`로 복사합니다.
7. `cafe24/` 폴더 안의 전체 내용을 FTP로 업로드합니다.

## 폴더 구조

자주 만지는 폴더만 기억하면 됩니다.

```text
src/app/
  실제 페이지 파일이 들어 있습니다.
  예: 홈, 컬렉션 상세, 정책 페이지, Contact Us

src/components/
  여러 페이지에서 같이 쓰는 화면 조각이 들어 있습니다.
  예: 헤더, 푸터, 홈 섹션, 제품 정보, 문의 폼

src/lib/
  SEO 정보, 정책 페이지 본문처럼 데이터 성격의 파일이 들어 있습니다.

public/
  이미지, SVG, 영상, PHP 메일 파일, .htaccess가 들어 있습니다.

out/
  npm run build 결과물입니다. 직접 수정하지 않습니다.

cafe24/
  카페24 FTP 업로드용 최종 결과물입니다. 직접 수정하지 않습니다.
```

`out/`과 `cafe24/`는 빌드 결과물이므로 수정해도 다음 빌드 때 덮어써집니다. 반드시 `src/` 또는 `public/`을 수정하세요.

## 실행 방법

처음 프로젝트를 받은 PC에서는 아래 명령을 한 번 실행합니다.

```bash
npm install
```

수정하면서 브라우저로 확인하려면 아래 명령을 실행합니다.

```bash
npm run dev
```

터미널에 보통 `http://localhost:3000` 주소가 표시됩니다. 브라우저에서 이 주소로 접속해 확인합니다.

작업 완료 전에는 반드시 아래 두 명령을 실행합니다.

```bash
npm run lint
npm run build
```

`npm run build`가 성공하면 `out/` 폴더가 갱신됩니다. FTP 업로드 전에는 아래처럼 `cafe24/`를 최신 `out/` 기준으로 다시 만듭니다.

```bash
rm -rf cafe24
cp -R out cafe24
```

## 언어 수정 규칙

이 사이트는 영어 `ENG`와 중국어 `CHN`을 지원합니다. 문구를 바꿀 때는 거의 항상 두 언어를 같이 수정해야 합니다.

대부분의 문구는 아래처럼 생겼습니다.

```tsx
{
  ENG: "English text",
  CHN: "中文文本",
}
```

영어만 바꾸고 중국어를 그대로 두면 언어 전환 시 내용이 맞지 않습니다. 클라이언트가 영어만 요청해도 중국어 문구를 함께 확인하거나 임시 번역이라도 넣어두세요.

## 문구 수정 빠른 찾기

VS Code에서 `Command + Shift + F` 또는 `Ctrl + Shift + F`로 전체 검색을 열고, 클라이언트가 바꾸고 싶어 하는 문구 일부를 검색하세요.

예를 들어 홈 히어로 문구를 바꾸려면 현재 문구 일부인 `A premium Hyaluronic Acid filler`를 검색합니다. 검색 결과가 `src/app/page.tsx`에 나오면 그 파일을 수정합니다.

문구를 찾기 어려울 때는 아래 표를 먼저 확인하세요.

| 수정 대상 | 파일 |
| --- | --- |
| 홈 히어로 상단 문구 | `src/app/page.tsx` |
| 홈 히어로 CTA | `src/app/page.tsx` |
| 헤더/푸터 메뉴명 | `src/components/site-navigation.ts` |
| 푸터 정책 링크명 | `src/components/site-navigation.ts` |
| 컬렉션 상세 제품 설명 | `src/app/collections/*/page.tsx` |
| Contact Us 폼 선택지 | `src/components/contact-us-form.tsx` |
| 정책 페이지 본문 | `src/lib/policy-content.ts` |
| SEO title/description | `src/lib/seo.ts` |

## 이미지 수정 규칙

이미지는 대부분 `public/` 폴더에 있습니다. 코드에서는 `/home-hero.webp`처럼 맨 앞에 `/`를 붙여 사용합니다. 이 경로는 실제로 `public/home-hero.webp` 파일을 뜻합니다.

이미지를 교체하는 가장 쉬운 방법은 기존 파일명 그대로 새 이미지를 덮어쓰는 것입니다.

예를 들어 홈 PC 히어로 이미지를 바꾸려면 새 이미지를 `public/home-hero.webp` 이름으로 저장합니다. 모바일용은 `public/home-hero-mobile.webp`입니다.

파일명을 바꿔야 하는 경우에는 코드의 `src` 값도 같이 바꿔야 합니다.

```tsx
<Image
  src="/home-hero.webp"
  alt=""
  fill
  sizes="100vw"
/>
```

위 예시에서 파일명을 `home-hero-new.webp`로 바꿨다면 아래처럼 수정합니다.

```tsx
<Image
  src="/home-hero-new.webp"
  alt=""
  fill
  sizes="100vw"
/>
```

이미지 교체 시 주의할 점입니다.

- PC 이미지와 모바일 이미지가 따로 있는 경우 둘 다 교체합니다.
- `.webp`, `.png`, `.svg`, `.webm` 같은 확장자를 코드와 실제 파일에서 똑같이 맞춥니다.
- 대소문자도 정확히 맞춥니다. `Home.webp`와 `home.webp`는 서버에서 다른 파일로 취급될 수 있습니다.
- 큰 배경 이미지는 가급적 기존 이미지와 같은 비율로 준비합니다.
- `alt`가 비어 있는 이미지는 장식용 이미지입니다. 제품명이나 논문 이미지처럼 의미가 있는 이미지는 `alt` 문구를 같이 관리하는 것이 좋습니다.

## 주요 이미지 위치

| 화면 | PC 파일 | 모바일 파일 |
| --- | --- | --- |
| 홈 히어로 | `public/home-hero.webp` | `public/home-hero-mobile.webp` |
| Our Philosophy 상단 | `public/our-philosophy-visual.webp` | `public/our-philosophy-visual-mobile.webp` |
| The 9 Essentials 상단 | `public/the-9-essentials-visual.webp` | `public/the-9-essentials-visual-mobile.webp` |
| S100 상단 | `public/collections-s100-visual.webp` | `public/collections-s100-visual-mobile.webp` |
| S300 상단 | `public/collections-s300-visual.webp` | `public/collections-s300-visual-mobile.webp` |
| S500 상단 | `public/collections-s500-visual.webp` | `public/collections-s500-visual-mobile.webp` |
| Eve X 상단 | `public/collections-eve-x-visual.webp` | `public/collections-eve-x-visual-mobile.webp` |
| Academy 상단 | `public/clinical-evidence-visual.webp` | `public/clinical-evidence-visual-mobile.webp` |
| Global Presence 상단 | `public/global-presence-visual.webp` | `public/global-presence-visual-mobile.webp` |
| Contact Us 상단 | `public/contact-us-visual.webp` | `public/contact-us-visual-mobile.webp` |

## 홈 페이지 수정

현재 홈 페이지는 인트로 오버레이와 히어로 섹션만 렌더링합니다.

### 홈 히어로 문구

파일: `src/app/page.tsx`

아래 값을 수정합니다.

```tsx
const heroPrimaryCopy = {
  ENG: "A premium Hyaluronic Acid filler crafted for natural elegance...",
  CHN: "一款为自然优雅而生的高端透明质酸填充剂...",
};
```

모바일에서는 특정 단어 뒤에서 줄바꿈이 들어갑니다.

```tsx
const mobilePrimaryLineBreakIndexes = new Set([4, 8, 14]);
```

문구 길이가 크게 바뀌면 모바일에서 줄바꿈 위치가 어색할 수 있습니다. 이 숫자는 단어 순서 기준입니다. 예를 들어 `[4, 8, 14]`는 5번째, 9번째, 15번째 단어 뒤에서 줄바꿈한다는 뜻입니다. 잘 모르겠다면 문구만 바꾼 뒤 모바일 화면을 확인하고, 줄이 너무 길거나 짧을 때만 조정하세요.

### 홈 히어로 하단 외부 링크

파일: `src/app/page.tsx`

링크 주소는 아래 `href`를 수정합니다.

```tsx
<a
  href="https://welcometojam.com/publications/clinicalPapers.html"
  target="_blank"
  rel="noopener noreferrer"
>
```

화면에 보이는 문구는 아래 값을 수정합니다.

```tsx
const heroBottomLinkCopy = {
  ENG: "Jetema Academic Meeting – JAM, Jetema Academic Meeting",
  CHN: "Jetema 学术会议 - JAM, Jetema 学术会议",
};
```

## 메뉴 수정

파일: `src/components/site-navigation.ts`

헤더와 푸터 메뉴는 같은 데이터를 사용합니다. 메뉴명을 바꾸면 헤더와 푸터에 같이 반영됩니다.

```tsx
{
  href: "/collections/s100",
  label: { ENG: "COLLECTIONS", CHN: "产品系列" },
  children: [
    { href: "/collections/s100", label: { ENG: "S100", CHN: "S100" } },
  ],
}
```

`label`은 화면에 보이는 이름이고, `href`는 이동할 주소입니다.

메뉴 순서를 바꾸고 싶으면 배열 안의 블록 순서를 바꿉니다. 단, 모바일 메뉴도 같은 데이터를 쓰므로 PC와 모바일을 모두 확인해야 합니다.

새 페이지를 메뉴에 추가할 때는 메뉴만 추가하면 끝나지 않습니다. 최소한 아래 항목을 함께 확인하세요.

- `src/app/` 아래에 실제 페이지 파일이 있는지
- `src/lib/seo.ts`에 SEO 정보가 있는지
- `src/app/sitemap.ts`에 반영되는지
- PC 헤더, 모바일 GNB, 푸터에서 레이아웃이 깨지지 않는지

## 컬렉션 상세 페이지 수정

컬렉션 상세 페이지 파일은 아래 위치에 있습니다.

```text
src/app/collections/s100/page.tsx
src/app/collections/s300/page.tsx
src/app/collections/s500/page.tsx
src/app/collections/eve-x/page.tsx
```

### 상단 비주얼 이미지

각 상세 페이지의 `SubPageLayout` 안 `visual` 값을 수정합니다.

```tsx
visual={{
  src: "/collections-s100-visual.webp",
  mobileSrc: "/collections-s100-visual-mobile.webp",
  alt: "",
  width: 1920,
  height: 720,
  mobileWidth: 700,
  mobileHeight: 720,
}}
```

기존 파일명 그대로 이미지를 교체한다면 코드는 수정하지 않아도 됩니다.

### 제품 이미지와 설명

각 상세 페이지의 `CollectionProductInfo` 값을 수정합니다.

```tsx
<CollectionProductInfo
  title={currentLabel}
  accentColor={accentColor}
  image={{
    src: "/collections-s100-product.webp",
    mobileSrc: "/collections-s100-product-mobile.webp",
  }}
  composition={[
    {
      name: "e.p.t.q. S 100",
      detail: "1.0mL * 1 syringe / box",
    },
  ]}
  description={
    <Localized
      value={{
        ENG: "English description",
        CHN: "中文说明",
      }}
    />
  }
  tags={["HA 24MG/ML", "MID DERMIS", "27G / 30G"]} 
/>
```

`composition`은 제품 구성, `description`은 본문 설명, `tags`는 둥근 태그입니다.

`accentColor`는 제품 포인트 색상입니다. 색상 코드는 `#35d330` 같은 형식입니다. 디자인 전체에 영향이 있으므로 클라이언트가 명확히 요청한 경우에만 바꾸세요.

## 서브 페이지 상단 비주얼 수정

대부분의 서브 페이지는 `SubPageLayout`을 사용합니다. 페이지 파일에서 아래처럼 생긴 부분을 찾으세요.

```tsx
<SubPageLayout
  currentLabel={<Localized value={{ ENG: "Contact Us", CHN: "联系我们" }} />}
  parentLabel={<Localized value={{ ENG: "FOR PROVIDERS", CHN: "医疗机构" }} />}
  visualHeight="small"
  visual={{
    src: "/contact-us-visual.webp",
    mobileSrc: "/contact-us-visual-mobile.webp",
  }}
>
```

`currentLabel`은 현재 페이지명, `parentLabel`은 상위 메뉴명입니다. `visual.src`는 PC 이미지, `visual.mobileSrc`는 모바일 이미지입니다.

`visualHeight`는 상단 비주얼 높이입니다. 기존 값은 `small` 또는 `large`를 사용합니다. 이미지 교체만 하는 경우 이 값은 바꾸지 마세요.

## 정책 페이지 수정

파일: `src/lib/policy-content.ts`

아래 세 데이터가 정책 페이지 본문입니다.

```tsx
termsOfUseContent
cookiePolicyContent
privacyPolicyContent
```

공통 업데이트 날짜는 파일 상단의 `updatedAt`을 수정합니다.

```tsx
const updatedAt = {
  ENG: "Last updated: May 26, 2026",
  CHN: "最后更新：2026年5月26日",
};
```

본문 구조는 보통 아래 형식입니다.

```tsx
{
  title: { ENG: "Website information", CHN: "网站信息" },
  body: {
    ENG: ["첫 번째 문단", "두 번째 문단"],
    CHN: ["第一段", "第二段"],
  },
  items: {
    ENG: ["목록 1", "목록 2"],
    CHN: ["列表 1", "列表 2"],
  },
}
```

`items`는 없는 섹션도 있습니다. 목록이 필요 없으면 기존처럼 `body`만 사용하면 됩니다.

## Contact Us 수정

### 폼 선택지

파일: `src/components/contact-us-form.tsx`

국가, 전문 분야, 문의 주제 선택지는 아래 배열에서 관리합니다.

```tsx
const countryOptions = [
  { value: "kr", label: <Localized value={{ ENG: "Korea", CHN: "韩国" }} /> },
];
```

```tsx
const specialisationOptions = [
  {
    value: "dermatology",
    label: <Localized value={{ ENG: "Dermatology", CHN: "皮肤科" }} />,
  },
];
```

```tsx
const subjectOptions = [
  {
    value: "product",
    label: <Localized value={{ ENG: "Product inquiry", CHN: "产品咨询" }} />,
  },
];
```

`value`는 서버로 전송되는 값입니다. 영어 소문자와 하이픈만 사용하는 것이 안전합니다. `label`은 화면에 보이는 문구입니다.

### 개인정보 안내 문구

파일: `src/components/contact-us-form.tsx`

폼 아래 긴 안내 문구는 `We comply with applicable data protection laws...` 문장을 검색해 수정합니다. 영어와 중국어가 각각 React 조각으로 들어 있으므로 `<br />`은 줄바꿈이라고 생각하면 됩니다.

### 메일 수신자

파일: `public/contact-submit.php`

문의 메일 수신자는 PHP 파일 안에서 관리합니다. 아래 값을 검색해 새 수신자로 바꿉니다.

```php
$recipient = 'global@jetema.com';
```

발신자 주소와 발신자 이름은 아래 값입니다.

```php
$senderEmail = 'no-reply@eptqfiller.com';
$senderName = 'e.p.t.q. Website';
```

메일 관련 수정 후에는 반드시 실제 운영 서버에서 Contact Us 폼을 제출해 수신 테스트를 해야 합니다. 로컬 개발 서버에서는 PHP 메일 발송이 실제 카페24와 다르게 동작할 수 있습니다.

### 카페24 메일 발송 PHP 운영 설정

현재 `public/contact-submit.php`는 완성된 SMTP 인증 발송 코드라기보다, 카페24 웹호스팅에서 제공하는 PHP `mail()` 함수로 문의 내용을 보내는 구조입니다. 실제 서비스에서 안정적으로 쓰려면 PHP 코드만 보는 것이 아니라 카페24 웹메일 계정, DNS SPF, 발신자 주소, 수신 테스트를 함께 맞춰야 합니다.

현재 코드의 핵심 구조는 아래와 같습니다.

```php
$recipient = 'global@jetema.com';
$senderEmail = 'no-reply@eptqfiller.com';
$senderName = 'e.p.t.q. Website';
```

- `$recipient`: 문의 메일을 실제로 받을 주소입니다.
- `$senderEmail`: 수신자에게 보이는 발신 주소입니다. 동시에 `Return-Path`와 `mail()` envelope sender로도 사용됩니다.
- `$senderName`: 수신자에게 보이는 발신자 이름입니다.
- `Reply-To`: 문의자가 입력한 이름과 이메일로 설정됩니다. 운영자가 받은 메일에서 답장을 누르면 문의자에게 답장하도록 하기 위한 값입니다.

운영 적용 전에는 아래 순서대로 확인하세요.

1. 카페24에서 발신용 메일 계정을 만듭니다.
2. `public/contact-submit.php`의 `$senderEmail`을 실제 생성한 발신용 계정으로 맞춥니다.
3. 문의를 받을 주소를 `$recipient`에 넣습니다.
4. 도메인 DNS에 카페24 발송 서버를 허용하는 SPF TXT 레코드를 등록합니다.
5. `npm run build`를 실행합니다.
6. 기존 `cafe24/`를 지우고 `out/`을 `cafe24/`로 복사해 `cafe24/contact-submit.php`에 반영합니다.
7. `cafe24/` 폴더 전체를 FTP 업로드합니다.
8. 운영 도메인의 Contact Us에서 실제 문의를 제출해 수신함, 스팸함, Reply-To 동작을 확인합니다.

카페24 공식 안내에 따르면 웹호스팅에서는 POP3 메일 계정을 만든 뒤 `/home/bin/sendmail` 또는 PHP `mail()` 함수로 발송할 수 있습니다. 발신 주소는 실제 생성한 POP3 또는 웹메일 계정을 사용하는 것이 안전합니다.

발신용 계정 예시는 아래처럼 정합니다.

```text
no-reply@eptqfiller.com
contact@eptqfiller.com
global@eptqfiller.com
```

권장 방식은 `no-reply@eptqfiller.com` 같은 사이트 전용 발신 계정을 만들고, 실제 담당자 주소는 `$recipient`로만 받는 것입니다. 개인 Gmail 주소를 발신자로 쓰면 SPF가 맞지 않아 스팸 처리되거나 “보낸 사람 주소가 실제 발송 주소와 다를 수 있다”는 경고가 표시될 수 있습니다.

카페24가 안내하는 SPF TXT 값은 아래 형식입니다.

```text
"v=spf1 ip4:183.110.224.0/24 ip4:175.126.146.0/24 ip4:112.175.12.0/24 ip4:118.219.233.0/24 ~all"
```

일반적으로 DNS에서 아래처럼 등록합니다.

```text
타입: TXT
호스트/이름: @
값: "v=spf1 ip4:183.110.224.0/24 ip4:175.126.146.0/24 ip4:112.175.12.0/24 ip4:118.219.233.0/24 ~all"
```

이미 SPF 레코드가 있다면 같은 도메인에 SPF TXT를 여러 개 만들지 말고 기존 SPF에 카페24 값을 병합해야 합니다. SPF 레코드가 여러 개 있으면 수신 서버에서 SPF 오류로 볼 수 있습니다. DNS 권한이 카페24가 아니라 다른 업체에 있다면 해당 DNS 업체에서 TXT 레코드를 등록해야 합니다.

운영용으로 실제 값을 정했다면 PHP 파일은 예를 들어 아래처럼 맞춥니다.

```php
$recipient = 'manager@example.com';
$senderEmail = 'no-reply@eptqfiller.com';
$senderName = 'e.p.t.q. Website';
```

여러 명이 문의 메일을 받아야 한다면 가장 안전한 방식은 카페24 웹메일에서 대표 수신 주소나 메일 그룹을 만들고, PHP의 `$recipient`에는 그 대표 주소 하나만 넣는 것입니다. PHP에서 여러 주소를 직접 나열하는 방식은 수신 서버 정책이나 헤더 처리에 따라 문제가 생길 수 있으므로 권장하지 않습니다.

현재 Contact Us 폼은 파일 첨부를 보내지 않습니다. 카페24 웹메일은 계정별 발송량과 1회 발송 통수 제한이 있으므로, 문의가 대량으로 들어오는 캠페인이나 광고 랜딩으로 사용할 때는 발송 제한에 걸리지 않는지 카페24 관리자에서 발송량을 확인해야 합니다.

운영 테스트는 아래 기준으로 진행합니다.

1. Contact Us 페이지에서 모든 필수값을 입력하고 제출합니다.
2. 화면에 성공 상태가 표시되는지 확인합니다.
3. `$recipient` 수신함에 메일이 도착했는지 확인합니다.
4. 스팸함에도 들어갔는지 확인합니다.
5. 받은 메일에서 답장을 눌렀을 때 문의자가 입력한 이메일로 답장이 가는지 확인합니다.
6. Gmail, 네이버, Outlook 같은 외부 메일 주소로도 수신 테스트를 합니다.
7. 메일 원문 보기에서 SPF가 `pass` 또는 정상에 가까운 상태인지 확인합니다.

테스트에서 PHP는 성공 응답을 주는데 메일이 오지 않는 경우가 있습니다. `mail()`의 성공은 카페24 서버의 메일 큐 접수에 가깝고, 최종 수신함 도착을 보장하지 않습니다. 이 경우 아래를 확인하세요.

- `$senderEmail`이 실제 생성된 카페24 메일 계정인지
- `$senderEmail`의 도메인과 운영 도메인이 맞는지
- SPF TXT 레코드가 등록되어 있는지
- DNS 변경 후 충분한 전파 시간이 지났는지
- 수신함의 스팸함, 프로모션함, 차단 목록에 들어갔는지
- 카페24 웹메일 발송량 제한에 걸리지 않았는지
- 카페24 호스팅에서 PHP `mail()` 사용이 가능한 상품인지

더 높은 수신 안정성이 필요하면 PHP `mail()` 대신 SMTP 인증 발송으로 바꾸는 것을 권장합니다. 이 경우 카페24 웹메일 SMTP 서버, 포트, 계정, 비밀번호를 사용하고, PHPMailer 같은 라이브러리로 `contact-submit.php`를 다시 작성해야 합니다. 다만 SMTP 비밀번호를 PHP 파일에 직접 넣으면 FTP 접근자에게 노출될 수 있으므로, 가능하면 서버 환경변수나 카페24에서 제공하는 안전한 설정 방식을 사용하세요.

SMTP 방식으로 전환할 때는 운영자가 단순히 문구만 바꾸는 작업이 아니라 개발 작업으로 보고 아래 정보를 먼저 준비해야 합니다.

```text
SMTP 호스트:
SMTP 포트:
보안 방식: SSL 또는 TLS
SMTP 계정:
SMTP 비밀번호:
발신자 주소:
수신자 주소:
```

SMTP 전환 후에도 Contact Us 폼의 프론트엔드 전송 주소는 그대로입니다.

```tsx
const contactEndpoint = "/contact-submit.php";
```

즉, 브라우저는 계속 `/contact-submit.php`로 전송하고, 서버의 PHP 내부 발송 방식만 `mail()`에서 SMTP로 바뀌는 구조입니다.

참고한 카페24 공식 문서입니다.

- 카페24 웹호스팅 PHP `mail()`/sendmail 발송 안내: https://help.cafe24.com/faq/web-hosting/introduce/setup-management/send-mail-via-pop3-on-webhosting/
- 카페24 SPF TXT 등록 안내: https://help.cafe24.com/faq/business-tools/webmail/setup-management/email-warning-spf-txt/
- 카페24 웹메일 발송량/용량 제한 안내: https://help.cafe24.com/faq/business-tools/webmail/setup-management/webmail_send_limits_daily_per_send_size/

## 푸터 수정

파일: `src/components/footer.tsx`

푸터 메뉴명은 `src/components/site-navigation.ts`를 사용하므로 대부분 그 파일에서 수정합니다.

저작권 문구는 `footer.tsx`에서 아래 부분을 수정합니다.

```tsx
© JETEMA CO., LTD. ALL RIGHTS RESERVED.
```

소셜 링크는 현재 빈 문자열입니다.

```tsx
<Link href="" aria-label="X" className="inline-flex">
```

실제 주소가 생기면 `href=""` 안에 넣습니다.

```tsx
<Link href="https://x.com/example" aria-label="X" className="inline-flex">
```

외부 링크를 새 창으로 열고 싶으면 아래 속성을 추가합니다.

```tsx
target="_blank"
rel="noopener noreferrer"
```

## SEO 수정

파일: `src/lib/seo.ts`

검색 결과와 SNS 공유에 쓰이는 정보는 `seoRoutes` 배열에서 관리합니다.

```tsx
{
  path: "/collections/s100/",
  title: "S100",
  description: "e.p.t.q. S100 is designed for delicate refinement...",
  image: "/collections-s100-visual.webp",
  changeFrequency: "monthly",
  priority: 0.9,
}
```

수정 항목의 의미입니다.

- `path`: 페이지 주소입니다. 홈 `/` 외에는 끝에 `/`가 붙습니다.
- `title`: 브라우저 탭, 검색 결과 제목, SNS 공유 제목에 쓰입니다.
- `description`: 검색 결과 설명과 SNS 공유 설명에 쓰입니다.
- `image`: SNS 공유 이미지입니다.
- `changeFrequency`: sitemap에 들어가는 변경 빈도입니다.
- `priority`: sitemap 우선순위입니다. 보통 홈은 `1`, 주요 제품은 `0.9`, 정책 페이지는 `0.3` 정도입니다.

새 페이지를 추가하면 `seoRoutes`에 새 항목도 추가해야 sitemap에 반영됩니다.

운영 도메인은 같은 파일의 `fallbackSiteUrl`에서 관리합니다.

```tsx
const fallbackSiteUrl = "https://eptqfiller.com";
```

도메인이 바뀌는 경우 이 값을 수정하고, `src/app/robots.ts`의 sitemap 도메인도 확인하세요.

## 새 페이지 추가

새 페이지 추가는 HTML/CSS만 아는 운영자에게는 난도가 높습니다. 가능하면 기존 페이지를 복사해 구조를 유지하세요.

예를 들어 `/news` 페이지를 추가하려면 아래처럼 폴더와 파일을 만듭니다.

```text
src/app/news/page.tsx
```

가장 단순한 페이지 예시는 아래와 같습니다.

```tsx
import type { Metadata } from "next";

import { createPageMetadata } from "@/lib/seo";

export const metadata: Metadata = createPageMetadata("/news/");

export default function NewsPage() {
  return (
    <main className="sub-page-after-visual">
      <section className="sub-page-shell mx-auto py-[8rem]">
        <h1 className="text-[3rem]">News</h1>
        <p className="mt-[2rem]">News content goes here.</p>
      </section>
    </main>
  );
}
```

새 페이지를 만든 뒤에는 아래도 같이 수정합니다.

1. `src/lib/seo.ts`의 `seoRoutes`에 `/news/` 추가
2. 메뉴에 노출할 경우 `src/components/site-navigation.ts` 수정
3. `npm run lint` 실행
4. `npm run build` 실행
5. `/news/` 주소로 화면 확인

복잡한 상단 비주얼, 스크롤 애니메이션, 다국어 전환이 필요한 페이지는 기존 서브 페이지를 복사해 만드는 편이 안전합니다.

## 스타일 수정 기초

이 프로젝트는 Tailwind CSS 문법을 많이 사용합니다. HTML의 `class`처럼 `className` 안에 스타일이 들어갑니다.

```tsx
<p className="mt-[2rem] text-[1.111111rem] text-[#666]">
```

자주 보이는 표현입니다.

| 표현 | 뜻 |
| --- | --- |
| `mt-[2rem]` | 위쪽 여백 |
| `mb-[2rem]` | 아래쪽 여백 |
| `pt-[2rem]` | 위쪽 안쪽 여백 |
| `pb-[2rem]` | 아래쪽 안쪽 여백 |
| `w-[10rem]` | 너비 |
| `h-[10rem]` | 높이 |
| `text-[1rem]` | 글자 크기 |
| `text-[#666]` | 글자 색상 |
| `bg-white` | 배경 흰색 |
| `flex` | 가로/세로 배치용 flex |
| `grid` | 격자 배치 |
| `hidden` | 숨김 |
| `max-[1024px]:hidden` | 1024px 이하에서 숨김 |
| `max-[1024px]:block` | 1024px 이하에서 보임 |

PC와 모바일 스타일이 함께 있는 경우가 많습니다.

```tsx
className="text-[2rem] max-[1024px]:text-[1.5rem]"
```

위 코드는 기본 글자 크기는 `2rem`, 화면 너비가 1024px 이하일 때는 `1.5rem`이라는 뜻입니다.

사이트 전체 기본 글자 크기는 화면 크기에 따라 바뀝니다. 그래서 `1rem`은 항상 같은 픽셀이 아닐 수 있습니다. 기존 스타일과 비슷하게 수정하려면 주변 코드의 단위를 따라가세요.

## 수정하면 위험한 부분

아래 파일은 단순 문구 수정이 아니라면 신중하게 다뤄야 합니다.

| 파일 | 이유 |
| --- | --- |
| `src/components/header.tsx` | PC/모바일 메뉴, 스크롤 숨김, 언어 선택, 모바일 스크롤 잠금이 모두 연결되어 있습니다. |
| `src/components/smooth-scroll-provider.tsx` | 전체 Lenis 스크롤을 관리합니다. |
| `src/components/home-intro-overlay.tsx` | 홈 첫 진입 인트로 애니메이션을 관리합니다. |
| `src/components/eve-x-visual-height-effect.tsx` | Eve X 전용 스크롤 효과입니다. |
| `next.config.ts` | 정적 export와 이미지 설정이 배포에 직접 영향을 줍니다. |

이 파일들은 문구나 이미지 경로 정도만 수정하고, 동작 로직은 개발자에게 맡기는 것을 권장합니다.

## 빌드와 배포

배포 전 체크리스트입니다.

1. PC 화면 확인: `http://localhost:3000`
2. 모바일 화면 확인: 브라우저 개발자 도구에서 모바일 너비로 확인
3. 언어 전환 확인: ENG와 CHN 모두 확인
4. 메뉴 클릭 확인: PC 헤더, 모바일 메뉴, 푸터
5. Contact Us 폼이 보이는지 확인
6. `npm run lint` 성공
7. `npm run build` 성공
8. 기존 `cafe24/`를 지우고 `out/`을 `cafe24/`로 복사
9. `cafe24/` 폴더 안의 전체 내용을 FTP 업로드
10. 운영 도메인에서 주요 페이지 확인

업로드할 때는 `cafe24` 폴더 자체를 올리는 것이 아니라, `cafe24/` 안에 들어 있는 파일과 폴더 전체를 웹 루트에 올립니다.

예를 들어 FTP에서 웹 루트가 비어 있다면 아래처럼 보여야 합니다.

```text
웹 루트/
  _next/
  collections/
  for-providers/
  philosophy/
  academy/
  technology/
  index.html
  sitemap.xml
  robots.txt
  contact-submit.php
  .htaccess
```

## 자주 나는 오류

### 따옴표 오류

문구 안에 `'`가 들어가면 문제가 생길 수 있습니다.

```tsx
ENG: "e.p.t.q.'s global presence"
```

큰따옴표 안에 작은따옴표는 괜찮습니다. 반대로 작은따옴표로 감싼 문구 안에 작은따옴표를 넣으면 오류가 납니다.

```tsx
ENG: 'e.p.t.q.'s global presence'
```

위 코드는 잘못된 예시입니다.

### 쉼표 누락

배열이나 객체에서 항목 사이에는 쉼표가 필요합니다.

```tsx
{
  ENG: "English",
  CHN: "中文",
}
```

여러 항목이 이어질 때는 특히 쉼표를 확인하세요.

### 태그를 닫지 않음

`<br />`, `<Image />`, `<Localized />`처럼 혼자 닫히는 태그는 끝에 `/`가 있습니다.

```tsx
<br />
```

`<p>`처럼 내용이 있는 태그는 닫는 태그가 필요합니다.

```tsx
<p>Text</p>
```

### 이미지가 안 보임

아래를 확인하세요.

- 파일이 `public/` 안에 있는지
- 코드의 `src`와 실제 파일명이 같은지
- 확장자가 같은지
- 대소문자가 같은지
- 빌드 후 FTP에 새 파일이 올라갔는지

### 모바일에서만 깨짐

`max-[1024px]:...`, `max-[768px]:...`, `max-[480px]:...`가 모바일 스타일입니다. PC 스타일만 수정하면 모바일에는 반영되지 않거나 반대로 깨질 수 있습니다.

문구가 길어졌다면 모바일에서 줄바꿈이 어색해질 수 있습니다. 긴 단어, 긴 제품명, 긴 URL은 특히 확인하세요.

## 클라이언트 요청별 수정 위치 요약

| 요청 | 먼저 볼 파일 |
| --- | --- |
| 홈 첫 화면 문구 바꾸기 | `src/app/page.tsx` |
| 홈 배경 이미지 바꾸기 | `public/home-hero.webp`, `public/home-hero-mobile.webp` |
| 메뉴명 바꾸기 | `src/components/site-navigation.ts` |
| 메뉴 순서 바꾸기 | `src/components/site-navigation.ts` |
| 제품 상세 설명 바꾸기 | `src/app/collections/s100/page.tsx` 등 |
| 제품 상세 이미지 바꾸기 | `public/collections-...` 이미지 |
| 정책 문구 바꾸기 | `src/lib/policy-content.ts` |
| 검색 결과 제목/설명 바꾸기 | `src/lib/seo.ts` |
| 문의 폼 선택지 바꾸기 | `src/components/contact-us-form.tsx` |
| 문의 메일 수신자 바꾸기 | `public/contact-submit.php` |
| 푸터 저작권 바꾸기 | `src/components/footer.tsx` |
| 소셜 링크 바꾸기 | `src/components/footer.tsx` |

## 최종 확인 기준

수정이 끝났다고 판단하려면 최소한 아래가 모두 통과해야 합니다.

```bash
npm run lint
npm run build
```

그리고 브라우저에서 아래 페이지는 반드시 확인하세요.

```text
/
/collections/s100/
/collections/s300/
/collections/s500/
/collections/eve-x/
/for-providers/contact-us/
/privacy-policy/
```

이미지나 문구가 특정 페이지만 바뀐 작업이라도 홈, 메뉴, 모바일 메뉴, 푸터는 같이 확인하는 것이 좋습니다.
