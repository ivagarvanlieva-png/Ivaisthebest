# App Plan — iService

## 1. App Overview

iService is a two-sided service marketplace for Madrid that connects people who need a task done with verified people who can do it. A client posts a job — a delivery, an errand, a repair, anything a person can do — and providers accept it; or the client browses verified providers and hires one directly. Both flows ship in v1. The platform takes a percentage commission on every completed job. The voice is service-oriented and practical: the app exists to get things done, day or night.

## 2. Key Components

Core:
- Email + password authentication with two roles on one account: client and provider (a user can be both).
- Job posting: title, description, category, location (Madrid district), budget in EUR, urgency (now / today / this week).
- Job feed for providers: browse open jobs, filter by category and district, accept a job.
- Provider profiles: services offered, short bio, availability text, hourly or per-job rate, average rating.
- Direct hire: client browses provider list, opens a profile, sends a hire request tied to a job.
- In-app messaging: one thread per job between the client and the accepted/requested provider.
- Job status lifecycle: open → accepted → in progress → completed → reviewed.
- Payment on completion via Stripe Checkout, with the 5% commission calculated and recorded.
- Two-way ratings: 1–5 stars plus a comment, client rates provider and provider rates client after completion.

Additional:
- Provider verification badge, granted by an admin after phone confirmation and ID document upload.
- Admin review screen to approve or reject pending provider verifications.
- Earnings summary for providers (completed jobs, gross, commission, net).

## 3. App Structure

Screens:
- **Landing** — what iService is, sign-up call to action.
- **Auth** — sign up / log in.
- **Home / Job feed** — providers see open jobs; clients see a "Post a job" button and their active jobs.
- **Post a job** — form to create a job.
- **Job detail** — full job info, status, action button (accept / mark complete / pay), link to message thread.
- **Browse providers** — grid of verified providers with filters.
- **Provider profile** — public profile with rating and a "Hire for a job" action.
- **Messages** — thread list and individual job thread.
- **My account** — profile edit, verification status/upload, earnings (if provider).
- **Admin** — verification queue (restricted to admin role).

Navigation flow:
- Landing → Auth → Home.
- Home → Post a job → Job detail (on submit).
- Home (provider) → Job detail → accept → status becomes accepted → Messages thread opens.
- Browse providers → Provider profile → Hire → creates a job in "accepted" state → Messages.
- Job detail → mark complete (provider) → client sees Pay button → Stripe Checkout → status completed → rating prompt.
- My account → upload ID → status "pending" → Admin approves → badge appears.

## 4. User Interface

- **Top nav (fixed):** iService wordmark left, centered links (Jobs, Providers, Messages), avatar menu right with My account / Log out. On mobile, links collapse into the avatar menu.
- **Job feed:** single column of job cards. Each card: title, category chip, district, budget (bold, right-aligned), urgency tag, "View" button. Filter bar above: category dropdown, district dropdown.
- **Post a job:** stacked form — title text input, description textarea, category select, district select, budget number input (EUR prefix), urgency radio group, "Post job" primary button.
- **Job detail:** header with title and status pill (color-coded: open grey, accepted blue, in progress amber, completed green); body with description, budget, location, client/provider names; right rail with the primary action button and "Open messages" link.
- **Browse providers:** responsive grid of provider cards — avatar, name, top service, star rating, rate. Click opens profile.
- **Provider profile:** avatar and name with verified badge, bio, services list, rate, rating breakdown, "Hire for a job" button.
- **Messages:** two-pane on desktop (thread list left, conversation right), single pane on mobile. Message bubbles, text input fixed at bottom.
- **My account:** profile form, verification card (upload control + status pill), earnings table for providers.

## 5. Backend Requirements

Backend required. Use Supabase (Lovable default).

Entities:
- **profiles** — id (fk auth.users), full_name, phone, is_provider (bool), bio, services (text[]), rate_eur, district, verification_status (enum: none/pending/verified), avatar_url, is_admin (bool).
- **jobs** — id, client_id (fk profiles), provider_id (fk profiles, nullable), title, description, category, district, budget_eur, urgency, status (enum), created_at.
- **messages** — id, job_id (fk jobs), sender_id (fk profiles), body, created_at.
- **reviews** — id, job_id, rater_id, ratee_id, stars (int 1–5), comment, created_at.
- **payments** — id, job_id, amount_eur, commission_eur, stripe_session_id, status, created_at.

Storage: Supabase Storage bucket `id-documents` (private) for verification uploads; `avatars` (public) for profile images.

Auth: Supabase email/password. Row Level Security so users read/write only their own jobs, threads, and reviews; admin role bypasses for the verification queue.

## 6. APIs and Libraries

- **Supabase** — auth, Postgres, storage, realtime (use realtime subscriptions for the messages thread so new messages appear without refresh).
- **Stripe Checkout** — one-time payment per completed job; commission recorded server-side via a Supabase edge function that creates the Checkout session and writes the payments row on success webhook.
- **Built-in to Lovable:** React, Tailwind, and shadcn/ui components ship by default — use them for nav, cards, dialogs, forms; do not add a separate component library.

## 7. Testing Strategy

- **Unit:** commission calculation (5% of budget, rounded to cents); job status transition guard (only valid next-states allowed); RLS policy checks (a user cannot read another user's thread).
- **Integration:** full post → accept → message → complete → pay → review loop for one job; direct-hire path produces an accepted job correctly; admin approval flips verification_status and surfaces the badge.
- **User acceptance:**
  - A client posts a job and a provider accepts it within the same session.
  - The two exchange messages in real time.
  - Provider marks complete, client pays via Stripe test card, payment row records gross + 5% commission.
  - Both leave ratings and the averages update on the profiles.
  - An unverified provider cannot be granted the badge without admin approval.

## 8. Platform-Specific Considerations (Lovable)

- Connect Supabase first; let Lovable generate typed tables from the schema in section 5, then add RLS policies before building UI.
- Use Supabase edge functions for the Stripe session creation and webhook — never call Stripe with secret keys from the client.
- Lean on shadcn/ui defaults (Card, Dialog, Badge, Tabs, Form) so the build stays consistent and fast.
- Use Supabase realtime for messages rather than polling.
- Keep all currency in cents (integers) in the database; format to EUR in the UI.
- Spanish-market detail: districts as a fixed Madrid list; EUR throughout; copy in clear English for v1 (Spanish localization is out of scope).

## 9. Out of Scope for v1

- Automated identity verification (Stripe Identity / Onfido) and selfie-to-ID match — v1 uses manual admin approval of an uploaded document.
- Stripe Connect payouts to provider bank accounts — v1 records the transaction and commission only.
- Native mobile apps.
- Dispute resolution, refunds, and partial payments.
- Provider bidding on a client-set budget.
- Background checks.
- Multi-city support and Spanish localization.
- Push notifications.

## 10. Definition of Done

- A new user can sign up, and set themselves as a provider with a profile.
- A client can post a job and see it appear in the feed.
- A provider can accept an open job, or be hired directly from their profile.
- Client and provider can message in one thread per job, updating in real time.
- A job moves open → accepted → completed, and the client pays via Stripe test checkout.
- The payment record stores gross amount and the 5% commission correctly.
- Both parties can leave a 1–5 star review and profile averages reflect it.
- An admin can approve a pending provider, granting the verified badge.
