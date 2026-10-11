# Tech Stack Reference

A catalog of the technologies, libraries, and services used across our projects.

**Why this exists:** before starting a new project, or when revisiting an existing one, scan this list for something we've already used elsewhere that could complement it. Each entry records what the tool is good for, how we feel about it, and which projects it shipped in.

## How to use this list

Each entry has a **status** so it's clear at a glance what to reach for:

| Status | Meaning |
| --- | --- |
| ✅ **Adopt** | Proven, comfortable, default choice |
| 🧪 **Trial** | Used on a real project, promising, still forming an opinion |
| 👀 **Assess** | Worth exploring or planned; not yet used in anger |
| ⛔ **Hold** | Avoid for new work (legacy, pain points, or superseded) |

**Used in** lists the projects (from the [Projects](#projects) index) where the technology shipped. A tool with several projects behind it is proven; a tool used in only one place may be a missed opportunity for the others.

To add something, drop a row into the right table: `| Name | Status | Used for | Notes | Used in |`.

---

## Projects

Index of projects referenced in the **Used in** column.

| Project | Description | Years | Repo / path |
| --- | --- | --- | --- |
| miguelgarcia.site | Personal portfolio (miguelangelgarcia.com): server-rendered one-pager with a Claude-powered "Ask about me" assistant and an owner-only admin | 2026 | `~/Code/Claude/miguelgarcia.site` |
| Appointment Hub | B2B scheduling SaaS connecting healthcare offices with pharma and medical sales reps (booking, calendar sync, rewards, referrals, payments) | 2024–2026 | `~/Code/www.meetingsmywayapp.site` |
| Quality Cleaning Plus | One Laravel app replacing two legacy apps for a staffing and cleaning company: QC Minute timesheets and clock-in (QR, GPS, kiosk tablets), the back office (people, work orders, invoicing, payroll, PTO, inventory, workflows), and the bilingual marketing site | 2026 | `~/Code/Claude/qcpminute.site` |

---

## 1. Languages

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| PHP 8.5 | ✅ Adopt | Server-side language | Minimum 8.3 | miguelgarcia.site, Appointment Hub, Quality Cleaning Plus (8.4) |
| TypeScript | ✅ Adopt | All frontend code | Type-checked with `tsc` | miguelgarcia.site, Appointment Hub, Quality Cleaning Plus |

## 2. Backend

### Frameworks

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Laravel 13 | ✅ Adopt | Full-stack PHP framework |  | miguelgarcia.site, Appointment Hub (v12), Quality Cleaning Plus |
| Inertia v3 | ✅ Adopt | Bridge between Laravel and React without a separate API | Server-side adapter; supports SSR via a Node server. React adapter under Frontend | miguelgarcia.site, Quality Cleaning Plus (v2) |
| Laravel React Starter Kit | ✅ Adopt | Official scaffold: Inertia + React + TypeScript + shadcn/ui | Ships ESLint and tsc checks | |

### Authentication & Authorization

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Laravel Fortify | ✅ Adopt | Headless auth backend (login, registration, 2FA, password reset) |  | Appointment Hub, Quality Cleaning Plus |
| Laravel Sanctum | ✅ Adopt | SPA session auth and API tokens | Also long-lived device tokens for kiosk tablets | Appointment Hub, Quality Cleaning Plus |
| Passkeys (WebAuthn) | ✅ Adopt | Passwordless sign-in | | |
| spatie/laravel-permission | ✅ Adopt | Roles and permissions |  | Appointment Hub, Quality Cleaning Plus |
| CASL (`@casl/ability`, `@casl/vue`) | 🧪 Trial | Frontend permission checks mirroring backend abilities | Shared abilities enum | Appointment Hub |
| Laravel Socialite | ✅ Adopt | OAuth sign-in and consent flows | SocialiteProviders for Google and Microsoft Azure; used for calendar connections | Appointment Hub |
| lab404/laravel-impersonate | ✅ Adopt | Admins sign in as another user for support |  | Appointment Hub |
| OIDC | 👀 Assess | Identity direction / SSO | | |

### Queues & Scheduling

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Laravel Scheduler | ✅ Adopt | Cron-style scheduled tasks |  | Appointment Hub, Quality Cleaning Plus |
| Laravel Horizon | ✅ Adopt | Queue monitoring dashboard and worker config | Requires Redis; behind an admin gate | |

### APIs & Integrations

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Saloon v4 | ✅ Adopt | Structured HTTP clients for vendor APIs | One connector per vendor | |
| spatie/laravel-query-builder | ✅ Adopt | Filtering, sorting, and includes from API query strings |  | Appointment Hub |

### Real-time & Notifications

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Laravel Reverb | ✅ Adopt | First-party WebSocket server | Live progress updates | Quality Cleaning Plus |
| Laravel Echo | ✅ Adopt | Client for broadcast events | `useEcho` hook in React; `@ably/laravel-echo` when broadcasting through Ably; `pusher-js` as the transport for Reverb | Appointment Hub, Quality Cleaning Plus |
| Ably | ⛔ Hold | Hosted pub/sub for broadcasting (managed alternative to Reverb) | `ably/laravel-broadcaster` + `@ably/laravel-echo`; Hold: use Reverb for new work | Appointment Hub |
| minishlink/web-push | 🧪 Trial | Browser push notifications (VAPID) | Custom notification channel alongside email and SMS | Appointment Hub |

### Content, Media & Documents

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| spatie/laravel-medialibrary | ✅ Adopt | Media storage and image conversions | e.g. sm/md/lg conversions | |
| HTML Purifier | ✅ Adopt | Rich-text sanitization | Allowlist profiles; sanitize on every save; `mews/purifier` is the Laravel wrapper | Appointment Hub |
| FFmpeg / ffprobe | ✅ Adopt | Video/audio assembly and media file inspection | Run from queued jobs | |
| barryvdh/laravel-dompdf | ✅ Adopt | Server-side PDFs rendered from Blade views | e.g. appointment attendee lists; invoices and reports | Appointment Hub, Quality Cleaning Plus |
| Intervention Image v3 | 🧪 Trial | Server-side image manipulation |  | Appointment Hub |
| Laravel Excel (`maatwebsite/excel`) | ✅ Adopt | Spreadsheet imports and exports | Hours import wizard; report exports | Quality Cleaning Plus |

### Dates, Recurrence & Validation

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| rlanvin/php-rrule | ✅ Adopt | RFC 5545 recurrence rules (recurring availability) | Paired with `rrule` on the frontend | Appointment Hub |
| Yasumi | 🧪 Trial | Public holiday calendars by country |  | Appointment Hub |
| libphonenumber-for-php | ✅ Adopt | Phone number parsing and validation | Custom `PhoneNumber` validation rule | Appointment Hub |

### Search

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Laravel Scout | 👀 Assess | Search abstraction over models | Planned | |
| Meilisearch | 👀 Assess | Search engine behind Scout | Planned | |

## 3. Frontend

### Frameworks & Rendering

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| React 19 | ✅ Adopt | Admin / app UIs | React Compiler enabled via Babel plugin | miguelgarcia.site, Quality Cleaning Plus |
| Inertia v3 (React adapter) | ✅ Adopt | Page components driven by Laravel routes | SSR available for public pages | miguelgarcia.site, Quality Cleaning Plus (v2) |
| Blade | ✅ Adopt | Server-rendered public pages | Bilingual (EN + `/es`) marketing site | miguelgarcia.site, Quality Cleaning Plus |
| Alpine.js | ✅ Adopt | Light interactivity on server-rendered pages | Keeps React off visitor-facing pages | |
| Vue 3 | ⛔ Hold | SPA app UIs | Composition API with `<script setup>`; Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |
| Vue Router + unplugin-vue-router | ⛔ Hold | File-based, typed routing for Vue | Layouts via `vite-plugin-vue-layouts`; Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |

### UI Kits & Admin Themes

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Mantine 9 | ✅ Adopt | React component library for admin UIs | Loaded on admin pages only | miguelgarcia.site |
| Paces admin theme (Coderthemes) | ✅ Adopt | Admin shell (sidenav, top bar) | Minute variant; reuse its demo markup before building new UI | Quality Cleaning Plus |
| Preline UI | ✅ Adopt | Tailwind-based dropdowns, accordions, etc. | Paired with Paces; Stateless chrome only (tabs, dropdowns, accordions); stateful pieces are React components | Quality Cleaning Plus |
| shadcn/ui | ✅ Adopt | Copy-in React components built on Tailwind | Default in the Laravel React Starter Kit | |
| Vuetify 3 | ⛔ Hold | Material component library for Vue | Theming through SCSS variables; `vuetify-sonner` for toasts; Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |
| Vuexy admin template (Pixinvent) | ⛔ Hold | Vue + Laravel admin shell, layouts, and core components | Ships many demo dependencies; only some are used; Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |

### Styling

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Tailwind CSS v4 | ✅ Adopt | Utility-first styling | CSS-based config via `@tailwindcss/vite`, no config file; `@tailwindcss/forms` and `@tailwindcss/typography` plugins | miguelgarcia.site, Quality Cleaning Plus |
| CSS custom properties (design tokens) | ✅ Adopt | Per-site theming | Tokenized variables layered with Tailwind utilities; Tailwind v4 `@theme` tokens | miguelgarcia.site |
| Bunny Fonts | 🧪 Trial | Privacy-friendly Google Fonts alternative | Self-hosted at build time via `laravel-vite-plugin/fonts` + `@fonts` Blade directive | miguelgarcia.site |
| Sass (SCSS) | ✅ Adopt | Theme variables and component styles | Drives Vuetify's SCSS config; Marketing site styles | Appointment Hub, Quality Cleaning Plus |
| Bootstrap 5 | ⛔ Hold | Marketing site layout and components | SCSS build with `quietDeps` to silence Bootstrap's Sass deprecations; Hold: use Tailwind for new work | Quality Cleaning Plus |

### Icons

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Tabler Icons | ✅ Adopt | Icon set | MDI and Font Awesome sets also used via Iconify | Appointment Hub |
| @iconify/react / @iconify/vue | ✅ Adopt | Runtime icon loading in admin UIs | `@iconify/vue` in Vue apps | Appointment Hub, Quality Cleaning Plus |
| @iconify-json/tabler | ✅ Adopt | Generated, bundled icon subset | Inlined for public pages (no runtime fetch); `build:icons` script generates the bundle | Appointment Hub |

### Rich Text

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| TipTap v3 | ✅ Adopt | Rich-text editing | StarterKit; inline editing planned | Appointment Hub (v2) |
| Quill (`react-quill-new`) | ⛔ Hold | Rich-text editing | Knowledge base articles; Hold: use TipTap for new work | Quality Cleaning Plus |

### State Management

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Zustand | ✅ Adopt | Client state stores | | |
| immer | ✅ Adopt | Immutable updates with mutable syntax | Used with Zustand | |
| zundo | ✅ Adopt | Undo/redo middleware for Zustand | | |
| Pinia | ⛔ Hold | Vue state stores | Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |

### Forms & Validation

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| vee-validate + Yup | ⛔ Hold | Form state and schema validation | Schemas kept in a `validations/` folder; Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |
| Maska | 🧪 Trial | Input masks (phone numbers, etc.) |  | Appointment Hub |

### Dates & Calendars

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Luxon | ✅ Adopt | Date/time and time zone handling | Auto-imported `DateTime` | Appointment Hub |
| FullCalendar 6 | ✅ Adopt | Calendar views (day, week, month, list) | Luxon and RRule plugins | Appointment Hub |
| rrule.js | ✅ Adopt | Recurrence rules on the client | Mirrors php-rrule on the backend | Appointment Hub |
| flatpickr (`vue-flatpickr-component`) | ✅ Adopt | Date and time pickers | Wrapped in an `AppDateTimePicker` component; Also vanilla on Blade pages | Appointment Hub, Quality Cleaning Plus |

### Charts & Data Display

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Chart.js + vue-chartjs | ⛔ Hold | Dashboard charts | Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |
| ApexCharts (`react-apexcharts`) | 🧪 Trial | Dashboard trend charts |  | Quality Cleaning Plus |
| TanStack Table | ✅ Adopt | Headless data tables (sorting, filtering, pagination) | Server-side pagination for ledgers, client-side for short config lists | Quality Cleaning Plus |
| react-countup | 🧪 Trial | Animated stat counters on dashboards |  | Quality Cleaning Plus |

### Interaction & Utilities

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| dnd-kit | ✅ Adopt | Drag and drop, including nested sortable containers | | |
| nanoid | ✅ Adopt | Short unique IDs on the client | | |
| Axios + axios-auth-refresh | 🧪 Trial | API calls with automatic token refresh | Sanctum token auth | Appointment Hub |
| VueUse | ⛔ Hold | Composition utilities for Vue (core, math, router, integrations) | Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |
| remeda | 🧪 Trial | Typed functional utilities (lodash alternative) |  | Appointment Hub |
| vue-i18n | ⛔ Hold | Internationalization | `@intlify/unplugin-vue-i18n` precompiles locale files; Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |
| @unhead/vue | ⛔ Hold | Page title and meta tags in an SPA | Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |
| Cropper.js | 🧪 Trial | Image cropping before upload |  | Appointment Hub |
| qr-code-styling + html2pdf.js | 🧪 Trial | Branded QR codes and client-side PDF export | Referral share cards | Appointment Hub |
| html2canvas | 🧪 Trial | Page screenshots in the browser | Attached to in-app feedback reports | Appointment Hub |
| qrcode.react | ✅ Adopt | QR codes rendered in React | Printable property clock-in QR codes | Quality Cleaning Plus |
| usehooks-ts | ✅ Adopt | Typed React hooks collection |  | Quality Cleaning Plus |
| clsx + tailwind-merge | ✅ Adopt | Conditional class names without Tailwind conflicts |  | Quality Cleaning Plus |
| SimpleBar (`simplebar-react`) | 🧪 Trial | Custom scrollbars in the admin layout |  | Quality Cleaning Plus |
| GSAP | 🧪 Trial | Marketing site animations |  | Quality Cleaning Plus |
| Swiper | 🧪 Trial | Carousels (marketing testimonials) |  | Quality Cleaning Plus |

## 4. Build Tooling

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Vite 8 | ✅ Adopt | Frontend bundler and dev server |  | miguelgarcia.site, Appointment Hub (v5), Quality Cleaning Plus (v7) |
| Vite+ (vite-plus) | ✅ Adopt | Unified toolchain wrapper around Vite | | |
| Laravel Wayfinder | ✅ Adopt | Generated TypeScript helpers for routes and controller actions | Generated output; never hand-edit; `formVariants` enabled | Quality Cleaning Plus |
| babel-plugin-react-compiler | ✅ Adopt | Automatic memoization for React | | |
| unplugin-auto-import + unplugin-vue-components | ⛔ Hold | Auto-import composables and components (no import boilerplate) | Generates `auto-imports.d.ts` / `components.d.ts`; Hold: React is the default; keep for maintaining existing Vue apps | Appointment Hub |

## 5. Data

### Databases

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| PostgreSQL 17 | ✅ Adopt | Relational database | Use when you need JSONB or stricter typing: typed columns plus indexed JSONB, real foreign keys | |
| MySQL 8 / MariaDB | ✅ Adopt | Relational database | Production database on every reviewed project; available on any host, including shared hosting; run in Docker for local dev | miguelgarcia.site (production), Appointment Hub (production and tests), Quality Cleaning Plus (production) |
| SQLite | ✅ Adopt | Fast local / CI test runs | In-memory for Pest; also fine as the local dev database for small apps; In-memory test runs | miguelgarcia.site, Quality Cleaning Plus |

### Cache & Sessions

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Redis 7 | ✅ Adopt | Cache, sessions, and queue backend | Also concurrency / rate limits | Quality Cleaning Plus (production queue and cache) |

### Backups

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| spatie/laravel-backup | ✅ Adopt | Nightly encrypted database backups | Stored in a private bucket in a different region | Appointment Hub |

### Audit Logging

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| spatie/laravel-activitylog | ✅ Adopt | Model change history and a browsable audit log |  | Quality Cleaning Plus |

## 6. Testing & Quality

### Testing

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Pest 4 | ✅ Adopt | PHP tests |  | miguelgarcia.site, Quality Cleaning Plus |
| PHPUnit 11 | ⛔ Hold | PHP tests (class-based) | Pest runs on top of it; Hold: use Pest for new work | Appointment Hub |
| ParaTest | ⛔ Hold | Run PHPUnit suites in parallel | Hold: use `pest --parallel` for new work | Appointment Hub |
| Vitest | ✅ Adopt | JS/TS unit and component tests | | |
| Testing Library | ✅ Adopt | Component testing by user behaviour | | |
| jsdom | ✅ Adopt | DOM environment for Vitest | | |
| Playwright | 👀 Assess | End-to-end browser tests | Planned | |

### Linting, Formatting & Static Analysis

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Laravel Pint | ✅ Adopt | PHP code style | `laravel` preset | miguelgarcia.site, Appointment Hub, Quality Cleaning Plus |
| PHPStan + Larastan | ✅ Adopt | PHP static analysis | Level 7 | Quality Cleaning Plus (level 5) |
| ESLint 9 | ✅ Adopt | JS/TS linting | With `typescript-eslint` and React Hooks rules | Quality Cleaning Plus (v10) |
| Prettier | ✅ Adopt | JS/TS/CSS formatting | | |
| prettier-plugin-tailwindcss | ✅ Adopt | Automatic Tailwind class sorting | | |
| tsc | ✅ Adopt | TypeScript type checking | `vue-tsc` for Vue single-file components | miguelgarcia.site, Appointment Hub, Quality Cleaning Plus |
| Stylelint | 🧪 Trial | SCSS/CSS linting | Standard SCSS config, idiomatic property order, logical properties | Appointment Hub |

### Dependency Scanning

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| composer audit | ✅ Adopt | PHP dependency vulnerability scan | Run in CI | |
| npm audit | ✅ Adopt | JS dependency vulnerability scan | Run in CI | |
| Dependabot | ✅ Adopt | Automated dependency update PRs | Weekly, grouped per ecosystem | Appointment Hub |

### Accessibility
<!-- e.g. axe, Lighthouse, Silktide, pa11y -->

## 7. CI/CD

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| GitHub Actions | ✅ Adopt | CI (tests, lint, types, static analysis) and push-to-deploy | Service containers for Postgres 17 and Redis 7; rsync-over-SSH deploy with a manual dry-run option | miguelgarcia.site (deploy), Appointment Hub (build + deploy) |
| codacy/git-version | 🧪 Trial | Automatic semantic version tags in CI | Version also written to a `version` file and passed to Vite | Appointment Hub |

## 8. Infrastructure & Hosting

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Laravel Forge | ✅ Adopt | Server provisioning and deploys | Forge CLI pulls `.env` in CI; deploys from a prebuilt-assets branch | Appointment Hub |
| Hetzner | 👀 Assess | VPS hosting | Planned; candidate behind Forge | |
| DigitalOcean | ✅ Adopt | VPS hosting | Production servers managed through Forge | Appointment Hub |
| Laravel Cloud | 🧪 Trial | Fully managed Laravel hosting (no server management) | Alternative to Forge + VPS | Quality Cleaning Plus (staging / cutover rehearsal) |
| Bluehost | ✅ Adopt | Shared (cPanel) hosting | Budget pick for small marketing sites with a simple CMS or CRM; PHP + MySQL, no Node at runtime; deployed with rsync over SSH. Larger apps go to Forge + DigitalOcean or Laravel Cloud | miguelgarcia.site |

### CDN & Edge
<!-- e.g. Cloudflare, CloudFront, Fastly -->

### Infrastructure as Code
<!-- e.g. Terraform, Pulumi, AWS CDK -->

## 9. Observability

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Sentry | 👀 Assess | Error tracking | Planned | |
| Laravel Pulse | ✅ Adopt | App health and performance dashboard | Behind an admin gate | |
| Laravel Telescope | ✅ Adopt | Request, query, and job debugging | Local development only | Appointment Hub |
| Laravel Nightwatch | 🧪 Trial | Hosted Laravel monitoring |  | Appointment Hub |
| Clockwork | ⛔ Hold | Request profiling in browser dev tools | Development only; Hold: use Telescope locally, Nightwatch in production | Appointment Hub |

## 10. Local Development

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Laravel Herd | ✅ Adopt | Local PHP, Postgres, and Redis | Serves `*.test` domains, no `artisan serve`; Multiple `.test` hosts for multi-domain apps | miguelgarcia.site, Quality Cleaning Plus |
| Laravel Pail | ✅ Adopt | Tail application logs in the terminal | Runs inside `composer dev` | miguelgarcia.site, Quality Cleaning Plus |
| Docker | ✅ Adopt | Containerized local services | Can run the full dev stack (PHP-FPM, nginx, MySQL 8, Redis) with `bin/` wrapper scripts | Appointment Hub |

## 11. Third-Party Services

### Vendor APIs

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Yardi RentCafe | ✅ Adopt | Property-management vendor API | Integrated via Saloon | |
| Trello API | 🧪 Trial | In-app feedback lands as Trello cards |  | Appointment Hub |
| Tremendous | 🧪 Trial | Gift card and reward payouts | Rewards and referral bonuses | Appointment Hub |

### Calendar

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Google Calendar API (`google/apiclient`) | ✅ Adopt | Sync appointments to Google Calendar | OAuth; hourly retry of failed syncs | Appointment Hub |
| Microsoft Graph (Outlook Calendar) | ✅ Adopt | Sync appointments to Outlook calendars | Called with Laravel's HTTP client; OAuth via Socialite | Appointment Hub |

### Maps

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| MapLibre GL JS | ✅ Adopt | Open-source interactive vector maps | Open fork of Mapbox GL; no API key; Geofence map for clock-in locations | Quality Cleaning Plus |
| OpenFreeMap | ✅ Adopt | Free vector tile hosting and map styles | Tile source for MapLibre; no API key or usage limits | Quality Cleaning Plus |

### Email

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Postmark | ✅ Adopt | Transactional email delivery | First-party Laravel mail driver | miguelgarcia.site, Appointment Hub, Quality Cleaning Plus |

### SMS

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Twilio | ✅ Adopt | SMS notifications | `laravel-notification-channels/twilio` | Appointment Hub |

### Spam & Bot Protection

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Google reCAPTCHA v3 | ✅ Adopt | Invisible spam scoring on public forms | Scores logged, low scores flagged; optional hard block threshold | miguelgarcia.site, Quality Cleaning Plus |

### Analytics

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Google Analytics 4 | ✅ Adopt | Site traffic analytics | Measurement ID set in production only; Skipped in demo mode | miguelgarcia.site, Quality Cleaning Plus |
| Google Tag Manager | 🧪 Trial | Tag management for analytics | `@gtm-support/vue-gtm` | Appointment Hub |

### Payments & Billing

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Stripe | ✅ Adopt | Payment processing |  | Appointment Hub |
| Laravel Cashier (Stripe) | ✅ Adopt | Subscriptions and one-off top-ups | Stripe pricing table embed | Appointment Hub |
| Stripe Tax | ✅ Adopt | Sales tax / VAT calculation | | |
| Stripe Radar | ✅ Adopt | Fraud checks | | |

### Storage

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Cloudflare R2 | ✅ Adopt | Object storage for all files | S3-compatible, so any S3 store works | |
| DigitalOcean Spaces | 🧪 Trial | S3-compatible object storage (private buckets) | Via `league/flysystem-aws-s3-v3`; also holds backups | Appointment Hub |

### Social Publishing

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| YouTube Data API | ✅ Adopt | Google sign-in and resumable video upload | | |
| TikTok Content Posting API | ✅ Adopt | Posting videos to TikTok | Inbox/draft flow first; Direct Post behind a flag | |

## 12. CMS & Content
<!-- e.g. WordPress, Statamic, Strapi, Sanity, custom CMS -->

## 13. AI & LLM Tooling

### Models & Providers

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Anthropic Claude API | 🧪 Trial | Text LLM tasks: classification with JSON-schema output, streamed answers | Two tiers: Haiku classifies, Sonnet answers | miguelgarcia.site |
| Qwen (Alibaba Cloud Model Studio) | ✅ Adopt | Text and vision LLM tasks: writing, editing, moderation, review | OpenAI-compatible API | |
| fal.ai | ✅ Adopt | Hosted generative media models (image, video) | | |
| MiniMax Hailuo | ✅ Adopt | Image-to-video clip generation | Served via fal.ai | |
| ElevenLabs | ✅ Adopt | Text-to-speech narration | Returns word-level timestamps | |

### SDKs

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| openai-php/laravel | ✅ Adopt | Laravel client for OpenAI-compatible APIs | Works with non-OpenAI providers like Qwen | |
| anthropic-ai/sdk (PHP) | 🧪 Trial | Official Anthropic PHP SDK | Streaming via `createStream`; structured output | miguelgarcia.site |

### Developer Tools

| Technology | Status | Used for | Notes | Used in |
| --- | --- | --- | --- | --- |
| Claude Code | ✅ Adopt | AI-assisted development | Project `CLAUDE.md` holds architecture rules and commands | miguelgarcia.site, Appointment Hub, Quality Cleaning Plus |
| laravel/pao | 🧪 Trial | Agent-optimized output for PHP testing tools | Pairs with Pint's `--format agent` | miguelgarcia.site |
| Laravel Boost | 🧪 Trial | MCP server giving AI agents Laravel-aware context and docs |  | Appointment Hub |
| Serena | 🧪 Trial | MCP server for symbol-level code navigation and edits | Optional for contributors | Appointment Hub |
| graphify | 🧪 Trial | Codebase knowledge graph for AI agents | Git hooks rebuild it on commit | Appointment Hub |

## 14. Design & Collaboration
<!-- e.g. Figma, Jira, Linear, Notion, Slack -->

---

## Patterns & Approaches

Stack-level choices worth reusing, independent of any single tool.

- **Split rendering:** React (via Inertia) for admin UIs; Blade + Alpine.js server-rendered public pages so no React ships to visitors. Alternative: Inertia SSR for public pages.
- **Hybrid schema:** typed columns for known fields, JSONB for flexible data, and real foreign keys throughout.
- **Tokenized theming:** CSS custom properties as design tokens, layered with Tailwind utilities, for per-site themes.
- **Sanitize on save:** run rich-text HTML through an allowlist sanitizer every time a draft is saved, not only at render.
- **Icons by audience:** runtime icon loading in the admin; a generated, bundled subset inlined on public pages.
- **Generated code stays generated:** route/action helpers (Wayfinder) are regenerated, never hand-edited.
- **Same services in CI as prod:** CI runs against the real database and Redis versions, with SQLite reserved for fast local test runs.
- **Queues by workload:** separate named queues for different job types (e.g. LLM calls, generation, rendering, default) so slow work can't starve fast work.
- **Per-tenant concurrency limits:** Redis-backed limits per team/account to keep one customer from hogging shared workers or provider quotas.
- **In-house tracing first:** a dedicated events table for tracing before adopting a third-party tracing tool.
- **Staged third-party rollouts:** start with the lowest-risk API flow (e.g. drafts) and put riskier ones (e.g. direct posting) behind a feature flag.
- **Dev tools stay dev-only:** debugging tools like Telescope run locally only; ops dashboards (Horizon, Pulse) sit behind an admin gate.
- **Dependency audits in CI:** `composer audit` and `npm audit` run on every build.
- **Crawler-first public pages (SEO + GEO):** server-rendered HTML with JSON-LD (`Person`, `WebSite`), a sitemap, a `robots.txt` that allows AI crawlers, and an `llms.txt` summary.
- **React islands on a server-rendered fallback:** mount a small React component into a Blade page that already works without JS.
- **Budget-tiered hosting:** match hosting to the app's size and budget. Small marketing sites with a simple CMS or CRM go on shared hosting; apps that need queue workers, Redis, or WebSockets go on Forge + DigitalOcean or Laravel Cloud.
- **Node at build time only:** Vite builds assets in CI; production runs PHP only (no SSR), which makes cheap shared hosting viable.
- **Swappable AI providers:** the app talks to `LlmClient` / `KnowledgeRetriever` interfaces; one class per provider, and a fake client in tests so the real API is never called.
- **Two-tier models:** a small, cheap model classifies the request (JSON-schema output); a stronger model writes the answer.
- **Keyword retrieval before embeddings:** start RAG with simple category/tag/keyword scoring over a curated knowledge base; move to embeddings only when needed.
- **Streamed AI answers over SSE:** responses streamed from a POST endpoint, read on the client from the `fetch` body (EventSource is GET-only).
- **AI usage logging without PII:** log every question with token counts and estimated cost, but no visitor personal data.
- **Named rate limiters:** per-IP limits per minute and per day on public endpoints that cost money (AI) or attract abuse (login, contact).
- **Content and business rules in config:** site copy (profile, projects, experience) and business rules (booking windows, reward rates, limits) live in config files, not in templates or code.
- **Canonical URL middleware:** one redirect sends http, www, and alias hosts to the single canonical https host in production.
- **Modular monolith:** one Laravel app split into domain modules (`app/Modules/<Domain>` or `app/Domain/<Context>`), each with its own models, actions, services, events, jobs, and policies.
- **Thin controllers:** authorize, validate with a Form Request, call one Action, return a response.
- **Prebuilt deploy branches:** CI builds assets, tags a version, and force-pushes a deploy branch that Forge deploys, so servers never run Node.
- **Docker wrapper scripts:** small `bin/artisan`, `bin/composer`, `bin/phpunit`, `bin/mysql` scripts run commands in the right container from anywhere in the repo.
- **Health endpoints for uptime monitoring:** token-protected `/health/*` checks (disk, queue and scheduler heartbeats, failed jobs, integration syncs) feed an external monitor and a public status page.
- **Env feature flags that keep data:** a flag hides a feature in the UI and API without deleting its data.
- **Mirrored permissions:** roles and permissions enforced on the server (spatie) and mirrored on the client (CASL) for UI decisions.
- **RRULE recurrence end to end:** recurring availability stored as RFC 5545 rules and expanded with the same standard on server and client.
- **Off-region encrypted backups:** nightly database backups, password-protected, stored in a private bucket in a different region from the app.
- **Agent-ready repos:** `CLAUDE.md` / `AGENTS.md` with commands and workflow, MCP servers (Boost, Serena), a committed codebase graph, and a test-first task loop.
- **Architecture decision records:** notable decisions written up as ADRs; ADRs are immutable, so a change means a new ADR that supersedes the old one.
- **Spec-first docs:** a numbered `docs/` tree (context, architecture, domain, schema, flows, UI, decisions, plan, open questions) written before the build, with a status table at the top of each file.
- **One app, several domains:** `Route::domain()` with hostnames from config serves multiple surfaces (marketing, back office, field app) from one deployable, each with its own Vite bundle.
- **Immutable financial records:** time entries snapshot their rates; a sent invoice is frozen and corrected only by void and reissue.
- **Workflows as code:** each workflow type is a PHP definition class resolved through a registry, sharing generic workflow tables and one task inbox.
- **Self-checking system reference:** the admin renders live catalogs of permissions, notifications, and scheduled tasks, and a test fails when they drift.
- **Kiosk device tokens:** tablets pair once with a short code and get a long-lived, revocable Sanctum token locked to one location.
- **Rehearsed data cutovers:** a scripted legacy migration rebuilds, runs a verify gate, compares table counts, and only then uploads to a rehearsal environment.
- **Demo mode:** a seeded demo company with reset and simulate commands for client walkthroughs.

---

## Decision Log

Record notable stack decisions here so the reasoning isn't lost.

| Date | Decision | Chosen | Alternatives considered | Why |
| --- | --- | --- | --- | --- |
| 2026-10-11 | Primary relational database | PostgreSQL 17 | MySQL 8 / MariaDB | JSONB with indexing suits the hybrid schema (typed columns + flexible data); strict foreign keys and types; runs locally via Herd and in CI as a service container, so dev, CI, and prod match |
| 2026-10-11 | MySQL status | MySQL back to ✅ Adopt, alongside PostgreSQL | MySQL on ⛔ Hold | All three reviewed projects run MySQL in production; it is proven, not legacy. PostgreSQL stays the default for new work that needs JSONB |
| 2026-10-11 | Default frontend framework | React 19 + Inertia | Vue 3 ecosystem (Vuetify, Pinia, Vuexy) | Both 2026 projects are React; one stack keeps components, hooks, and UI kits reusable across projects. Vue tools stay listed (on Hold) for maintaining Appointment Hub |
| 2026-10-11 | Real-time broadcasting | Laravel Reverb | Ably | Budget: Reverb runs on the app's own server with no per-message billing, unlike Ably. It's first-party, and Laravel Echo works with either, so switching later is cheap |
| 2026-10-11 | PHP test framework | Pest 4 | PHPUnit (class-based), ParaTest | Pest runs on PHPUnit, has `--parallel` built in, and is already used in the two newest projects |
| 2026-10-11 | Hosting | By budget and app size: shared hosting (Bluehost) for small marketing sites with a simple CMS or CRM; Forge + DigitalOcean or Laravel Cloud for larger apps | Everything on DigitalOcean or AWS | Budget. Shared hosting is the cheapest fit for small sites, but it can't run persistent queue workers, Redis, or a WebSocket server, so larger apps move up. Forge + DigitalOcean has run Appointment Hub in production for two years |
| 2026-10-11 | Rich-text editor | TipTap | Quill | One editor across projects; TipTap is headless and extensible |
| 2026-10-11 | Request debugging | Laravel Telescope (local) + Nightwatch (production) | Clockwork | First-party tools; one profiler per environment |
| 2026-10-11 | Marketing site styling | Tailwind CSS | Bootstrap 5 | Same styling system as the app surfaces; Bootstrap was only used because a theme shipped with it |
