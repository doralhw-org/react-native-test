# Senior React Native Developer Challenge

Welcome to our Senior React Native Developer Challenge, designed to comprehensively assess your technical expertise, architectural thinking, and leadership capabilities in mobile development. This challenge goes beyond basic coding to evaluate how you approach complex problems, make architectural decisions, lead technical initiatives, and deliver production-ready solutions. Our aim is to understand not only your technical skills but also how you think, communicate, and deliver as a senior engineer.

## Introduction

This challenge involves building "WikiDaily", a production-minded mobile application that lets users explore Wikipedia Featured Content by date and language, with an offline-first experience and shareable cards. The project demonstrates three core competencies we value:

- **Hands-on excellence in React Native**: offline-first UX, native integrations, performance optimization, and comprehensive testing.
- **Team lead skills**: architecture decisions, code review quality, delivery planning, PR hygiene, and technical documentation.
- **Backend for mobile**: NestJS APIs optimized for mobile devices including latency considerations, payload optimization, caching strategies, versioning, and resilience.

The challenge emphasizes shipping a small but production-minded application with clean code, tests, and documentation. We provide core requirements (must-have features) and stretch goals (nice-to-have features that demonstrate seniority).

## Frontend

Build a mobile application using React Native with TypeScript that interacts with Wikipedia Featured Content and provides an exceptional user experience on both iOS and Android platforms.

### Requirements

#### Date & Language Selection
- Platform-appropriate pickers (iOS/Android) for date and language selection.
- Fetch featured content for the selected day and language.
- Persist the user's last selection across app sessions.

#### Card-Based Feed
- Scrollable list of featured items displaying title, thumbnail, and short extract.
- Pull-to-refresh functionality.
- Mark items as "viewed" with a visible indicator.
- Tap to open a Details screen showing more text and hero image (if available).
- Optimize for fast list rendering with approximately 100 items.

#### Native Sharing
- Integrate native share sheet from both card view and Details screen.
- Support sharing via messaging apps, social media, etc.

#### Offline-First Architecture
- View previously fetched days without network connectivity.
- Graceful "you're offline" states with appropriate user messaging.
- Background refresh when the app regains connectivity (if feasible).
- Implement robust caching strategy for seamless offline experience.

#### Performance & UX Polish
- Image caching to avoid unnecessary network requests.
- Prevent layout shift during image loading.
- Support Dark Mode and Dynamic Type (font scaling).
- Accessibility labels, proper hit slop, color contrast.
- VoiceOver/TalkBack sanity checks.

### Technical Stack & Architecture

- **Framework**: React Native (TypeScript). You may use Expo or bare React Native. If using Expo, document where you'd need to eject for specific requirements.
- **State Management**: React Query/TanStack Query or Redux Thunk (or equivalent) for network requests and caching.
- **Data Persistence**: On-device persistence using SQLite/Drizzle/WatermelonDB/Realm for offline support.
- **Navigation**: React Navigation with minimum two screens (Feed, Details).
- **List Performance**: FlatList/SectionList with proper keys, windowSize, and getItemLayout where appropriate.
- **Image Optimization**: Image caching using `react-native-fast-image` or Expo equivalents.

### Testing Requirements

- **Unit Tests**: Jest + React Testing Library for critical components and hooks.
- **E2E Tests**: Detox for at least one happy path: open app → load feed → open details → share (can be mocked).
- Include a simple performance budget in the README (e.g., TTI target on mid-range Android).

### Bonus Features (Frontend)

- **Bookmarks**: Persist user bookmarks across sessions.
- **Infinite Scroll**: Load previous day's content as user reaches the end of the feed.
- **Responsive Layouts**: Support phones and small tablets in both orientations.
- **Deep Links**: Implement deep linking (e.g., `wikidaily://item/{id}` opens Details).
- **Feature Flags**: Remotely toggle features on/off (local mock is acceptable).

## Backend

Build a mobile-oriented Node.js API using NestJS that proxies Wikipedia Featured Content and optionally provides translation services. The API should be optimized for mobile consumption with proper caching, validation, and error handling.

### Requirements

#### Core Endpoints

**GET /v1/feed**
- Proxy Wikipedia Featured Content API.
- Validate query parameters (date, language, etc.).
- Mobile optimization: return only fields the app needs.
- Optionally compress/transform images (can be simulated or documented).
- Implement caching with HTTP cache headers and server-side cache layer.
- Return structured error format for all failures.

**GET /v1/feed/translate/:lang**
- Same functionality as `/v1/feed` but translates `title` and `extract` into target language.
- Validate supported languages against LibreTranslate API.
- Use LibreTranslate API (self-hosted or community instance; no paid keys).

#### Architecture & Best Practices

- **Versioning**: Implement API versioning (`/v1/...`) with documentation on how `/v2` could evolve.
- **Rate Limiting**: Per-IP rate limiting to prevent abuse.
- **Caching**: In-memory or Redis caching with documented TTL choices.
- **Observability**:
  - Health check endpoint (`/healthz`).
  - Basic metrics (Prometheus or simple counters) or OpenTelemetry traces (bonus).
- **Documentation**: Swagger/OpenAPI documentation.
- **Security**: Environment variable validation, no secrets in code, proper `.env.example`.

### Testing Requirements

- **Unit Tests**: Services, pipes, guards coverage.
- **E2E Tests**: At least one e2e test using supertest with cache header assertions.

### Packaging

- **Docker**: Dockerfile and `docker-compose.yml` for local development.
- Support running NestJS API with optional Redis.
- Fast local setup experience (goal: running in under 10 minutes).

### Bonus Features (Backend)

- **Push Notifications**: Simple topic broadcast system for "New featured content available" (can be simulated).
- **Advanced Observability**: Sentry or similar crash reporting, log correlation IDs between app and API.
- **Advanced Caching**: Redis implementation with detailed TTL strategy.

## Stretch Goals

These advanced features demonstrate senior-level capabilities and are optional but highly valued:

### Mobile Native Integrations

- **Push Notifications**: Backend topic broadcast with frontend handling (Expo Notifications or FCM/APNs).
- **Background Tasks**: Background fetch for "today's" content with scheduling, backoff, and OS constraint respect.
- **Native Device Features**: Biometric lock for Bookmarks screen, or save images to media library with proper permissions.

### DevOps & Production Readiness

- **Observability End-to-End**: Sentry (or similar) crash reporting wired to both app and API with log correlation IDs.
- **CI/CD Pipeline**: GitHub Actions for type-check, lint, unit tests, e2e tests (API), optional Detox, and build artifacts.
- **EAS Build**: If using Expo, set up EAS build profile and document OTA update strategy.
- **App Store Ready**: Build configs, app icons, splash screens, release notes template, and store requirements checklist.

### Native Module Development

- Create a custom native module that extends React Native capabilities beyond JavaScript bridge.

## Leadership & Delivery Artifacts

These documents demonstrate your senior-level thinking and communication skills:

### Architecture Decision Record (ADR)

Create an ADR titled "Offline-first strategy for WikiDaily" that includes:
- Alternatives considered (e.g., React Query only vs. normalized cache vs. SQLite).
- Chosen approach with detailed rationale.
- Trade-offs and future evolution considerations.

### PR Workflow

Submit your work as at least 2 PRs into your own repository:
- **PR #1**: "Vertical slice: feed + API integration"
- **PR #2**: "Offline + tests + polish"

Use meaningful PR descriptions, checklists, and small atomic commits that tell a story.

### Code Review Exercise

Create a `CODE-REVIEW.md` document where you critique one of your own PRs as if reviewing a teammate's work. Call out:
- Potential risks
- Test coverage gaps
- Naming and code organization
- Performance considerations
- Accessibility concerns
- Maintainability issues

### Delivery Plan

Create a `DELIVERY-PLAN.md` document: "From Zero to First Release in 2 weeks with a 2-person team"
- Define clear milestones
- Identify risks and mitigation strategies
- Establish cut-lines (what features to drop first under time pressure)

### Runbook

Create a `RUNBOOK.md` for backend operability that documents:
- Environment variables and configuration
- Health check endpoints and expected responses
- Rate limiting rationale and configuration
- Cache TTL decisions and reasoning
- How to manage API keys and secrets in production

## Evaluation Criteria

The project will be scored over **300 total points** (+10 bonus), distributed across three main categories. Projects scoring over 270 points typically demonstrate lead-level mastery.

### Mobile App (110 points)

| Criteria | Points |
|----------|--------|
| Code quality & architecture | 20 |
| UX/UI polish | 10 |
| Accessibility (labels, Dynamic Type, contrast, VoiceOver/TalkBack) | 10 |
| Offline-first & data strategy (cache, SWR pattern, graceful offline states) | 25 |
| Performance (FlatList hygiene, image caching, perf budget documentation) | 15 |
| Tests (unit + one Detox happy-path) | 20 |
| Runner docs (setup, scripts, troubleshooting) | 10 |

### Backend API (110 points)

| Criteria | Points |
|----------|--------|
| Code quality & structure | 15 |
| API design & validation | 15 |
| Caching, versioning, and rate-limiting | 20 |
| Observability & health checks | 15 |
| Security & configuration hygiene (.env.example, validation, no secrets) | 10 |
| Tests (unit + e2e with cache header assertions) | 25 |
| Packaging & DX (Dockerfile, docker-compose, fast local run) | 10 |

### Leadership & Delivery (80 points)

| Criteria | Points |
|----------|--------|
| ADR quality (focus on offline strategy) | 20 |
| PR hygiene & commit discipline | 15 |
| Code-review critique (risk assessment, tests, performance, accessibility) | 15 |
| Delivery plan realism (milestones, risks, cut lines) | 15 |
| Cross-cutting DX (Makefile, CI checks, bootstrap speed) | 15 |

### Bonus Points (up to +10)

Optional stretch goals unlock bonus points only if each core section scores ≥70%:
- Push notifications implementation
- Sentry integration
- CI pipeline with Detox
- Native module development
- Complete app store packages

**Total: 300 points (+10 bonus)**

## Quality Standards

All submissions must meet these baseline standards:

- **TypeScript**: Strict mode enabled everywhere.
- **Code Style**: ESLint + Prettier configs committed to repository.
- **Environment Management**: `.env.example` with safe defaults; no secrets in code.
- **Mobile Optimization**: Demonstrate payload trimming/reshaping for mobile (fields, pagination, TTL).
- **Error Handling**: User-friendly messages on mobile; structured responses on API.
- **Accessibility**: Include a checklist of what you verified (VoiceOver, Dynamic Type, color contrast, etc.).

## Submission Guidelines

### Repository Requirements

- Upload the project to a **public GitHub repository** and share the URL.
- Structure as monorepo using `apps/` or `packages/` if desired (Turborepo/Nx optional).

### Documentation

Include the following files in your repository:

- **README.md**: Setup instructions for both mobile and API, scripts, assumptions, and project overview.
- **ADR.md**: Architecture Decision Record for offline strategy.
- **CODE-REVIEW.md**: Self-critique of one of your PRs.
- **DELIVERY-PLAN.md**: Two-week delivery plan with milestones and risks.
- **RUNBOOK.md**: Backend operational documentation.

### Demos

- Include screenshots or a short video demonstrating the app on both iOS and Android (simulators are acceptable).
- Show key user flows: date selection, content loading, offline behavior, and sharing.

### Submission Timing

- Submit whatever you accomplished by the deadline specified in the email.
- **Important**: Do not introduce any commits after the submission timestamp you provide in your PR description.
- Late submissions will be immediately disqualified.

### Privacy Note

To keep your submission private:
- **DO NOT** fork this repository
- **DO NOT** use this file content verbatim
- **DO NOT** mention the company name in your README

## Success Criteria

We evaluate submissions based on these key questions:

- **Launch experience**: Does the app provide immediate clarity with fast list rendering and no jank?
- **Offline resilience**: Toggle Airplane Mode—is the app still useful? Does it recover gracefully?
- **Responsiveness**: Change date & language—is the response quick with appropriate caching?
- **Native integration**: Does sharing work correctly with the native share sheet?
- **API design**: Does the API return just enough data with proper caching headers and consistent errors?
- **Test quality**: Do tests actually catch regressions? (Try breaking something to verify a test fails.)
- **Developer experience**: Can someone run the project in under 10 minutes?

---

Good luck! We're excited to see your approach to building production-ready mobile applications. Remember, we value quality over quantity—a well-executed core implementation is better than incomplete stretch goals.
