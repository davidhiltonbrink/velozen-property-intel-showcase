# velozen-property-intel-showcase

**Address in, full property intelligence out.**

An AI-powered property research platform for real estate wholesalers, investors, and agents. Enter an address and get the property profile, owner information, comparable properties, investment analysis, and AI-generated outreach in seconds.

> 🔒 **This is a showcase repository.** Velozen Property Intelligence is a proprietary product currently in a pilot with a real estate investment client, so the source code is private. This repo describes what the platform does, how it's built, and my role in building it. I'm happy to walk through the product live on request.

---

## The Problem

Wholesalers and investors spend hours piecing together a single property: records from one site, ownership from another, comps from a third, and then writing outreach by hand. Consumer sites like Zillow show the house but not the person who owns it, which is the one thing a wholesaler actually needs to act.

## What It Does

```
Address → Property Record + Owner + Comps → Investment Analysis → AI Outreach → CRM Pipeline
```

- **Property profile:** beds, baths, square footage, year built, taxes, assessed value, automated valuation, and rent estimate, from live licensed data
- **Ownership panel:** owner names, mailing address, ownership type (individual, LLC, trust), and absentee-owner detection, placed first on the page because it's the information consumer sites don't show
- **Comparable properties:** live comps with neutral labeling, so listing data is never misrepresented as confirmed sales
- **Investment analysis:** after-repair value, maximum allowable offer (70% rule), ROI, and a composite wholesale score, all clearly labeled as estimates
- **AI photo condition analysis:** upload a property photo and a vision model returns a condition score, repair categories, and distress signals
- **AI outreach generator:** personalized SMS, email, direct-mail letter, and cold-call script for each property and owner, copy-only by design
- **Owner contact enrichment:** on-demand phone and email lookup through a skip-tracing provider
- **Web-search cross-check:** an optional second source that checks property facts against public listing sites and flags disagreements with the licensed data
- **CRM pipeline:** deal stages on a Kanban board, follow-up tasks, a per-lead detail page, a buyers list, and CSV export

## Tech Stack

| Layer | Tools |
|---|---|
| Framework | Next.js (App Router), TypeScript |
| UI | Tailwind CSS, shadcn/ui |
| Database & Auth | Supabase (PostgreSQL) |
| Property data | RentCast API |
| Geocoding & imagery | Google Maps (Geocoding, Places, Street View) |
| Contact enrichment | BatchData API |
| AI | Anthropic Claude (vision, structured output, web search) |
| Testing | Vitest, Playwright |
| Deployment | Vercel |

## My Role

I'm a founding developer on a two-person team. I built:

- The core address lookup flow and property dashboard, integrating the property data and geocoding APIs
- The investment analysis model
- AI photo condition analysis and the outreach generator
- Owner contact enrichment
- The web-search cross-check feature
- The CRM pipeline upgrade (deal stages, Kanban board, tasks, buyers list)

My co-founder built the authentication and multi-tenant architecture and the data-confidence scoring engine.

## Engineering Highlights

**Catching a formula flaw by checking the math, not just the code.** In the first version of the wholesale score, one component was algebraically constant: under the 70% rule it reduced to 30% of ARV no matter what the repair cost was, so every property scored the same on it. Type checks and builds passed. I caught it by hand-checking the formula against a real property, then replaced it with a component that actually varies.

**Live verification over assumed correctness.** Every feature was verified against real API responses and a running browser session before shipping, not just unit tests. One example: the web-search cross-check passed typecheck, build, lint, and tests on the first try, but a live call showed that a single blocked domain in the search allowlist failed the entire request. That bug would have broken the feature in production.

**Paid calls are opt-in.** Contact enrichment and web-search corroboration cost money per call, so they run only when a user clicks for them. The default property page stays fast and costs nothing extra.

**Compliance by design.** Outreach is generated and copied, never sent automatically. Automated SMS and calls trigger TCPA and A2P 10DLC requirements, so the product helps users write messages without sending on their behalf.

**Honest data labeling.** The provider's "comparables" are listing-based, not confirmed closed sales, so the UI says "comparables" and never "sold." Where the data can't support a claim, the product doesn't make it.

## Screenshots

<!-- Replace with real screenshots. Blur or avoid real owner names and mailing addresses. -->

| Property dashboard | Investment analysis |
|---|---|
| ![Property dashboard](screenshots/dashboard.png) | ![Investment analysis](screenshots/investment.png) |

| AI outreach | CRM pipeline |
|---|---|
| ![Outreach generator](screenshots/outreach.png) | ![Pipeline](screenshots/pipeline.png) |

## Status

Live since September 2026, currently in a client pilot and in active development.

## Contact

David Brink · David.Brink@jacks.sdstate.edu
