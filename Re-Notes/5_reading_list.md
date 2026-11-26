# What to read for the RevenueCat DSE interview

The content of these pages, summarised, is in `revenuecat_dse_knowledge.md`. Use this list to read the originals.

Everything a Developer Support Engineer candidate should read before the hiring manager call (Thu 10 Dec 2026, 7:30 PM IST), in order. All links were checked on 25 Nov 2026.

Tick each box when done. For every item, the "Take away" line is what you should be able to say afterwards without looking.

Total: about 6–7 hours, spread over 30 Nov – 7 Dec.

---

## Part 1 — The role and the company (about 1 hour)

Read these first. They tell you what Guilherme is judging.

- [ ] **[The job posting](https://jobs.ashbyhq.com/revenuecat/edcc931f-acd1-49ee-9b58-207cdcf31b43)** (10 min)
  Take away: the requirements, and the 1 / 3 / 6 / 12-month milestones. Be able to say which ones you already do.

- [ ] **[How we hire at RevenueCat](https://www.revenuecat.com/blog/company/how-we-hire-at-revenuecat/)** (10 min)
  Take away: the stages, that they use STAR-style stories, and that they don't negotiate offers.

- [ ] **[Interview expectations](https://revenuecat.notion.site/RevenueCat-Interview-Expectations-1bdcb1a108b0800c8941def0f076f9e2)** (5 min)
  Take away: be honest and self-aware, have concrete examples, and expect no feedback if rejected.

- [ ] **[RevenueCat values](https://www.revenuecat.com/blog/company/values/)** (10 min)
  Take away: Customer Obsession, Always Be Shipping, Own It, Balance, each in one sentence, with one story of yours for each.

- [ ] **[Life as a Developer Support Engineer](https://www.revenuecat.com/blog/company/developer-support-engineer-stories)** (10 min)
  Take away: what the daily work is (tickets, logs, SQL, store policies, docs, tools), the varied backgrounds they hire, and the India story.

- [ ] **[Compensation and employment overview](https://revenuecat.notion.site/RevenueCat-Compensation-Employment-Overview-184cb1a108b0801398d4e525928597ab)** (10 min, you read it on 20 Nov)
  Take away: no pay cut for India, but India isn't in their employment-partner list, so ask how you'd be employed.

---

## Part 2 — The product, as a DSE must know it (about 2.5 hours)

This is the vocabulary of every ticket. Read each page, then explain it aloud in your own words, using Health Decoder as the example.

- [ ] **[Entitlements](https://www.revenuecat.com/docs/getting-started/entitlements)** (15 min)
  Take away: an entitlement is the access level ("Pro"); apps check the entitlement, never the product, so restores and other platforms work.

- [ ] **[Offerings, packages and products](https://www.revenuecat.com/docs/offerings/overview)** (20 min)
  Take away: product = what the store sells; package = a slot such as monthly or annual; offering = the set shown on a paywall, changeable without an app update.

- [ ] **[Customer info](https://www.revenuecat.com/docs/customers/customer-info)** (15 min)
  Take away: the object that says what a user has; when it is cached and when it is fetched.

- [ ] **[Identifying customers](https://www.revenuecat.com/docs/customers/identifying-customers)** (20 min)
  Take away: anonymous IDs versus your own app user IDs, logging in and out, and why your reinstall made Pro disappear.

- [ ] **[Restoring purchases](https://www.revenuecat.com/docs/getting-started/restoring-purchases)** (15 min)
  Take away: when restore is needed, and how it moves purchases between app user IDs.

- [ ] **[Paywalls](https://www.revenuecat.com/docs/tools/paywalls)** (15 min)
  Take away: built in the dashboard, attached to an offering, live only after publishing.

- [ ] **[Kotlin Multiplatform installation](https://www.revenuecat.com/docs/getting-started/installation/kotlin-multiplatform)** (10 min)
  Take away: the steps you followed in Health Decoder, so you can talk through them.

- [ ] **[Test Store](https://www.revenuecat.com/docs/test-and-launch/sandbox/test-store)** (10 min)
  Take away: purchases with no store setup; restore does nothing; a Test Store key crashes a release build on purpose.

- [ ] **[Google Play sandbox testing](https://www.revenuecat.com/docs/test-and-launch/sandbox/google-play-store)** (20 min)
  Take away: license testers, internal test tracks, and why a sandbox purchase may not appear.

---

## Part 3 — Troubleshooting, the core of the job (about 1.5 hours)

This is what you'll do every day, and what the take-home and live coding are likely to touch.

- [ ] **[Debugging](https://www.revenuecat.com/docs/test-and-launch/debugging)** (15 min)
  Take away: how to turn on debug logs and what to ask a developer to send.

- [ ] **[General troubleshooting](https://www.revenuecat.com/docs/revenuecat-support/general-troubleshooting)** (20 min)
  Take away: the first checks support runs on any ticket.

- [ ] **[Google Play service credentials](https://www.revenuecat.com/docs/service-credentials/creating-play-service-credentials)** (15 min)
  Take away: why RevenueCat needs them, and that missing or wrong credentials break purchases and renewals.

- [ ] **[Community: why are offerings or products empty?](https://community.revenuecat.com/sdks-51/why-are-offerings-or-products-empty-124)** (15 min)
  Take away: the most common ticket. On Google Play: product not Active, ID or package-name mismatch, bad credentials, not a license tester.

- [ ] **[Community forum](https://community.revenuecat.com/)** (30 min)
  Read ten recent Android or Kotlin Multiplatform questions and the staff replies. Notice how they ask for logs and versions, and how they phrase things. If you can answer one yourself, do it; it's a story for the interview.

---

## Part 4 — India and APAC (about 30 min)

Your edge as a candidate in India. Read before you use the India line in "What would you bring?".

- [ ] **[Updates to Google Play subscriptions in India](https://www.revenuecat.com/blog/engineering/indias-new-subscription-rules)** (15 min)
  Take away: from 30 Sep 2021, Play subscriptions priced at ₹5,000 or more became non-renewing in India, unprepared banks couldn't authorise new subscriptions, and users had to verify their card. History, not current rules.

- [ ] **[Google Pay India: RBI auto-payment changes](https://support.google.com/pay/india?p=rbi_auto)** (10 min)
  Take away: what a user in India sees when a renewal needs approval, and what they must do.

---

## Part 5 — Your own material (about 1 hour)

He'll ask about these; you should know them better than anything above.

- [ ] **`~/personal/doctorApp/client/composeApp/src/commonMain/kotlin/com/healthdecoder/client/billing/ProAccess.kt`**
  Take away: explain every line aloud — entitlement check, restore, paywall listener.

- [ ] **The `MainActivity` change** in the same project.
  Take away: where the SDK is configured, why the key lives in `local.properties`, and why a release build with a test key turns purchases off.

- [ ] **`~/personal/jks/resume_revenuecat.md`**
  Take away: every line is true and you can give an example for each. Fix the "7-day trial" claim.

- [ ] **`~/personal/jks/revenuecat_interview_prep.md`**
  Take away: the 10 real questions, your four stories, and the questions to ask him.

---

## Skip

- **Scoutify's RevenueCat page**: generated questions and a generic engineering process, not real reports.
- **iOS-only docs** (StoreKit, App Store Connect setup): useful later, not before this call. Know that iOS exists and differs; don't study it now.
- **Dashboard charts, integrations and experiments in depth**: know they exist, nothing more.

---

## Schedule (around the Meghalaya trip, 26 Nov – 1 Dec)

| Dates | Read |
|---|---|
| 25–26 Nov | Part 5 (your own code), with the laptop |
| 26 Nov – 1 Dec | Optional, on your phone during flights and drives: `revenuecat_dse_knowledge.md`. Nothing else. |
| 2 Dec | Part 1 |
| 3 Dec | Part 2, each page explained aloud |
| 4 Dec | Part 3 and Part 4, including ten forum threads |
| 5–7 Dec | No new reading. Practise answers (see the prep sheet plan) |
| Day before the call | Skim the "Take away" lines only. |

DSA: paused for the trip; 30 minutes a day from 2 Dec; full plan from 10 Dec.
