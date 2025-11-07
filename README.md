# Senior Mobile Team Lead — React Native + NestJS Challenge

## What we’re assessing

- **Hands-on excellence in React Native**: offline-first UX, native integrations, performance, testing.
- **Team lead skills**: architecture decisions, code review quality, delivery plan, PR hygiene, and documentation.
- **Backend for mobile**: NestJS APIs optimized for devices (latency, payloads, caching, versioning, resilience).

>  **Two lanes to succeed**
> - **Core (must-have):** a polished vertical slice with clean code, tests, docs.
> - **Stretch (nice-to-have):** advanced features that show seniority (observability, CI/CD, native modules, etc).

---

## Product Brief

Build **“WikiDaily”**, a mobile app that lets users explore Wikipedia Featured Content by date and language, with an **offline-first** experience and **shareable** cards. Ship a small but production-minded app.

### Core User Stories (Required)

1. **Pick Date & Language**
   - Platform-appropriate pickers (iOS/Android) for date and language.
   - Fetch featured content for the selected day.
   - Persist last choice.

2. **Feed of Cards**
   - Scrollable list of “featured” items (title, thumbnail, short extract).
   - Pull to refresh.
   - Mark items as “viewed” (visible indicator).
   - Tap to open a **Details** screen (more text, hero image if available).

3. **Share**
   - Native share sheet from a card and from Details.

4. **Offline-first**
   - View previously fetched days **without network**.
   - Graceful “you’re offline” states.
   - Background refresh when the app regains connectivity (if feasible).

5. **Performance & UX polish**
   - Image caching; avoid layout shift; fast list rendering for ~100 items.
   - Support **Dark Mode** and **Dynamic Type** (font scaling).

### Bonus User Stories (Pick any)

- **Bookmarks** (persisted).
- **Infinite scroll by day** (as the user reaches the end, load previous day, etc).
- **Responsive/adaptive** layouts (phones & small tablets, both orientations).
- **Deep links** (e.g., `wikidaily://item/{id}` opens Details).
- **Feature flags** (remotely flip “bookmarks” on/off — local mock OK).

---

## Mobile Requirements (React Native)

- **Stack**: React Native (TypeScript). You may use Expo **or** bare RN. If Expo, note where you’d eject for specific needs.
- **State & data**
  - Use **React Query / TanStack Query** or **Redux Thunk** (or equivalent) for network + caching.
  - Use on-device persistence (e.g., SQLite/Drizzle/WatermelonDB/Realm) for offline.
  - Avoid global heavy state where unnecessary; keep boundaries clear.
- **Navigation**: React Navigation with a minimum of two screens (Feed, Details). Deep links if you choose that bonus.
- **Accessibility**: accessible labels, hit slop, color contrast, Dynamic Type, VoiceOver/TalkBack sanity checks.
- **Testing**
  - **Unit** (Jest + React Testing Library) for critical components and hooks.
  - **E2E** (Detox) for at least: open app → load feed → open details → share (can be mocked).
- **Performance**
  - Use FlatList/SectionList correctly (keys, windowSize, getItemLayout where appropriate).
  - Image caching (e.g., `react-native-fast-image` or Expo equivalents).
  - Add a simple **perf budget** in the README (e.g., TTI target on mid-range Android).

> ✅ **Deliverable for leadership**: include **one ADR** (Architecture Decision Record) that explains your **offline strategy** and **state choice**, with considered alternatives.

---

## Backend Requirements (NestJS)

Build a small mobile-oriented API that **proxies** Wikipedia Featured Content and optionally **translates** fields via **LibreTranslate** (self-hosted or community instance; no paid keys).

### Endpoints

- `GET /v1/feed`
  - Proxies Featured Content.
  - Validates query (date, language, etc).
  - **Mobile optimization**: only return fields the app needs; optionally compress/transform images (you can simulate or document).
  - Cache responses (HTTP cache headers + server cache layer).
  - Structured error format.

- `GET /v1/feed/translate/:lang`
  - Same as `/v1/feed`, but translate `title` and `extract` into `:lang`.
  - Validate supported languages.

### Platform features

- **Versioning** (`/v1/...`) with an example of how `/v2` could evolve (can be stub + doc).
- **Rate limiting** (e.g., per IP).
- **Caching** (in-memory or Redis; document TTL choices).
- **Observability**:
  - Health check (`/healthz`).
  - Basic metrics (Prometheus or simple counters) **or** OpenTelemetry traces (bonus).
- **Docs**: Swagger
- **Tests**: unit tests for services/pipes/guards and at least one e2e test (supertest).
- **Packaging**: Dockerfile + `docker-compose.yml` for local run (Nest + optional Redis).

> ✅ **Deliverable for leadership**: include a **Runbook** (operability notes: env vars, health checks, rate limit rationale, cache TTL, how to roll keys).

---

## Stretch Goals (Advanced)

- **Push notifications**  
  - Backend: simple topic broadcast “New featured content available” (simulate).
  - App: receive & route to the selected day (Expo Notifications or FCM/APNs if bare).

- **Background tasks & sync**  
  - Background fetch for “today’s” content; schedule + backoff; respect OS constraints.

- **Native device features**  
  - Biometric lock for Bookmarks screen; or save-for-offline images to media library (ask permission).

- **Observability end-to-end**  
  - Sentry (or similar) crash reporting wired to both app and API.
  - Log correlation ID from app → API.

- **CI/CD**
  - GitHub Actions: type-check, lint, unit tests, e2e (API), Detox (optional), build artifacts.
  - If Expo: set up EAS build profile and document OTA update strategy.

- **App Store–ready**
  - Build configs, app icons, splash; release notes template; checklist of store requirements.

---

## Team Lead Dimensions (Required Artifacts)

1. **Architecture Decision Record (ADR)**  
   - Title: “Offline-first strategy for WikiDaily”.  
   - Alternatives considered (e.g., React Query only vs. normalized cache vs. SQLite).  
   - Chosen approach + trade-offs + future evolution.

2. **PR Workflow**  
   - Submit your work as **at least 2 PRs** into your own repo:
     - PR #1: “Vertical slice: feed + API integration”.
     - PR #2: “Offline + tests + polish”.
   - Use meaningful descriptions, checklists, and small commits.

3. **Code Review Exercise**  
   - Add a `code-review.md` where you critique **one** of your own PRs as if you were reviewing a teammate’s: call out risks, test gaps, naming, performance, a11y, and maintainability.

4. **Delivery Plan**  
   - `delivery-plan.md`: **From Zero to First Release** in 2 weeks with a 2-person team.  
   - Milestones, risks, and cut-lines (what you’d drop first).

---

## Scoring Breakdown (Total: 300 pts + 10 bonus)

| **Category**                          | **Criteria**                                                                                                  | **Points** |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ---------- |
| **Mobile App (110 pts)**              | Code quality & architecture                                                                                   | 20         |
|                                       | UX/UI polish                                                                                                  | 10         |
|                                       | Accessibility (labels, Dynamic Type, contrast, basic VO/TalkBack)                                             | 10         |
|                                       | Offline-first & data strategy (cache, SWR pattern, graceful offline states)                                   | 25         |
|                                       | Performance (FlatList hygiene, image caching, perf budget doc)                                                | 15         |
|                                       | Tests (unit + one Detox happy-path)                                                                           | 20         |
|                                       | Runner docs (setup, scripts, troubleshooting)                                                                 | 10         |
| **Backend API (110 pts)**             | Code quality & structure                                                                                      | 15         |
|                                       | API design & validation                                                                                       | 15         |
|                                       | Caching, versioning, and rate-limiting                                                                        | 20         |
|                                       | Observability & health checks                                                                                 | 15         |
|                                       | Security & configuration hygiene (.env.example, validation, no secrets)                                       | 10         |
|                                       | Tests (unit + e2e with cache header assertions)                                                               | 25         |
|                                       | Packaging & DX (Dockerfile, docker-compose, fast local run)                                                   | 10         |
| **Leadership & Delivery (80 pts)**    | ADR quality (focus on offline strategy)                                                                       | 20         |
|                                       | PR hygiene & commit discipline                                                                                | 15         |
|                                       | Code-review critique (risk, tests, perf, a11y)                                                                | 15         |
|                                       | Delivery plan realism (milestones, risks, cut lines)                                                          | 15         |
|                                       | Cross-cutting DX (Makefile, CI checks, bootstrap speed)                                                       | 15         |
| **Bonus (up to +10 pts)**             | Optional stretch goals (push notifications, Sentry, CI Detox, etc.) — unlocked only if each core section ≥70% | +10        |

**Total:** 300 pts (+10 bonus)


>  Projects scoring **>270** usually demonstrate lead-level mastery.

---

## Technical Guardrails & Tips

- **TypeScript everywhere** (strict mode on).
- **Linting/formatting**: ESLint + Prettier configs committed.
- **Env management**: `.env.example` + safe defaults; no secrets in code.
- **Mobile payload**: prove you trimmed/reshaped responses for the app (fields, pagination, TTL).
- **Error handling**: user-friendly on mobile; structured on API.
- **Accessibility**: include a short checklist of what you verified.

---

## Submission

- Public GitHub repository URL.
- Include:
  - `README.md` with setup (mobile + API), scripts, and assumptions.
  - `apps/` or `packages/` structure if monorepo (optional via Turborepo/Nx).
  - Screenshots or short video of iOS + Android (simulators are fine).
  - `ADR`, `code-review.md`, `delivery-plan.md`, and `RUNBOOK.md`.
- Timeboxing: submit **whatever you accomplished** by the deadline. No commits after the timestamp you share in the PR description.


---

## Evaluation Cheatsheet (what reviewers will look for)

- Launch app → immediate clarity, fast list, no jank.
- Toggle Airplane Mode → still useful; recover gracefully.
- Change date & language → quick response, cached where possible.
- Share from a card → native share opens correctly.
- API returns **just enough** data, with caching headers and consistent errors.
- Tests actually catch regressions (break one thing to see a test fail).
- Docs make it easy to run in <10 minutes.
