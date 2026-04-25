# The Ofline Co. — PRD

## Original Problem Statement
A premium 48-hour offline experience brand for high-performing individuals. Tagline: "Log out. Show up." Core ritual: hand over smartphone, receive button phone. Cohorts of 12–15, ₹12k–18k. Application-based, location revealed 24h before. Cinematic, philosophical brand voice. Dark navy + amber palette, Playfair Display + Inter.

## User Personas
- **The Burned-Out Operator** (28–40): founder, exec, designer who scrolls more than they speak.
- **The Quietly Curious** (25–45): senior IC who wants to participate, not consume.
- **The Studio Admin**: curates cohorts, runs experiences, controls countdown.

## Core Requirements (static)
- Cinematic landing with full-bleed hero
- Anonymous, multi-step application form
- Live countdown to next reveal (configurable from admin)
- Experience preview pages (CMS-driven)
- Admin dashboard: applications, experiences, countdown, testimonials
- Razorpay reservation flow (deferred — keys not provided yet)
- Mobile-first, dark theme with grain & custom cursor

## Implemented (2025-12)
- **Backend (FastAPI + MongoDB)**:
  - Auth: JWT bearer, bcrypt, idempotent admin seed (`admin@theofflineco.com` / `Offline@2025`)
  - Public: `/api/health`, `/api/applications`, `/api/experiences[/{slug}]`, `/api/countdown`, `/api/testimonials`
  - Admin: applications CRUD/status, experiences CRUD, countdown PUT, testimonials CRUD
  - Razorpay: order create + signature verify (returns 503 until `RAZORPAY_KEY_ID/SECRET` set)
  - Seed data: 2 sample experiences, 3 testimonials, 30-day default countdown
  - Mongo: indexes on `users.email`, `experiences.slug`, `id` everywhere; `_id` excluded from all responses
- **Frontend (React + Tailwind + shadcn + framer-motion)**:
  - Landing: Hero (parallax) → Problem → Concept Reveal (phone ritual) → Experience cards → Countdown → Testimonials → Differentiation → How It Works → Pricing → Final CTA → Footer
  - Apply: 5-step editorial form with progress bar, validation, success state
  - Experience preview page: chapters, sticky reserve card
  - Admin: tabs for Applications, Experiences (dialog CRUD), Countdown (calendar + time), Testimonials
  - Global grain overlay, custom amber cursor with spring physics, sonner toasts
- **Testing**: 18/18 backend pytest, full frontend smoke (testing_agent_v3 iter_1) — 100% pass

## Backlog
**P0**
- Razorpay keys + end-to-end paid booking flow (test card walkthrough once keys arrive)

**P1**
- Email triggers on application submit + selection (Resend or SendGrid)
- "Selected" automated address-reveal email 24h before `next_reveal_at`
- Public testimonials submission (post-experience storytelling)

**P2**
- Invite-only referral codes (each participant gets 2 invites)
- Mystery social share asset (no-photo participant cards)
- Admin: bookings table view (currently endpoint exists, no UI)
- Add `aria-describedby` / DialogDescription to admin Dialog for a11y polish
- Split server.py into routers/auth/seed modules as it grows

## Next Tasks
1. Provide Razorpay test keys → enable real reservations
2. Hook applications + selection → automated email flow
3. Build "Bookings" admin tab (list + status)

## Session Notes (2026-04-25)
- User confirmed scope: **(b) Email flow** + **(d) Polish all areas** (hero, concept, pricing, application form UX, admin dashboard).
- Email scope: ALL emails — application received, accepted/rejected, payment receipt + experience details.
- Pending decisions before implementation: (1) email provider (Resend vs SendGrid), (2) sender identity (sandbox vs custom domain), (3) RESEND_API_KEY / SENDGRID_API_KEY.
- User asked to **pause** and create a backup file; further instructions will come on the next run.
- Backup snapshot: `/app/backups/ofline_co_backup_20260425_104401.tar.gz` (backend + frontend + memory + design_guidelines + test_reports, excludes node_modules/.git).
