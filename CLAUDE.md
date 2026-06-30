# iService — Project Brief

## What This Is

iService is a two-sided service marketplace that connects people who need any kind of task done with people who can do it. A client can post a job (e.g. "go to this shop, buy X, deliver it to me") and a provider accepts it — or a client can browse available providers and hire one directly. Both flows are supported.

Launching in **Madrid**, web app first (mobile-responsive).

## Business Model

- Commission-based: iService takes a percentage cut from every completed transaction.
- **Commission rate: 10%** of each completed transaction.
- Providers receive payouts equal to the job price minus the 10% platform commission.

## Users

**Clients**
- Age: 20–60, male and female
- Need: get any kind of task or service done, on demand, any time of day
- They post jobs or browse providers

**Providers**
- People who offer services of any kind through the platform
- Must be verified before they can accept paid jobs (see Verification below)
- They browse open jobs or make themselves discoverable to clients
- **Providers set their own rates** (fixed and/or hourly); clients see the rate and pay it

## Core Features

### For Clients
- Post a job (title, description, location, budget, urgency)
- Browse available verified providers
- Hire a provider directly at their published rate
- Track job status
- Rate and review provider after completion
- Secure in-app payment

### For Providers
- Create a service profile (what they offer, availability, rate)
- Browse open jobs and apply/accept
- Get discovered by clients
- Track earnings and job history
- Rate and review client after completion
- Receive payouts to linked bank account

### Platform
- Two-sided marketplace matching engine
- In-app messaging between client and provider
- 10% commission deduction on every transaction
- Dispute resolution flow (TBD)
- Admin dashboard for verification review and moderation

## Pricing Model

- **Providers set their own rates.** Each provider publishes the price for their service (fixed or hourly).
- Clients see the rate up front and pay it — no bidding/auction at MVP.
- Posted jobs still carry a client budget, but the agreed price for a direct hire is the provider's published rate.

## Payments

- **Digital only at launch.** All payments are processed in-app (card). No cash.
- Digital-only keeps commission capture reliable and the transaction record clean.
- Money flow: client is charged in-app → funds held → on completion, provider is paid out their rate minus the 10% commission.

## Provider Verification Flow

Providers must pass verification before accepting paid jobs:

1. **Phone number** — SMS code confirmation
2. **Government ID** — DNI, NIE, or passport upload (automated via Stripe Identity or Onfido)
3. **Selfie match** — face matched to ID document
4. **Bank account link** — required to receive payouts (via Stripe Connect)
5. **Ratings** — reputation builds after each completed job; acts as an ongoing quality filter

No background checks at MVP stage (GDPR complexity in Spain; revisit at scale).

## Tech Stack (Recommended)

Web-first, mobile-responsive. Proposed stack optimized for shipping an MVP fast with payments and identity built in:

- **Frontend:** Next.js (React) + TypeScript, mobile-responsive UI
- **Backend:** Next.js API routes / Node.js (TypeScript) — single codebase to start
- **Database:** PostgreSQL (managed — e.g. Supabase or Neon)
- **Payments & payouts:** Stripe — Stripe Payments (client charges), Stripe Connect (provider payouts + commission split), Stripe Identity (ID/selfie verification)
- **SMS verification:** Twilio (phone confirmation)
- **Hosting:** Vercel (web) + managed Postgres
- **Auth:** email/password + phone, session-based (e.g. Auth.js)

Stripe is the backbone: it covers the in-app charge, the 10% commission split, provider payouts, and KYC/identity in one integration. This can be revisited, but it's the fastest path to a compliant, paying marketplace in Spain.

## Brand Voice

Service-oriented. Clear, practical, reliable. The app exists to get things done.

## Forbidden Words

None.

## Geography

Madrid, Spain — initial launch market. Expand later.

## Brand Name

**iService** (working name confirmed). Previous name "Slave.com" was considered and discarded — incompatible with app stores, payment processors, and domain registrars.

## Open Questions

- [ ] How disputes between clients and providers are handled (refunds, partial payment, jobs that go wrong)
- [ ] Minimum / maximum job value
- [ ] GDPR compliance plan for storing ID documents
- [ ] Exact payout timing (instant vs. held until rating window closes)
- [ ] Cancellation policy and fees

## What NOT to Build at MVP

- Native mobile apps (web first)
- Background checks
- Complex pricing algorithms / bidding
- Multi-city support
- Cash payments
