<h1 align="center">Raykookie</h1>

<p align="center"><strong>Handmade confectionery, from the oven to your table: the menu and the order in the customer's pocket, and the right price for every sweet in the baker's.</strong></p>
<p align="center">Online store, Android apps (customer and management), web dashboard and a Java API with Pix and card payments: a complete system, in production.</p>

<p align="center">
  <img alt="Next.js 15" src="https://img.shields.io/badge/Next.js-15-000000?logo=nextdotjs&logoColor=white">
  <img alt="React 19" src="https://img.shields.io/badge/React-19-149ECA?logo=react&logoColor=white">
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white">
  <img alt="Tailwind CSS 4" src="https://img.shields.io/badge/Tailwind-4-06B6D4?logo=tailwindcss&logoColor=white">
  <img alt="Flutter" src="https://img.shields.io/badge/Flutter-02569B?logo=flutter&logoColor=white">
  <img alt="Android app version 1.0.0 build 17" src="https://img.shields.io/badge/app%20Android-1.0.0%20(17)-D42A76">
  <img alt="Status: in production" src="https://img.shields.io/badge/status-in%20production-2EA043">
</p>

<p align="center">
  <img src="docs/media/hero-loop.gif" width="860" alt="Raykookie website home screen: the pink cookie from the logo shakes, takes three bites with crumbs flying, and returns to the whole logo, in a continuous loop.">
</p>

> This repository is the project's **public showcase**: README, images and animations. The source code is private. The file and folder names mentioned below exist in the source repository.

---

## Overview

Raykookie is an artisanal confectionery that works **on a made-to-order basis**. This repository describes the entire system that runs it, in production, end to end: the store customers use to place orders, the dashboard the baker uses to run the business, and the API that connects the two, with payments via Pix and card.

| Piece | Technology | For whom | Folder |
|---|---|---|---|
| **Website and store** | Next.js 15, React 19, TypeScript | customer | `frontend/` |
| **Customer Android app** | Flutter | customer | `mobile-client/` |
| **"raykookie business" web dashboard** | Next.js (same project as the website) | baker | `frontend/app/(app)` |
| **Management Android app** | Flutter | baker | `mobile-server/` |
| **API** | Java 25, Spring Boot, PostgreSQL, Clean Architecture | both sides | `backend-java/` |
| **Shared app package** | Dart | both apps | `mobile-common/` |

```
 browser ──▶ Vercel (Next.js) ── /api/* ──┐
                                          ▼
 customer app ─┐                 Java API (Docker, always on) ──▶ PostgreSQL
 mgmt app     ─┴──────────────▶    ▲                │
                                   │                └─▶ Mercado Pago (Pix and card, signed webhook)
                                   └── scheduled tasks inside the server itself
```

Both Android apps are built, **signed** and published by the pipeline itself.

### Customer side

- **Website** (Next.js): the house's storefront. Works in any browser and is designed for people arriving from Instagram, using their thumb.
- **Android app** (Flutter): the same world on the phone, with the account always at hand and notifications when the order changes stage. The APK is downloaded straight from the website.

For anyone who wants a sweet, whether as a gift, to celebrate, or to satisfy the week's craving, the path is short:

| What the person does | How the product helps |
|---|---|
| **Choose** | Always up-to-date menu, with photos, categories and combos. The menu can be shared by link (and, in the app, by QR code). |
| **Order** | Order in 5 quick steps. Sign in with Google or with email and password, and what was already chosen isn't lost at login. |
| **Track** | Order timeline (received, in preparation, ready, delivered) and a phone notification on every change. |
| **Manage their own data** | "My data" screen with account export and deletion. |

### Baker side (management)

The dashboard comes in **two versions that talk to the same API**: the web dashboard (`raykookie business`, inside the website itself, with no public link) and the Android management app. The core idea is that the baker doesn't need a spreadsheet: she records what she buys and what she makes, and the system returns the **right price for every sweet**.

| Area | What it solves |
|---|---|
| **Inventory** | Ingredients with cost per gram, ml or unit calculated from the packaging, restock alerts, and **invoice (NFC-e) reading by QR code** to stock items in. |
| **Recipes and prices** | Each recipe adds up ingredients, labor and a share of the month's expenses and **suggests the price** with the chosen margin; it warns when the real margin falls below the minimum. A product can have several recipes (the default one sets the price). |
| **Menu** | Chooses what appears on the website and in the customer app (only photo, name, price and allergy badges go out; cost and margin never do), plus **combos** and **coupons**. |
| **Orders** | Orders from the website and the app arrive on their own; counter orders are entered by hand, with a Pix payment link. Production, delivery and payment status. |
| **Refunds** | Cancellation with automatic Pix refund, requests with photos, and the baker's decision. |
| **Goals and costs** | Monthly goals (revenue, orders, average ticket, new customers) against actuals; fixed and variable expenses that go into the price. |
| **In-person sales** | Static Pix screen per product that works **without internet and without login**, for selling at the fair. |
| **Security and audit trail** | Insert-only audit history (inventory, recipes, prices, coupons and orders), biometrics to open the app and push notifications for new orders. |

---

## Screen gallery

> All the screens below are real captures of the website and the apps running, with **fictional data** (sample customer, orders, inventory and prices). The customer section comes first; the management one follows right after, in [Management apps](#management-apps).

<p align="center">
  <img src="docs/media/mockup-apps.webp" width="880" alt="Website on a laptop and the app on two phones, showing the home screen and the menu.">
</p>

### Website (desktop)

<table>
  <tr>
    <td width="50%" align="center"><img src="docs/media/mockup-web-home.webp" alt="Website home screen: serif title, Fazer pedido (Place order) button and the pink cookie from the logo."><br><sub><b>Home</b>: the house in one sentence and an order button always in view.</sub></td>
    <td width="50%" align="center"><img src="docs/media/mockup-web-cardapio.webp" alt="Menu section of the website: categories pinned at the top and cards with photo, name, allergen badges, price and an order button."><br><sub><b>Menu</b>: photos, allergen badges and an order button on every item.</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="docs/media/mockup-web-novo-pedido.webp" alt="New order screen on the website, step 1 of 5, with the item list and the total pinned at the bottom."><br><sub><b>New order</b>: the total follows the person along the bottom.</sub></td>
    <td align="center"><img src="docs/media/mockup-web-pedidos.webp" alt="My orders screen on the website, with each order's timeline and payment status."><br><sub><b>My orders</b>: timeline and payment status.</sub></td>
  </tr>
  <tr>
    <td align="center" colspan="2"><img src="docs/media/mockup-web-entrar.webp" width="50%" alt="Website sign-in screen: Continue with Google button and email and password form."><br><sub><b>Sign in</b>: with Google or with email and password.</sub></td>
  </tr>
</table>

**The website in motion**

<table>
  <tr>
    <td align="center"><img src="docs/media/web-rolagem.gif" width="440" alt="Scrolling the website home page, from the top to the menu and back to the logo."><br><sub>From the introduction to the menu.</sub></td>
    <td align="center"><img src="docs/media/web-pedido.gif" width="440" alt="Clicking Order on a menu item and the new order screen opening."><br><sub>From the menu to the order, one click away.</sub></td>
  </tr>
</table>

### Android app

<p align="center">
  <img src="docs/media/mockup-app-telas.webp" width="880" alt="Four app screens on phones: home, menu, new order and my orders.">
</p>

<table>
  <tr>
    <td width="33%" align="center"><img src="docs/media/mockup-app-home.webp" alt="App home screen with the house title and the Place order button."><br><sub><b>Home</b></sub></td>
    <td width="33%" align="center"><img src="docs/media/mockup-app-itens.webp" alt="App menu with item cards, including the brand placeholder for an item without a photo."><br><sub><b>Menu</b>: with no photo, the brand symbol steps in.</sub></td>
    <td width="33%" align="center"><img src="docs/media/mockup-app-novo-pedido.webp" alt="New order in the app, step 1 of 5, with a cookie already added and the total at the bottom."><br><sub><b>New order</b> in 5 steps.</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="docs/media/mockup-app-pedidos.webp" alt="My orders in the app, with timeline and payment confirmation."><br><sub><b>My orders</b></sub></td>
    <td align="center"><img src="docs/media/mockup-app-entrar.webp" alt="App sign-in with Google or email and password."><br><sub><b>Sign in</b></sub></td>
    <td align="center"><img src="docs/media/mockup-app-cardapio.webp" alt="Start of the menu in the app, with the category filter and the featured combo."><br><sub><b>Categories</b> one tap away.</sub></td>
  </tr>
</table>

**The app in motion**

<table>
  <tr>
    <td align="center"><img src="docs/media/app-cardapio.gif" width="300" alt="Scrolling the app home screen down to the menu and back to the top."><br><sub>Browsing the menu.</sub></td>
    <td align="center"><img src="docs/media/app-pedido.gif" width="300" alt="Opening a new order in the app, choosing items and advancing to the Your details step."><br><sub>Building an order.</sub></td>
  </tr>
</table>

> The app captures come from the same Flutter code compiled for the web and opened in a phone-sized window (390×844), against a mock API. The device frame is drawn in HTML/CSS; the screen content is the app's own.

---

## Management apps

The baker's web dashboard and Android app, also in real captures with fictional data (an invented confectionery, with sample inventory, recipes, orders and goals). The prices and margins shown come from the **same production calculation** (`shared/pricing.ts`), not from hand-typed numbers.

<p align="center">
  <img src="docs/media/mockup-gestao-apps.webp" width="880" alt="The baker's web dashboard on a laptop and the management app on two phones, showing the month summary and the ingredient inventory.">
</p>

### Web dashboard

<table>
  <tr>
    <td width="50%" align="center"><img src="docs/media/mockup-gweb-inicio.webp" alt="Dashboard home: progress ring for the month's goal, revenue, profit, upcoming deliveries, items to restock and most profitable recipes."><br><sub><b>Home</b>: the month's goal, upcoming deliveries and what needs restocking.</sub></td>
    <td width="50%" align="center"><img src="docs/media/mockup-gweb-receitas.webp" alt="Recipes and prices: cards with cost per unit, suggested price, profit and margin for each product."><br><sub><b>Recipes and prices</b>: cost, price and margin of each product, calculated automatically.</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="docs/media/mockup-gweb-estoque.webp" alt="Ingredient inventory: table with quantity, packaging, cost per unit and status of each item."><br><sub><b>Inventory</b>: cost per unit and status of each ingredient.</sub></td>
    <td align="center"><img src="docs/media/mockup-gweb-pedidos.webp" alt="Orders: status filters, totals and the list of orders with delivery, amount, status and payment."><br><sub><b>Orders</b>: from intake to delivery, with payment alongside.</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="docs/media/mockup-gweb-metas.webp" alt="Sales goals: progress ring and four goals (revenue, orders, average ticket and new customers) against actuals."><br><sub><b>Goals</b>: actuals against target, with no retyping.</sub></td>
    <td align="center"><img src="docs/media/mockup-gweb-custos.webp" alt="Costs and adjustments: the month's fixed and variable expenses and the basis of the price calculation."><br><sub><b>Costs and adjustments</b>: the month's expenses go into each recipe's price.</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="docs/media/mockup-gweb-cardapio.webp" alt="Menu: product list with switches to appear on the website, show ingredients and allergy badges."><br><sub><b>Menu</b>: what the customer sees, with allergy badges.</sub></td>
    <td align="center"><img src="docs/media/mockup-gweb-entrar.webp" alt="Dashboard sign-in screen: pink panel with the headline Know how much to charge for each sweet and the username and password form."><br><sub><b>Sign in</b>: access by URL only, no link on the public page.</sub></td>
  </tr>
</table>

### Management Android app

<p align="center">
  <img src="docs/media/mockup-gapp-telas.webp" width="880" alt="Four management app screens on phones: home, inventory, recipes and orders.">
</p>

<table>
  <tr>
    <td width="33%" align="center"><img src="docs/media/mockup-gapp-inicio.webp" alt="Management app home with the month's goal and upcoming deliveries."><br><sub><b>Home</b></sub></td>
    <td width="33%" align="center"><img src="docs/media/mockup-gapp-receita.webp" alt="Recipe editor in the app: profit margin, menu price, weight and photo."><br><sub><b>Recipe editor</b>: margin, price and photo.</sub></td>
    <td width="33%" align="center"><img src="docs/media/mockup-gapp-mais.webp" alt="The app's More menu: menu, categories, suppliers, coupons, in-person sales and refunds."><br><sub><b>More</b>: menu, coupons, in-person sales and refunds.</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="docs/media/mockup-gapp-metas.webp" alt="Sales goals in the app: the month's progress ring and goal fields."><br><sub><b>Goals</b></sub></td>
    <td align="center"><img src="docs/media/mockup-gapp-pedidos.webp" alt="Orders in the app, with status filters and the list of orders."><br><sub><b>Orders</b></sub></td>
    <td align="center"><img src="docs/media/mockup-gapp-entrar.webp" alt="Management app sign-in with username and password."><br><sub><b>Sign in</b></sub></td>
  </tr>
</table>

> As with the customer app, the management app screens are the same Flutter code compiled for the web in a phone-sized window (390×844), against the mock API.

---

## Home screen animation

The pink cookie from the logo appears whole, **shakes, takes three bites** (with crumbs flying from each) and settles into the static logo. The GIF at the top is the real animation, recorded from the running website. There is also a video version: [`docs/media/hero-loop.mp4`](docs/media/hero-loop.mp4).

**How it was made** (`frontend/lib/logo-intro.ts` and `frontend/components/LogoIntro.tsx`):

- **anime.js v4, a single timeline**: entrance, shake, three bites and crumbs on the same clock.
- **Pure SVG, no image**: the geometry comes from the logo's own path (a disc minus three circles). During the scene, the bites are *mask* circles that grow from zero, so they are truly transparent on any background.
- **Crumbs with simple physics**: irregular polygons, each with impulse, a little hop, gravity, spin and fade.
- **The ending is the official logo**: when it finishes, the scene is swapped for the brand file's path. The final state is the logo, with no leftover transform, mask or crumb.
- **Replays on hover** (on a phone, on tap). The replay starts from the finished logo, "rebuilds" the cookie and ends on the logo again, which is why the GIF closes the loop with no cut: the first and last frames of the recording are identical (pixel difference of zero).

**Performance and accessibility**

- The animation library is loaded on demand (dynamic `import()`): it stays out of the page's initial JS.
- It only starts when at least half of the logo is on screen (`IntersectionObserver`).
- Everything is written as SVG attributes on each frame, with no inline style left over.
- `prefers-reduced-motion: reduce` shows the finished logo straight away, with no animation or replay.
- The SVG has `role="img"` and `aria-label`; without JavaScript, a `<noscript>` shows the static logo.

---

## Design decisions

**Direction**: contemporary artisanal confectionery, with the countertop, raw dough, cocoa and the brand pink. The sweet (the photo) is the protagonist; the rest is a quiet frame. Mobile-first, because almost all traffic comes from phones. The full brief is in `frontend/PRODUCT.md` and `frontend/DESIGN.md`.

**Palette** (website tokens in `frontend/app/site.css`; the app uses the same values in `mobile-common/lib/ui/theme.dart`):

| Role | Value | Why |
|---|---|---|
| Cream background | `#FBF3EA` | the warmth of bakery paper instead of cold white |
| Cocoa ink | `#2A160F` | high-contrast text (15.7:1 on the cream, according to `DESIGN.md`) |
| Chocolate | `#3A1D12` | dark bands that give the page rhythm |
| Brand pink | `#E94B8A` | fill and decoration; kept rare, so it feels intentional |
| Action pink | `#D42A76` | primary button (4.6:1 with light text) |
| Text pink | `#A81857` | links and chips (6.5:1) |

The brand pink is not used as a text color (low contrast on the cream). A darker pink exists for that.

**Typography**: headings in **Young Serif** (the homey side, against the logo, which is a heavy grotesque) and body text and prices in **Figtree**. Both fonts are bundled with the project, asking nothing from third-party servers: on the website through `@fontsource` (Latin subset only, `font-display: swap`) and in the app as font files.

**Shape**: arch frame for the main photo (reminiscent of the oven's mouth), cards with 24 px corners, pill buttons, depth through tone rather than shadow.

**With no photo, the brand takes its place**: an item without a photo shows the cookie symbol over a tone of the sweet (caramel for cookie, brown for brownie, soft pink for the rest). This was a deliberate decision: the menu stays cohesive even when not every item has a photo.

**Component system**: the public website has its styles isolated under `.rk-site` (nothing leaks into the logged-in area). The app builds its theme in a `ThemeExtension` (`AppColors`) with light and dark versions.

**Responsiveness**: the website is mobile-first with a fluid type scale; the app was designed for phone screens.

**Accessibility** (what is in the code):

- a "Skip to the menu" link at the top, for keyboard navigation;
- `lang="pt-BR"`, heading hierarchy and named regions (`aria-labelledby`);
- `prefers-reduced-motion` respected in the logo splash, smooth scrolling, and entrance and scroll-reveal animations;
- in the app, `Semantics` labels on the order and orders screens;
- the project guidelines are WCAG AA, visible focus and touch targets of at least 44 px (recorded in `PRODUCT.md`).

**Dark theme**: the app has a "night" variant, with chocolate in place of cream. The website (public and customer area) is light only.

---

## SEO decisions (website)

Only what exists in the code:

- **Metadata**: `title` with a template (`%s · Raykookie`), `description`, `keywords`, `applicationName`, `metadataBase` and a **canonical URL** per page (`/` and `/cardapio`). Robots with `index, follow` and `max-image-preview: large`.
- **Open Graph and Twitter Cards**: `og:*` with `locale pt_BR`, a 1200×630 image with alt text and `summary_large_image`. `/cardapio` repeats the image in its own Open Graph, otherwise the shared link would come out without a photo.
- **`sitemap.xml`** generated in code, with the public pages (home, menu, terms, privacy and account deletion), frequency and priority.
- **`robots.txt`** generated in code: allows the site, blocks the API, the logged-in area and the payment link, but **allows `/api/fotos/`** (the most specific rule wins), so Google can render the home page photos.
- **`noindex` where it makes sense**: the whole customer area (`/cliente/…`, which includes login and orders). It remains crawlable on purpose (`robots.txt` doesn't block it entirely), because Google only reads `noindex` if it can access the page.
- **Structured data (JSON-LD, schema.org)**: a `@graph` with `WebSite` and `Bakery` (name, image, menu link, currency and accepted payment methods, Instagram). The JSON is escaped against `<` injection.
- **Web manifest** (`manifest.webmanifest`), icons (`.ico`, SVG and `apple-touch-icon`) and `theme-color`.
- **Google Search Console verification** via meta tag.
- **Rendering strategy**: **on-demand SSR** on the pages that depend on the menu. The home page and the menu fetch data on the server, with a short `fetch` cache, so the HTML already arrives with the items, names and photos; the crawler doesn't depend on JavaScript to see the menu. Terms and privacy are **static**, pre-rendered at build time.
- **Semantic URLs** in Portuguese: `/cardapio`, `/termos`, `/privacidade`, `/excluir-conta`.
- **Images**: declared dimensions (no layout shift), `loading="lazy"` and `decoding="async"`, and `fetchPriority="high"` on the first menu photo. The home page showcase photos use reduced versions (800 px) and have alt text.
- **Performance**: in the local production build, the home page weighs **162 kB of JS on first load** (102 kB shared). Fonts are bundled and the animation library is loaded on demand. The project also has **Vercel Speed Insights** enabled, which measures real Core Web Vitals in production.

---

## Technical decisions

### Website

| | |
|---|---|
| **Stack** | Next.js 15 (App Router), React 19, TypeScript, Tailwind CSS 4 + the public site's own CSS, anime.js 4, lucide-react |
| **Data** | Server Components fetch the menu on the server; only what is interactive is a Client Component |
| **Same origin** | Next forwards `/api/*` to the API (rewrite): from the browser's point of view there is no CORS and the session cookie works without adjustments |
| **State in the URL** | the chosen item and the click's origin go in the URL; without a session, login returns the person exactly to it. The post-login destination is validated (`safeNext`) |
| **Errors** | if the API fails, the public menu degrades to empty instead of taking the page down; there is a custom 404 page |
| **Security** | headers on all responses: CSP (enforcing `frame-ancestors`, `base-uri`, `object-src` and `form-action`), HSTS, `nosniff`, `X-Frame-Options`, `Referrer-Policy` and `Permissions-Policy`. The full CSP runs in report-only mode |
| **Shared code** | `shared/` holds types and rules in plain TypeScript, used by the website |

### Android app

| | |
|---|---|
| **Stack** | Flutter, `provider` for state, `go_router` for navigation, `http`, `intl` (pt_BR) |
| **Shared code** | the `mobile-common` package (API, session, models, theme and widgets) is shared with the house's other app |
| **Navigation** | declarative routes with guards: order screens require login and return to the right destination afterwards (`?next=`, internal paths only) |
| **Session** | the token is kept in the system vault (Keystore on Android, Keychain on iOS); a 401 expires the session and leads to login |
| **Offline and errors** | startup doesn't hang without internet (local token read, with an 8 s limit for the server); every call has a 30 s timeout and returns messages in Portuguese: "no connection" and "the server took too long" |
| **Login** | Google (`google_sign_in`) or email and password |
| **Notifications** | push notifications (Firebase) for order changes, asking permission with a "Want to know when the order changes?" card; without Firebase in the build, the app carries on without notifications |
| **Updates** | the app checks for a newer build, downloads the APK inside itself, verifies the `sha256` and opens the installer; the final confirmation is Android's |
| **Analytics** | anonymous usage analytics, **off by default** (only enabled with a build flag) |
| **Tests** | 78 widget and unit tests passing in the customer app, plus those of the shared package |

### Management Android app

| | |
|---|---|
| **Stack** | Flutter, `provider`, `go_router`, the same `mobile-common` as the customer app |
| **Navigation** | five tabs (Home, Inventory, Recipes, Orders, More) and full-screen views on top, with the owner's session guard |
| **Price calculation** | the price is calculated **on the device in real time** while the recipe is being edited and **on the server** on save; the rule exists in Dart (`mobile-common/lib/core/pricing.dart`) and in TypeScript (`shared/pricing.ts`), each with its own tests |
| **In-person sales** | static Pix per product that works without internet and without login, with the key stored on the device |
| **Invoice** | reads the NFC-e QR and stocks items into inventory, with a review screen before saving |
| **Security** | biometrics to open the app, token in the system vault and push notifications for new orders, with a dedicated sound |

### API (Java backend)

The production API was **rebuilt in Java** from the original Node version, keeping the HTTP contract so that the website and apps didn't need to change.

| | |
|---|---|
| **Stack** | Java 25, Spring Boot, PostgreSQL, Flyway, packaged in Docker |
| **Modules** | `backend-java/modules`: `catalog`, `orders`, `payments`, `refunds`, `purchasing`, `customers`, `identity`, `engagement`, `feedback`, `insights`, `notifications`, `releases` and `storefront`, each in layers (domain, application, adapters) |
| **Platform** | `backend-java/platform`: `kernel`, `web`, `persistence`, `security`, `ratelimit`, `scheduling`, `audit`, `observability` and `testing` |
| **Payments** | Pix and card through Mercado Pago (Orders API), **webhook with mandatory signature** and payment always re-queried from the Mercado Pago API; idempotent retries; automatic refund |
| **Server-side rules** | the price, the coupon discount and "paid" never come from the client: the order body only says *what* and *how many* |
| **Access control** | every route declares its level (`@Access`: owner, customer, payment link, signed webhook or public) and the tests fail if any route doesn't declare one |
| **Defenses** | passwords with scrypt, session stored as a hash, attempt limits in the database, security headers and CSP |
| **Audit** | **insert-only** history of inventory, recipes, prices, coupons and orders |
| **Scheduled tasks** | run inside the server (no external cron) |

### Quality and delivery

- **Tests against a real Postgres**: the API uses Testcontainers, not an in-memory database.
- **Architecture rules as tests**: an architecture test (ArchUnit) prevents dependencies between layers and modules outside the design, and an access-matrix test checks the `@Access` of every route.
- **Checkstyle and coverage (JaCoCo)** in `mvnw verify`.
- **Cross-language parity**: the rules for pricing, money, refunds, engagement and legal texts exist in TypeScript (website) and in Java (API). CI regenerates the test vectors from the TypeScript and compares them with the Java ones (`backend-java/rules-vectors`).
- **Additive migrations**: the migration history is verified by checksum in CI.
- **CI/CD**: CI runs only what changed, and CD, on every push to `main`, builds and **signs the APKs**, checks the certificate fingerprint, publishes to Releases and deploys the API with **automatic rollback** to the previous image if health doesn't return within 2 minutes.
- **Tested security**: there are attack tests (headers, XSS, access control).

---

## About the business

Raykookie is a small artisanal confectionery. It was born in a home kitchen, with a cookie recipe, and today makes cookies, brownies, tarts, cakes and little sweets. Everything is prepared in small batches and **only to order**, so it arrives fresh for whoever ordered it.

Almost all of the clientele arrives through Instagram and decides quickly: they see the photos, choose and want to order without hassle. The website and the app exist to keep that path short and clear, with a menu that reflects what the house is offering, an order the person puts together on their own, and simple tracking until the sweet arrives.

The product solves the problem of a small operation that wants to keep its homemade feel without giving up organized service.

---
