# Product Requirements Document: Pando

**Document Version:** 1.0
**Status:** Draft — Pending Client Review
**Prepared by:** QuitCode
**Product Owner:** Janet (janet@tweekle.com)
**Document Date:** [Insert Date]
**Review Date:** [TBD — to be scheduled with Pando team]

---

## Table of Contents

1. [Business Case](#business-case)
2. [Business Context & Goals](#business-context--goals)
3. [Current State Analysis](#current-state-analysis)
4. [Users & Stakeholders](#users--stakeholders)
5. [Desired Future State](#desired-future-state)
6. [Hypothesis](#hypothesis)
7. [Functional Requirements — Phase 1: Seed Tool](#functional-requirements--phase-1-seed-tool)
8. [Functional Requirements — Phase 2: SMS Pilot](#functional-requirements--phase-2-sms-pilot)
9. [Non-Functional Requirements](#non-functional-requirements)
10. [Technical Environment & Constraints](#technical-environment--constraints)
11. [Success Metrics](#success-metrics)
12. [Open Questions & Dependencies](#open-questions--dependencies)

---

## Business Case

### Why This Product Is Being Built

Parents in local communities make frequent, high-stakes decisions about child activities, classes, camps, and caregivers. The most reliable answers to these decisions do not exist in any structured, searchable form. They live in private text threads, WhatsApp groups, school hallways, and the memories of trusted neighbors — scattered, ephemeral, and rapidly outdated.

Existing platforms fail these parents in specific and compounding ways:

- **Google and Yelp** surface public business listings but cannot confirm whether a teacher recently changed, whether a class suits a specific child, or whether a recommendation came from someone whose life is genuinely relevant to the person asking.
- **Care.com and Nextdoor** offer broad reach but provide no provenance, no freshness signal, and no relevance matching.
- **Generic AI assistants** synthesize publicly available information but have no access to real community knowledge and cannot signal trust.

The result is that parents either make decisions with insufficient trusted information or spend significant time soliciting personal recommendations through fragmented, informal channels. This represents both a meaningful pain point and a defensible market opportunity.

**Pando's thesis:** The best local parenting knowledge already exists — in communities, in relationships, in lived experience. The problem is that it is unstructured, unverified, and inaccessible at the moment of decision. Pando captures that knowledge, gives it structure and freshness, and delivers it through the lowest-friction channel available: a standard SMS message.

The long-term defensible asset is not the SMS interface. It is the **structured human-truth graph** — a compounding network of parent knowledge with source attribution, freshness labels, and two-layer matching logic (social affinity + life relevance). This graph grows more valuable with every contribution, confirmation, and thanks signal the network generates, and it cannot be replicated by any generic platform.

**Initial market:** Pasadena, California.
**Build approach:** Two sequential phases — Phase 1 (Seed Tool) to ingest high-quality founder data, Phase 2 (SMS Pilot) to deliver the live product to parents. Phase 2 does not begin until Phase 1 data quality has been reviewed and approved.

---

## Business Context & Goals

### Goals & Objectives

| # | Goal | Objective |
|---|---|---|
| G1 | Build and validate the human-truth ingestion layer | Collect structured, high-quality recommendations and contributor profiles from approximately 350 founding parents in Pasadena before the SMS product launches |
| G2 | Deliver a working SMS pilot | Enable parents to text Pando, receive labeled and trustworthy answers, and trigger paid community outreach (Blasts) when needed |
| G3 | Prove the core engagement loop | Contributors share knowledge → parents ask questions → answers are matched, labeled, and delivered → thanks flow back to contributors → contributors remain engaged and continue contributing |
| G4 | Establish a monetization mechanism | Validate that parents will pay for targeted, high-quality community outreach (Blast tiers) as a premium over free general answers |
| G5 | Protect contributor trust and engagement | Demonstrate that frequency caps, gap enforcement, and the thanks loop sustain contributor participation through the pilot period |

### The "Why" — Strategic Rationale

Pando occupies a position that no existing product holds: a **living, local, compounding trust network** built on structured human knowledge rather than public listings or algorithmic synthesis.

The strategic rationale rests on three pillars:

1. **Trust scarcity creates value.** The more parents rely on generic or unattributed sources, the more they are willing to pay for answers that come with provenance, freshness, and relevance context. Pando's answer labels and matching logic are the direct expression of this.

2. **Network effects compound the asset.** Each contribution makes the graph more valuable. Each thanks signal reinforces contributor engagement. Each new contributor deepens the matching pool. This flywheel is the core reason the product becomes harder to replicate over time.

3. **SMS eliminates the adoption barrier.** By operating over standard SMS rather than requiring an app download or account creation, Pando meets parents in the channel they already use for trusted local communication. Phone number as identity removes every sign-up friction point.

### KPIs — How We Measure Success

**Phase 1 KPIs**

| KPI | Target | Measurement Method |
|---|---|---|
| Founding contributor onboarding completion rate | [TBD — to be set by Pando team; suggested ≥70% of invited contributors] | Completed seed conversations / total invite links sent |
| Contributors submitting 2+ recommendations | [TBD — suggested ≥60% of completers] | Recommendation count per contributor_id |
| Usable activity recommendations collected | [TBD — suggested ≥500] | Approved activities in admin dashboard |
| Caregiver nominations collected | [TBD] | Caregiver records with consent_status = pending or consented |
| Contributors opted into Blasts | [TBD — suggested ≥50% of completers] | blast_opt_in = true in contributors table |
| Profile completeness rate | [TBD — suggested ≥80% of contributors completing neighborhood + child age] | profile_completeness score in social_profiles |
| Drop-off points identified | All major drop-off points documented | Funnel analysis of seed_conversations |

**Phase 2 KPIs**

| KPI | Target | Measurement Method |
|---|---|---|
| Repeat query rate per user | [TBD — suggested ≥2 queries per active user within first 30 days] | conversations count per user_id over rolling 30 days |
| % of queries answered with human-truth layer | [TBD — suggested ≥50% of activity queries; 100% caregiver queries from consented pool only] | Answer composition labels per conversation |
| Blast response rate per contributor | ≥25% over rolling 30 days | blast_responses / blasts_sent per contributor |
| Paid Blast conversion | [TBD — suggested ≥15% of Blast-offered conversations result in purchase] | Stripe payment confirmations / Blast offers presented |
| Contributor retention over pilot period | [TBD — suggested ≥70% of opted-in contributors respond to at least one Blast within 60 days] | blast_responses per contributor over pilot window |
| Thanks delivery rate | [TBD — suggested ≥30% of resolved queries generating a thanks] | thanks_events with thanks_given = true |
| Contributor opt-out rate | [TBD — target <10% over pilot period] | blast_opt_in changes to false |
| User satisfaction / "first stop" behavior | Qualitative signal from pilot interviews + repeat use rate | [TBD — method to be confirmed with Pando team] |

### Definition of Success

**Phase 1 is successful when:**
- Approximately 350 founding contributors have been invited and a high proportion ([TBD] ≥70%) have completed the onboarding flow and submitted at least one recommendation.
- The Pando team has reviewed the collected data and confirmed it is sufficient in quality and quantity to support Phase 2 answer retrieval.
- At least one caregiver has completed consent and been activated (`consent_status = consented AND active = true`).
- The admin dashboard provides sufficient visibility for the team to review, approve, and reject submissions without requiring direct database access.

**Phase 2 is successful when:**
- Parents are returning to Pando for specific, high-intent local decisions — not just trying it once.
- At least one Blast at each paid tier has been fulfilled end-to-end within the promised time window.
- The contributor frequency and protection system is operating in code with no manual overrides needed for cap enforcement.
- The thanks loop is closing: parents are prompted, responding, and thanks are reaching contributors.
- No caregiver has appeared in a user-facing answer without `consent_status = consented AND active = true` enforced at the query level.

---

## Current State Analysis

### Manual Processes — How Things Work Today

| Process | Current Method | Problem |
|---|---|---|
| Finding activity recommendations | Texting individual parents, posting in WhatsApp groups, searching Yelp or Google | Answers are scattered, unsolicited, unverified, and quickly go stale |
| Vetting caregivers | Word-of-mouth through personal networks, Care.com profiles, occasional Nextdoor posts | No provenance, no freshness, no structured trust signal; high-stakes decision with no supporting infrastructure |
| Evaluating recommendation relevance | Mentally assessing whether the recommending parent's context is similar to one's own | Entirely implicit and unreliable; no mechanism to surface whose advice is most contextually relevant |
| Following up on recommendations | Ad hoc, if at all; the feedback loop almost never closes | Contributors never learn whether their advice helped; no reinforcement mechanism |
| Community knowledge management | Not managed at all; knowledge is lost when people move, change phones, or simply forget | Compounding community knowledge loss with each life transition |

### Software Currently Used

Pando is a greenfield product — there is no existing Pando software. The problem space it addresses is currently served (inadequately) by:

| Tool | Role in Current Workflow | Why It Falls Short |
|---|---|---|
| WhatsApp / iMessage groups | Primary channel for trusted local recommendations | Private, ephemeral, unsearchable, no structure |
| Google Search / Google Maps | Activity discovery | Public listings only; no trust layer; no freshness |
| Yelp | Activity reviews | Generic public reviews; no provenance; no relevance matching |
| Care.com | Caregiver discovery | Broad reach; no community trust; no freshness |
| Nextdoor | Neighborhood Q&A | Noisy; no structure; no freshness labeling; no matching logic |
| Generic AI (ChatGPT, Gemini, etc.) | General information synthesis | No real community knowledge; cannot signal trust or freshness |

### Top 3 Pain Points

**Pain Point 1 — Knowledge is trapped and ephemeral.**
The most valuable local parenting knowledge exists in private conversations and personal memory. It cannot be searched, referenced later, or accessed by parents who were not present in those conversations. When a parent moves, changes their phone, or simply forgets, that knowledge is gone.

**Pain Point 2 — Relevance is invisible.**
Even when a recommendation is available, there is no way to assess whether it came from someone whose life context makes it useful. A recommendation from a premium-budget family is not useful to a budget-conscious family. A recommendation from a parent at a different school, in a different neighborhood, with different scheduling constraints may be technically correct but practically irrelevant. There is no mechanism to surface or signal this.

**Pain Point 3 — Trust is unverifiable and stale.**
Reviews on public platforms are anonymous, of unknown recency, and carry no provenance. Parents cannot know when something was last confirmed, who confirmed it, or what their relationship to the subject was. For caregivers in particular, this gap is not merely inconvenient — it is a safety and liability concern.

---

## Users & Stakeholders

### Primary Users

#### Persona 1 — The Querying Parent (Phase 2 End User)

| Attribute | Description |
|---|---|
| **Who they are** | Local parents in Pasadena (initially), seeking trusted recommendations for activities, classes, camps, and caregivers for their children |
| **Primary need** | Fast, trustworthy, contextually relevant answers to high-stakes local parenting decisions, without having to post in multiple groups or make multiple calls |
| **Behavior pattern** | They text Pando a natural-language question when they have a specific need. They may or may not have heard of Pando before; they are invited by a friend or a founding contributor |
| **Key friction points** | Does not want to download another app; does not want to create another account; does not want generic AI answers dressed up as human trust |
| **Success looks like** | They text Pando, receive a clear and honest answer with trust labels, and either act on it directly or trigger a Blast for fresher, more targeted responses |
| **Monetization relevance** | Primary Blast purchaser; pays for Standard, Targeted, Precision, or Last Minute Care tiers based on urgency and specificity of need |

#### Persona 2 — The Founding Contributor (Phase 1 Primary User)

| Attribute | Description |
|---|---|
| **Who they are** | Approximately 350 curated local parents in Pasadena, selected by the Pando team for their community knowledge and network position |
| **Primary need** | To share what they know in a low-friction, dignified way — not to fill out a form or complete a survey |
| **Behavior pattern** | Opens an invite link on their phone; completes a short tap-first profile; engages in a conversational recommendation flow; may return to add more recommendations |
| **Key friction points** | Any experience that feels like a form, a survey, or a chore; being asked too many questions; being contacted too frequently in Phase 2 |
| **Success looks like** | They complete the onboarding flow, submit at least 2 structured recommendations, opt into Blasts, and later receive thanks signals that confirm their contributions helped |
| **Dual role** | In Phase 2, founding contributors also become the first Blast respondent pool and may themselves query Pando |

#### Persona 3 — The Contributor (Phase 2 Ongoing)

| Attribute | Description |
|---|---|
| **Who they are** | Parents who have completed the onboarding questionnaire and are opted into the Blast system |
| **Primary need** | To be a trusted resource in their community without being overwhelmed; to know their contributions matter |
| **Behavior pattern** | Receives occasional Blast SMS messages; responds when they have relevant knowledge; receives thanks when their advice helped |
| **Key friction points** | Over-contact; being asked irrelevant questions (e.g., about activities they have no knowledge of); never knowing whether their advice helped |
| **Success looks like** | They remain opted in, maintain a response rate above 25%, progress through contributor tiers, and receive thanks that reinforce engagement |

#### Persona 4 — The Caregiver (Passive Subject)

| Attribute | Description |
|---|---|
| **Who they are** | Babysitters, nannies, au pairs, or other caregivers who have been nominated by contributors |
| **Primary need** | To control whether they appear in the Pando network; to have accurate information attributed to them |
| **Behavior pattern** | Nominated by a contributor (first name + last initial only); contacted separately by the Pando team for consent; activates or declines |
| **Key friction points** | Being listed without consent; having inaccurate or stale information attributed to them |
| **Success looks like** | `consent_status = consented AND active = true` set only after explicit outreach; disclaimer language on all caregiver answers; ability to be removed |

### Admin Users

| Role | Responsibility |
|---|---|
| Pando Admin (Phase 1) | Review contributor submissions, approve/reject activity and caregiver records, manage caregiver consent outreach, annotate records with notes |
| Pando Admin (Phase 2) | Monitor SMS inbox and conversation history, manage answer queue and Blast fulfillment, handle payment/refund flags, run quality scoring, manage thanks/impact tracking, review pending taxonomy options |

> **⚠️ Open Item:** The number of admin users requires confirmation from the Pando team. If more than one person will perform sensitive actions (particularly caregiver consent management), individual admin logins must be configured from the start so every action is traceable to a named person.

### Decision Maker / Sign-Off Authority

**Product Owner:** Janet (janet@tweekle.com) — final authority on product flow, content, taxonomy decisions, admin process design, and caregiver consent workflow design.

---

## Desired Future State

### Ideal Process — How Things Should Work After the Product Is Built

#### Phase 1 — Ideal Contributor Onboarding Flow

1. A founding contributor receives an invite (link and/or SMS — method [TBD]).
2. They open the link on their phone — no app download, no account creation.
3. They complete a tap-first Social Affinity + Life Relevance profile questionnaire in under 60 seconds. Selections are pre-populated with Pasadena-specific options (neighborhoods, schools, groups, classes). Only neighborhood and child age are required.
4. Pando initiates a natural conversational flow, asking what activities or caregivers the contributor would like to share. The experience feels like texting, not filling out a form.
5. The contributor submits one or more structured activity recommendations and/or caregiver nominations.
6. Caregiver nominations are flagged for separate human-managed consent outreach. No caregiver is activated automatically.
7. Data is saved in structured form. The admin team reviews submissions, approves or rejects records, adds notes, and tracks consent status.
8. The contributor may return to the Seed Tool to add more recommendations at any time [to be confirmed].

#### Phase 2 — Ideal Parent Query Flow

1. A parent saves a US phone number as "Pando" in their contacts.
2. They text a natural-language question: *Any good toddler music classes near South Pasadena?*
3. Pando may ask one clarifying question to improve the answer and capture life-relevance context: *How old is your child? Any budget or schedule constraints?*
4. Pando classifies the intent, retrieves the best available answer from the human-truth graph, applies correct trust and freshness labels, and composes a response.
5. The response is immediately helpful, honest about what it is, and actionable:
   - Human-truth answer components are labeled (e.g., "Validated by multiple parents," "Last confirmed 4 weeks ago").
   - Public/general information is clearly labeled as such and never presented as human trust.
   - A Blast offer is presented if the answer is absent, stale, or weak.
6. If the parent wants fresher or more targeted information, they can purchase a Blast tier. They receive a Stripe payment link and confirm.
7. The Blast is scored against the contributor pool using two-layer matching (social affinity + life relevance), subject to frequency caps and the 48-hour gap rule.
8. Contributors receive a targeted, warm, low-friction SMS. Those who respond are reviewed by the admin team. The best answer is approved and sent within the promised window.
9. A few days later, Pando asks the parent whether the recommendation worked out and whether they would like to send thanks to the contributor.
10. If the parent says yes, the contributor receives a thanks message. Thanks are batched (no more than one per week per contributor).

#### Ideal Caregiver Answer Flow

1. A parent texts Pando asking for a caregiver recommendation.
2. Pando checks the database for caregivers with `consent_status = consented AND active = true`.
3. If a qualifying caregiver exists, Pando returns their first name and last initial, the nominating parent's relationship type, any caveats, freshness date, reference willingness, and the required disclaimer language.
4. If no qualifying caregiver exists, Pando honestly states this and offers a Last Minute Care Blast.
5. Public caregiver listings (e.g., Care.com profiles) are never presented as trust-backed community recommendations.

### Key Data the System Must Capture and Provide

| Data Category | Captured Via | Used For |
|---|---|---|
| Contributor identity | Phone number (E.164), name, neighborhood, child ages | Identity, matching, freshness attribution |
| Social affinity profile | Tap-first questionnaire (schools, neighborhoods, groups, activities) | Two-layer matching scoring |
| Life-relevance profile | Tap-first questionnaire (budget posture, logistics, family setup, trust circles) | Matching modifier layer |
| Activity recommendations | Conversational seed flow | Answer retrieval, trust labeling |
| Caregiver nominations | Conversational seed flow | Consent workflow, caregiver answer pool |
| Freshness signals | Last validated date, confirmation count, vouch count | Freshness labels on answers |
| Blast responses | Contributor SMS replies to Blast | Answer composition, quality scoring |
| Thanks events | Parent Y/N response to thanks prompt | Contributor engagement, tier progression |
| Contributor tier status | Quality response count, thanks received, response rate | Access privileges, matching priority |
| Payment records | Stripe webhook events | Blast activation, refund management |
| Pending taxonomy options | "Other" entries from questionnaire | Admin review and promotion to canonical list |

---

## Hypothesis

### Pain Points Addressed

| Pain Point | How Pando Addresses It |
|---|---|
| Knowledge is trapped and ephemeral | Structured ingestion of community knowledge with source attribution, freshness timestamps, and permanent (but updatable) storage |
| Relevance is invisible | Two-layer matching (social affinity score + life-relevance modifiers) surfaces answers from contributors whose lived context is genuinely relevant to the person asking |
| Trust is unverifiable and stale | Mandatory answer labels (provenance, freshness, validation count) on every response component; caregiver hard consent filter enforced at database level |
| Friction of finding recommendations | Single SMS interface, no app download, no account; phone number as identity |
| Contributors never know if they helped | Thanks loop closes the feedback cycle; access-tier rewards recognize sustained contribution |

### Goals Achieved

**If the hypothesis holds:**
- Parents will use Pando as a first stop for high-intent local decisions, not as a fallback after other channels fail.
- Contributors will sustain engagement through the pilot period because the thanks loop and access-tier system make contribution feel meaningful and rewarded.
- The Blast system will demonstrate that parents are willing to pay for targeted, high-quality community outreach when their need is specific and urgent.
- The structured trust graph will grow more valuable with each cycle, creating compounding defensibility against generic alternatives.

### Potential Solutions Considered

| Solution Considered | Why Not Selected |
|---|---|
| Native iOS/Android app | Eliminates the zero-friction onboarding advantage; deferred to P1 or later |
| Web-based Q&A platform (Yelp-style) | No provenance, no freshness, no matching logic; does not solve the core problem |
| WhatsApp bot integration | Platform dependency risk; does not provide structured data ingestion; not selected for pilot |
| Email-based digest | Wrong channel for high-intent, time-sensitive queries |
| SMS-first with no human-truth layer (AI only) | Core product differentiation requires real community knowledge; AI-only would replicate existing generic assistants |
| Subscription model from launch | Premature; requires proven value loop first; deferred to P1 |

---

## Functional Requirements — Phase 1: Seed Tool

### Epic 1: Contributor Access & Identity

**Epic Summary:** Founding contributors must be able to access the Seed Tool from their mobile phone without downloading an app or creating an account. Access is controlled via invite mechanism.

---

**Story 1.1 — Invite Link Access**
*As a founding contributor, I want to open a link on my phone and access the Seed Tool immediately, so that I do not have to download an app or remember a password.*

**Acceptance Criteria:**
- The Seed Tool is accessible via a URL that opens in a mobile browser without requiring an app download.
- The link grants access to the contributor flow without a username or password.
- The access method (unique invite link per contributor, phone number capture, or both) is confirmed with the Pando team before build [TBD — Open Question #3].
- If unique invite links are used, each link is associated with a specific contributor record and can be tracked for completion status.
- If phone capture is used, contributors enter their phone number in E.164 format and this becomes their identity.
- The link or phone capture experience renders correctly on iOS Safari and Android Chrome at mobile viewport sizes.

**Priority:** P0
**Phase:** Phase 1

---

**Story 1.2 — Contributor Record Creation**
*As the Pando admin team, I want each contributor who accesses the Seed Tool to have a unique, identifiable record created, so that their submissions can be attributed, reviewed, and tracked.*

**Acceptance