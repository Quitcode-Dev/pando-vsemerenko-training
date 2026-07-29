# Project Charter: Pando

**Document Version:** 1.0
**Date:** [Insert Date]
**Prepared by:** QuitCode
**Status:** Draft — Pending Client Review

---

## Executive Summary

Pando is a greenfield SMS-first local parenting trust network built for a hyperlocal community audience. Parents save a single US phone number as "Pando" in their contacts and text it naturally to receive trusted, community-backed answers about local activities, classes, camps, and caregivers. Unlike generic review platforms or AI assistants, Pando's core asset is a **structured human-truth graph** — combining real parent knowledge with freshness labels, provenance tracking, and two-layer matching logic (social affinity + life relevance).

The build is structured across two sequential phases:
- **Phase 1 — Seed Tool:** A mobile-first chat web application to ingest high-quality recommendations and contributor profiles from approximately 350 curated founding parents ahead of launch.
- **Phase 2 — SMS Pilot:** The live product, built in slices after Phase 1 data quality has been reviewed, enabling parents to text Pando and receive trusted, labeled, community-backed answers.

The initial market is **Pasadena, California**. The total estimated build effort is approximately **204 hours** across both phases.

---

## Project Sponsor & Stakeholders

| Role | Name | Contact | Interest / Responsibility |
|---|---|---|---|
| **Project Sponsor / Client** | Pando | [TBD] | Funding, final product decisions, business direction |
| **Product Owner** | Janet | janet@tweekle.com | Product vision, content decisions, admin oversight, caregiver consent workflows |
| **Project Manager (QuitCode)** | [TBD] | [TBD] | Delivery management, timeline, scope, risk |
| **Technical Lead (QuitCode)** | [TBD] | [TBD] | Architecture, engineering decisions, API integrations |
| **Founding Contributors** | ~350 curated local parents | Via SMS / invite link | Primary data source; seed the human-truth layer |
| **End Users (Phase 2)** | Local parents (Pasadena) | Via SMS | Query the network; pay for Blast tiers |
| **Admin User(s)** | Pando team | [TBD — number of admin users to be confirmed] | Review submissions, manage caregiver consent, quality control, Blast fulfillment |

> **Note:** The number of admin users requires confirmation. If more than one person will review submissions or manage caregiver consent workflows, individual admin logins must be configured from the start so every sensitive action is fully traceable.

---

## Business Case

Parents regularly make high-stakes local decisions — which activities are worth attending, which camps are genuinely good, which babysitter is trustworthy — and the best answers to these questions almost never exist in any searchable, structured form. They live in private text threads, WhatsApp groups, and people's heads. They are scattered, ephemeral, and rapidly go stale.

Existing solutions fail to solve this:
- **Google and Yelp** surface public listings but cannot confirm whether a teacher recently changed, whether a class suits a shy toddler, or whether a recommendation came from someone whose life is relevant to the person asking.
- **Care.com and Nextdoor** offer broad reach but no provenance, freshness, or relevance matching.
- **Generic AI assistants** synthesize public information but have no access to real community knowledge and cannot signal trust.

Pando's opportunity is to be the **trusted local friend at scale** — capturing human knowledge with its source, structuring it, keeping it fresh, and delivering it through the lowest-friction interface available: a standard text message.

The long-term defensible asset is not the SMS interface itself. It is the **structured trust and freshness graph** that grows more valuable with every contribution, confirmation, and thanks signal the network generates. The matching logic — routing questions to parents whose social world and lived context are genuinely relevant to the person asking — is the reason the paid tier is worth paying for and the reason no generic platform can replicate it.

---

## Project Goals & Objectives

### Primary Goals

1. **Build and validate the human-truth ingestion layer** (Phase 1) by collecting structured, high-quality recommendations and contributor profiles from approximately 350 founding parents in Pasadena before the SMS product launches.

2. **Deliver a working SMS pilot** (Phase 2) that allows parents to text Pando, receive labeled and trustworthy answers, and trigger paid community outreach (Blasts) when needed.

3. **Prove the core loop:** Contributors share knowledge → parents ask questions → answers are matched, labeled, and delivered → thanks flow back to contributors → contributors remain engaged and continue contributing.

### Measurable Objectives

| Objective | Target |
|---|---|
| Founding contributors completing Phase 1 onboarding | High completion rate of invited ~350 contributors [specific % TBD] |
| Contributors submitting 2+ recommendations | [TBD — to be set by Pando team] |
| Usable activity recommendations collected | [TBD] |
| Caregiver nominations collected | [TBD] |
| Contributors opted into Blasts | [TBD] |
| Phase 2 repeat query rate per user | [TBD] |
| Blast response rate | Target above 25% per contributor |
| Paid Blast conversion | [TBD] |
| Contributor retention over pilot period | [TBD] |

---

## Scope

### In Scope

**Phase 1 — Seed Tool (P0)**
- Mobile-first chat web interface for founding contributors (no app download required)
- Tap-first Social Affinity + Life Relevance onboarding profile questionnaire
- Structured activity and class recommendation capture via conversational flow
- Caregiver nomination capture (first name + last initial only; consent workflow separate)
- Caregiver consent outreach workflow (human-managed; no auto-activation)
- Admin dashboard for Phase 1: contributor management, activity and caregiver review, consent status tracking, conversation transcript review, approval/rejection, notes
- Pasadena market taxonomy seeded into `market_options`: neighborhoods, schools, classes, parent groups, clubs, faith/community groups
- Invite link mechanism and/or phone capture for contributor access [access method TBD — see Open Questions]

**Phase 2 — SMS Pilot (P0)**
- Twilio SMS integration (one US number, provisioned by Pando/Twilio)
- Phone-number-as-identity; no usernames or passwords
- SMS OTP authentication for web sessions
- Basic intent classification (Claude API)
- One clarifying question flow before answering
- Answer retrieval with correct trust and freshness labels (activity and caregiver)
- Caregiver answer retrieval with hard consent filter (`consent_status = consented AND active = true`) enforced at query level
- Basic Blast workflow (all four paid tiers: Standard, Targeted, Precision, Last Minute Care)
- Stripe payment integration for Blast purchases
- Two-layer matching engine (social affinity scoring + life-relevance modifiers)
- Contributor frequency caps and 48-hour gap enforcement (in code, not manual)
- Human review queue for sensitive, low-confidence, or caregiver-related answers
- Thanks mechanism (prompt, delivery, batching)
- Basic contributor tracking and tier calculation
- Admin dashboard for Phase 2: SMS inbox, conversation history, answer queue, Blast manager, contributor profiles, frequency limits, payment/refund flags, caregiver consent workflow, quality scoring, thanks/impact tracking, pending-options review
- Scheduled background jobs (thanks dispatch, tier recalculation, Blast expiry, response rate monitoring)
- Security hardening: Twilio and Stripe webhook signature validation, OTP expiry and rate limiting, row-level security, sanitized inputs, no PII in logs

**Infrastructure (Both Phases)**
- Next.js 14 (App Router) — full-stack framework
- Supabase (PostgreSQL) — primary database with row-level security
- Vercel — hosting
- Upstash Redis — OTP and session caching
- Tailwind CSS — styling
- All Blast prices and matching score weights configurable via environment variables or config table (no code deploy required to change)

---

### Out of Scope

The following are explicitly **not part of P0** and are deferred to P1 or later:

- Reference Connections (paid direct-intro workflow between parents and contributors) — P1
- Subscription / recurring billing model
- Marketplace features
- Caregiver placement or success fees
- Partner perks or any sponsored answer mechanism
- Multi-market support (Pando launches in Pasadena only; multi-timezone and multi-market logic deferred)
- Native mobile application (iOS/Android)
- Public-facing contributor leaderboard or competitive scoring
- Background check integration or caregiver vetting services
- Automatic Blast refunds (manual admin action for first ~60 days; automation deferred until fulfillment rules are proven)
- Anti-bot / human-verification mechanisms beyond OTP [flagged as an open consideration — see Risks]
- Any commercial promotion, advertising, or sponsored answer placement

---

## Key Deliverables

| # | Deliverable | Phase | Description |
|---|---|---|---|
| 1 | Phase 1 Seed Tool | Phase 1 | Mobile-first chat web app for founding contributor onboarding, profile capture, and recommendation ingestion |
| 2 | Tap-First Onboarding Questionnaire | Phase 1 | Social Affinity + Life Relevance profile with pre-loaded Pasadena taxonomy |
| 3 | Caregiver Nomination & Consent Workflow | Phase 1 | Capture pipeline with human-managed consent outreach; no auto-activation |
| 4 | Phase 1 Admin Dashboard | Phase 1 | Contributor, activity, and caregiver review tools with conversation transcripts |
| 5 | Pasadena Market Taxonomy | Phase 1 | Seeded `market_options` dataset for neighborhoods, schools, classes, groups |
| 6 | SMS Inbound & Identity Layer | Phase 2 | Twilio webhook, phone-as-identity, OTP web auth |
| 7 | Intent Classification & Clarifying Step | Phase 2 | Claude-powered intent routing and single clarifying question flow |
| 8 | Answer Retrieval & Trust Label System | Phase 2 | Labeled responses with correct provenance, freshness, and trust signals |
| 9 | Two-Layer Matching Engine | Phase 2 | Social affinity scoring + life-relevance modifier logic for Blast targeting |
| 10 | Blast System & Stripe Integration | Phase 2 | All four paid tiers, payment flow, contributor selection, admin fulfillment tools |
| 11 | Contributor Frequency & Protection System | Phase 2 | Frequency caps, 48-hour gap enforcement, auto-tier reduction on low response rates |
| 12 | Thanks & Contributor Rewards System | Phase 2 | Thanks prompt, delivery, batching, and access-tier progression |
| 13 | Phase 2 Admin Dashboard | Phase 2 | Full operational dashboard including Blast management, consent workflow, quality scoring |
| 14 | Scheduled Background Jobs | Phase 2 | Thanks dispatch, tier recalculation, Blast expiry, response rate monitoring |
| 15 | Security Hardening & Deployment | Phase 2 | Webhook validation, RLS, OTP controls, staging and production deploy |

---

## High-Level Timeline

> **Note:** The following timeline is based on estimated hours from the engineering specification (~204 hours total). Specific calendar dates depend on team composition, start date, and sprint velocity — all of which are **[TBD — to be confirmed with QuitCode team and client]**. The sequencing below is fixed: Phase 2 must not begin until Phase 1 has been tested with real contributors and data quality reviewed.

| Milestone | Estimated Effort | Sequence |
|---|---|---|
| **M1 — Project Kickoff** | — | Start |
| Finalize Seed Tool flow, Social Affinity chips, invite link approach, and Pasadena taxonomy | — | Pre-build |
| **M2 — Phase 1 Build Complete** | ~50 hours | After M1 |
| **M3 — Phase 1 Internal QA** | Included in Phase 1 hours | After M2 |
| **M4 — Phase 1 Live with Real Contributors (20–30 founding users)** | — | After M3 |
| **M5 — Data Quality Review & Phase 2 Spec Revision** | — | After M4 |
| **M6 — Phase 2 Core Build Complete** (SMS, identity, intent, answer retrieval) | ~48 hours | After M5 |
| **M7 — Phase 2 Features Complete** (Blast, matching, thanks, rewards) | ~64 hours | After M6 |
| **M8 — Phase 2 Admin Dashboard Complete** | ~22 hours | Parallel to M7 |
| **M9 — Security Hardening, Staging & Deploy** | ~20 hours | After M7/M8 |
| **M10 — Phase 2 SMS Pilot Live** | — | After M9 |
| **M11 — Post-Launch Review** (pilot metrics reviewed against success criteria) | — | ~4–6 weeks post-launch |

> **Critical path dependency:** Phase 2 build does not begin until Milestone 5 is complete and the Pando team has signed off on data quality from the real-contributor test.

---

## Budget & Resources

### Budget

| Item | Estimate |
|---|---|
| **Total Development Hours** | ~204 hours |
| **Development Rate** | [TBD — QuitCode to confirm] |
| **Total Development Cost** | [TBD] |
| **Infrastructure (Pilot Phase)** | Low — Vercel free tier, Supabase free tier, Upstash Redis free tier cover pilot scale |
| **Twilio SMS** | Pay-per-message; volume-dependent [TBD] |
| **Claude API (claude-sonnet-4-6)** | Usage-based [TBD — monitor during pilot] |
| **Stripe** | Standard transaction fees on Blast payments |
| **General Liability Insurance** | **[TO BE CONFIRMED]** — must be in place before caregiver matching goes live |
| **Contingency** | [TBD — recommend 15–20% of development budget] |

### Team Composition

| Role | Source | Notes |
|---|---|---|
| Engineering (Full-Stack) | QuitCode | Next.js, Supabase, Twilio, Claude API, Stripe |
| Project Management | QuitCode | [TBD — confirm PM assignment] |
| Product Owner | Pando (Janet) | Decisions on flow, content, taxonomy, admin processes |
| Admin / Content Review | Pando team | Caregiver consent, Blast fulfillment, quality review |
| Founding Contributors | Pando-curated | ~350 local parents; not a cost item but a critical resource |

> **Blast pricing for all four paid tiers is [TBD — Pando team to supply].** These will be configured as environment variables and can be adjusted at any time without a code deployment.

---

## Major Risks

| # | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| R1 | **Cold start / insufficient seed data** — Phase 2 launches with too few quality recommendations to be useful | Medium | High | Phase 1 Seed Tool with curated founding contributors; do not launch Phase 2 until data quality review confirms sufficient coverage |
| R2 | **Contributor fatigue** — founding contributors disengage from Blast participation over time | Medium | High | Frequency caps (3/5/10/20 per month), 48-hour gap enforcement, thanks loop, access-tier rewards; all enforced in code |
| R3 | **Caregiver liability** — incorrect, unconsented, or misleading caregiver information causes harm or legal exposure | Low | Critical | Hard consent filter at DB/API level (`consent_status = consented AND active = true`); human review queue; required disclaimer language on all caregiver answers; general liability insurance required before caregiver matching goes live |
| R4 | **Trust degradation** — public information presented as human trust, especially for caregivers | Medium | High | Mandatory answer labels on every response component; QC enforced in code and admin review; no label can overstate what the data shows |
| R5 | **Data quality issues from open-ended input** | Medium | Medium | Tap-first profile design; structured conversation flows; admin review and approval before data is used; pending_options queue for "