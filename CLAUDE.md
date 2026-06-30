# iService — Project Brief

## What This Is

iService is a two-sided service marketplace that connects people who need any kind of task done with people who can do it. A client can post a job (e.g. "go to this shop, buy X, deliver it to me") and a provider accepts it — or a client can browse available providers and hire directly. Both flows are supported.

Launching in **Madrid**, web app first.

## Business Model

- Commission-based: iService takes a percentage cut from every completed transaction.
- Commission rate: TBD (example used: 5%).
- Providers receive payouts minus the platform commission.

## Users

**Clients**
- Age: 20–60, male and female
- Need: get any kind of task or service done, on demand, any time of day
- They post jobs or browse providers

**Providers**
- People who offer services of any kind through the platform
- Must be verified before they can accept jobs (see Verification below)
- They browse open jobs or make themselves discoverable to clients

## Core Features

### For Clients
- Post a job (title, description, location, budget, urgency)
- Browse available verified providers
- Hire a provider directly
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
- Commission deduction on every transaction
- Dispute resolution flow (TBD)
- Admin dashboard for verification review and moderation

## Provider Verification Flow

Providers must pass verification before accepting paid jobs:

1. **Phone number** — SMS code confirmation
2. **Government ID** — DNI, NIE, or passport upload (automated via Stripe Identity or Onfido)
3. **Selfie match** — face matched to ID document
4. **Bank account link** — required to receive payouts (via Stripe Connect or similar)
5. **Ratings** — reputation builds after each completed job; acts as ongoing quality filter

No background checks at MVP stage (GDPR complexity in Spain; revisit at scale).

## Tech Stack

Not yet decided. Starting from scratch. Likely direction: web app (mobile-responsive). All major decisions are open.

## Brand Voice

Service-oriented. Clear, practical, reliable. The app exists to get things done.

## Geography

Madrid, Spain — initial launch market. Expand later.

## Brand Name

**iService** (working name confirmed). Previous name "Slave.com" was considered and discarded — incompatible with app stores, payment processors, and domain registrars.

## Open Questions

- [ ] Commission rate (exact %)
- [ ] Tech stack selection (frontend framework, backend language, database, cloud provider)
- [ ] How disputes between clients and providers are handled
- [ ] Whether providers can set their own rates or clients set the budget and providers bid
- [ ] Minimum/maximum job value
- [ ] How to handle jobs that go wrong (refunds, partial payment, etc.)
- [ ] GDPR compliance plan for storing ID documents
- [ ] Whether to support cash payment or strictly digital

## What NOT to Build at MVP

- Native mobile apps (web first)
- Background checks
- Complex pricing algorithms
- Multi-city support
