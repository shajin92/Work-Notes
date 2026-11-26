# RevenueCat hiring manager interview: prep sheet

> **For the hiring manager call, use `revenuecat_interview_plan.md`.** This file is the full reference behind it, including material for later rounds.

Everything here is based on `resume_revenuecat.md`, the resume RevenueCat has. Anything in [square brackets] is a detail only you know; fill it in before you practise.

## The facts

| | |
|---|---|
| When | **Thursday 10 Dec 2026, 7:30–8:15 PM IST** (2:00–2:45 PM UK time), on Zoom. Confirmed 26 Nov. |
| Who | Guilherme Mota, Developer Support Engineer, Portugal (6:30 PM his time) |
| Format | Video call, conversational, 45 minutes. The link is in the Google Calendar invite. |
| Recording | RevenueCat records with BrightHire. You may opt out; it does not affect your application. |
| Not allowed | Your own recording or AI note-taking tools during the call. Do not use any AI help live. |
| After | They generally do not share feedback. Expect a take-home, a presentation of it, live coding, then a founder interview if you pass. |

## What they said they want (from their two pages)

- Honest, self-aware answers: what motivates you, where you want to go, what you learned from mistakes.
- Concrete examples tied to the role and to their values. They suggest the STAR shape: situation, task, action, result.
- A quiet place, headphones, working camera.

Their values: **Customer Obsession, Always Be Shipping, Own It, Balance.**

## About the interviewer

- About ten years as a mobile engineer (started on Android in 2014), then moved into this same role in April 2025.
- Worked remotely for years, including at Automattic, where almost everything is written.
- Active in the RevenueCat community: mentored and judged a hackathon, posts about Shipaton and App Growth Annual.

He will understand your Android work in detail, and he will very likely ask about Health Decoder because your resume says it was built for Shipaton 2026.

## Three things to fix before the call

### 1. Health Decoder and RevenueCat (integration done 24 Nov; now learn it)

The SDK is now in the app and a Test Store purchase has gone through. This was the biggest gap, and it is closed. But Claude wrote most of the code, so your job now is to understand every line well enough to explain it without notes.

**What exists (all true, you can say it):**

- RevenueCat Kotlin Multiplatform SDK `purchases-kmp` 4.2.0, core and paywall UI libraries, in the shared `commonMain` code.
- The API key is read from `local.properties` into the Android build config, so it is never in git. The SDK is configured once in `MainActivity`.
- One entitlement, `health_decode_pro`. One offering, `default`, with yearly ($79.99) and monthly ($9.99) packages.
- A paywall built in RevenueCat's paywall editor, shown from an Upgrade button under the capture screen.
- `billing/ProAccess.kt` checks the entitlement from customer info, updates when the paywall reports a purchase, and has a Restore action.
- A Test Store purchase completed on the Android emulator and the app showed Pro as active.
- The paywall was redesigned in the editor (pill label, headline, three benefits, yearly and monthly plans) and published. The app picked up the new design without a rebuild, because paywalls are remote.
- The privacy sheet now tells users that RevenueCat and the app store receive a random install ID and what was bought, and never the documents.
- A release build with a Test Store key runs with purchases off, because the SDK would otherwise crash on purpose.
- All of it is on the branch `feature/revenuecat-pro-and-redesign` in `~/personal/doctorApp` (not pushed).

**What is not done (say this plainly if asked):**

- It uses the Test Store only. There are no real Google Play products and the app is not on the Play Store.
- Pro does not unlock a feature yet; it only changes the status bar text.
- iOS compiles but has no app shell, so the SDK is not configured there.
- [Did you submit Health Decoder to Shipaton? Write the true answer here.]
- The document explanation runs in a development "sample mode" right now, because the AI account has no credit. Do not demo it as the real product; say it is sample output if it comes up.

**Real things you hit, which make good interview answers:**

| What happened | Why it matters in support |
|---|---|
| The `android { }` block was placed above `plugins { }` in the Gradle file, so the build could not work | A setup mistake, not an SDK bug. Most tickets start like this. |
| The first version number used was old (`2.2.2+17.10.0`); the latest was `4.2.0` | Old versions and copied snippets are a common cause of problems. |
| The log said "Displaying default template because paywall is missing for offering 'default'" | The paywall must be published and attached to the offering, or the SDK shows a fallback. |
| Restore did nothing: "Restoring purchases not available in test store" | Test Store behaves differently from the real stores. |
| A Test Store key crashes the app on purpose in a release build | Developers must swap to the real store key before shipping. |
| A paywall change showed up in the app only after pressing Publish | Drafts are not live; "my paywall did not change" is usually an unpublished draft. |
| After reinstalling the app, Pro was gone | A reinstall creates a new anonymous user. Purchases belong to the app user ID, which is why Restore and logging in exist. |
| The paywall editor refused to publish until an offering was selected | The dashboard and the code both have to be set up; either can be the missing half. |

**Before the call, do these yourself:**

1. Read `billing/ProAccess.kt` and the `MainActivity` change until you can explain each line aloud.
2. Break it on purpose and read the logs: rename the entitlement in code, remove the API key, unpublish the paywall. See what each failure looks like.
3. Do the next step alone, with no AI: make Pro unlock one real thing, or add Google Play products. Note where the docs confused you.
4. Fix the paywall's Terms and Privacy links (they point to example.com) and publish the recoloured draft.

**If he asks "did you write this yourself?"** Tell the truth: "I set up the dashboard and worked through the integration with an AI assistant, then went through the code and broke it on purpose to understand it." Their own onboarding has a "Using AI to integrate RevenueCat?" button, so this is normal. Claiming you wrote it all by hand is the risk, because he will ask follow-up questions.

### 2. Numbers on the resume

- "4 years of experience": August 2022 to now is 4 years 3 months, and the first part overlapped with Zoho Schools. If asked, explain it plainly.
- "25+ features": have five you can name immediately.
- Education: BS in progress until 2027. Be ready to say how you manage study and work.

### 3. Notice period

Confirmed 25 Nov: **15 days** at Zoho. Say it plainly.

## Your answers, drafted

Practise these aloud until each takes under two minutes. Change the wording so it sounds like you.

### Tell me about yourself

> I'm an Android developer at Zoho. For four years I've worked on Arattai, our messaging app, mostly on login, multi-device support and contact sync. The part of the job I enjoy most is investigation: a user reports something strange, and I trace it through the app and the server logs until I know where it actually broke. Outside work I build my own apps. This year I built Health Decoder, which explains medical reports in simple language, for Shipaton. That is how I got to know RevenueCat, and it is why this role caught my attention.

### Why support engineering and not pure Android development?

> In my current job, the work I look forward to is not writing a new screen. It is when someone says "this user's contacts are wrong and we don't know why". I like finding the cause and explaining it clearly to the person who has the problem. In this role that is the main job, not a side task. I also keep writing code, because debugging someone's integration means reading and writing it.

### Why RevenueCat?

> Two reasons. First, I've been on the developer's side: when I planned subscriptions for my own app, I saw how much work the stores leave to you, and RevenueCat removes most of it. Second, your customers are developers like me, so I understand what they are trying to do when they write in.
>
> I also read the stories from your support engineers. Several of them came from building their own apps, like me, and what stood out was that the job doesn't stop at tickets: docs, internal tools, and investigations like the one into India's payment rules. That's the part I want.

### What would you bring to the team? (or: why you?)

> Three things. Debugging with very little information: at Zoho I trace user reports through client and server logs to a root cause, which is most of this job. Real Android and Kotlin Multiplatform experience, plus some Swift, so I can read most of the code developers send. And I'm in India, so I know the local payment side first-hand: e-mandates, UPI, cards that fail renewals. I read about the e-mandate investigation on your blog, and that's the kind of regional problem I could pick up quickly for APAC developers.

Read RevenueCat's India post (link in "Reading list") before you say the last part.

### What is your experience with RevenueCat and in-app purchases?

> I integrated it into my own app, Health Decoder, which is Kotlin Multiplatform. I set up a `health_decode_pro` entitlement, an offering with yearly and monthly plans, and a paywall from the paywall editor, and I completed a purchase in the Test Store on Android. It is not live with real Play products yet. What caught me out was the dashboard side: the app showed a fallback paywall until I published mine and attached it to the offering. I also learned that Restore does nothing in the Test Store, and that a Test Store key crashes a release build on purpose. [Add one thing you found confusing in the docs, from your own notes.]

Note: your resume says "7-day trial". The offering you built has no trial yet. Either add one or do not mention it.

### Story 1: Customer Obsession (a hard bug)

The contacts bug where a saved contact showed "Invite to Arattai" or a "~name" instead of the contact's name.

- **Situation:** users reported that people saved in their phone book, who were on Arattai, showed an "Invite" button, and the chat showed the person's profile name with a "~" instead of the saved contact name. [How many reports, and when.]
- **Task:** find out whether the fault was in the app or on the server. Nobody knew at the start.
- **Action:**
  - Pulled the server logs for the affected user and checked the server's copy of their contacts. The server had the contact correctly, so the app was losing it.
  - Matched each sync request against the Android code of the exact app version the user was running.
  - Found the cause: the server returns contacts 500 at a time with a token for the next page. On a full refresh the app cleared its local contacts table, saved only the first page, ignored the next-page token, and still stored the newer sync marker. So anyone with more than 500 synced contacts silently lost the rest.
  - Reproduced it in a unit test.
- **Result:** the app now follows the paging token (merged in October 2026), plus a one-time full resync to repair phones that were already affected, because the paging fix alone only stops new cases. Until the update, logging out and in again was the workaround. [Did reports stop? Which release?]

Say it in general terms as above. Do not share internal identifiers, user IDs, log contents or code from Zoho.

**Why this story fits the role:** it is exactly a support engineer's job: a vague user report, logs, the right app version, a root cause, a workaround for the user today, and a fix for everyone later.

**Backup story (same value):** one device was uploading its whole phone book 80–100 times a day and getting rate-limited. Cause: two sync runs were cancelling each other, and the Refresh button reported success when it had failed. Fix: let only one sync run at a time and report the real result.

### Story 2: Always Be Shipping

Personal-to-Business account migration, or SIM-bound login.

- **Situation:** [why it was needed and the deadline].
- **Task:** your part of it.
- **Action:** how you split it into pieces, what you shipped first, how you worked with the IAM backend team.
- **Result:** [release date, how many users, any issues after release].

### Story 3: Own It (a mistake)

Pick a real one. They ask for mistakes directly, so do not say you have none.

- **Situation:** [a bug you shipped, or a wrong assumption you made].
- **Action:** how you found out, what you told your team, how you fixed it.
- **Result:** what you do differently now.

### Story 4: Explaining something to a non-technical person

The poultry farm app.

> The client ran the farm on spreadsheets. I sat with them to understand their day, found that some of their spreadsheet calculations were wrong, and explained it using their own numbers and not technical terms. [What they said, and what changed.]

### Questions other candidates were asked

All 10, with full answers, are in "Every reported question, with an answer to say" further down.

### Notice period and availability

> My notice period at Zoho is 15 days. I'm fine with APAC working hours.

## Questions to ask him

1. You moved from mobile engineering into support. What made you switch, and what was harder than you expected?
2. What does a normal ticket look like, and how much of your day is debugging versus writing?
3. How does the APAC shift fit with a team mostly in Europe and the Americas? What hours would I cover, and how much overlap with American mornings is expected?
4. India is not in your employer-of-record list. How does employment work for someone based here?
5. What are the stages after this call for this role?
6. Your blog mentions DSEs building internal tools and docs alongside tickets. How is time split between tickets and that kind of work in practice?

## What the interview needs (online research, 24 Nov)

### What the job posting asks for, and where you stand

| Requirement | You |
|---|---|
| 3+ years in an engineering or technical role | Yes: Zoho since August 2022 |
| Experience integrating and shipping RevenueCat in a mobile app | Partly: integrated with a Test Store purchase; not shipped to a store |
| Understands mobile development and the release process | Yes: Arattai releases. Be ready to describe a release start to finish |
| iOS (Swift/Objective-C) or Android (Java/Kotlin) | Yes: Kotlin, plus KMP and some Swift interop |
| Debugging with limited information; reading stack traces | Yes: the contacts investigations. Your strongest area |
| Excellent written and spoken communication, daily | Weakest area. Practise aloud and write short, clear answers |
| Empathy for developers, who write in at their lowest point | Show it in the scenario answers below |
| Likes wearing many hats | Side projects, freelance client, KMP and iOS work |

The posting lists no nice-to-haves and says nothing about working hours, on-call or weekends. Ask.

### What they expect over the first year (from the posting)

- **Month 1:** learn the common customer problems and unblock them; learn the support tools; improve docs.
- **Month 3:** suggest product improvements; build small internal apps to reproduce issues.
- **Month 6:** find the main problem areas and fix them; build internal tools and dashboards.
- **Month 12:** expert troubleshooter; interview, hire and mentor other support engineers.

Useful line for "why you": Health Decoder is already the kind of small app they expect you to build to reproduce issues.

### Their values, in their own terms

| Value | What they mean | Your story |
|---|---|---|
| Customer Obsession | Decide hard questions by asking what makes the customer's life easier | Contacts bug: workaround for the user today, fix for everyone later |
| Always Be Shipping | Deliver value, not talk; cut scope to the smallest useful version | [your story] |
| Own It | If nobody owns the issue, it is now yours; no blame, no throwing it over a fence | [your mistake story] |
| Balance | The work should stretch you, not break you | Job plus degree plus side projects: how you keep it sustainable |

### Product knowledge to have ready

The most common support problem is "my offerings are empty". RevenueCat's docs say it is nearly always configuration. On Google Play the usual causes are:

- the product is Draft or Deactivated in Play Console, not Active;
- the package name or product ID does not match exactly between Play Console and RevenueCat (one developer had pasted an offering ID where a subscription ID belonged);
- Google Play service credentials are missing or wrong;
- the device is not a license tester, so no sandbox purchase is offered;
- testing on an emulator or with several Google accounts signed in; a physical device is recommended.

Also know: the difference between a product, an entitlement, an offering and a package; anonymous app user IDs versus logging in; Restore purchases; Test Store versus the Play sandbox versus production.

### The later rounds, from candidate reports

- **Take-home:** a couple of hours on a fairly simple use case, to learn the product.
- **Presentation:** you present the take-home to two team members.
- **Live coding:** close to real-world scenarios; they watch how you collaborate.
- **Founder interview:** with the CTO, about the business.

All 7 Glassdoor reports for this role, newest first (read 25 Nov):

| Date | Result | What it adds |
|---|---|---|
| Apr 2026 | No offer, positive | 1.5 months from applying to the first call, then answers within a day after each step |
| Dec 2025 (Nov interview) | Hired | Five interviews including two tests; the tests checked common sense and attention to detail, not specific skills. Asked: "Is the customer always right?" |
| Nov 2025 | No offer, negative | Rejected after two rounds and a take-home, with no feedback. Asked: a new problem a customer hits, how you solve it and communicate |
| May 2025 | Hired | Hiring manager chat, take-home (a couple of hours), presentation to two team members, live coding, founder interview with the CTO. Asked: telling a developer their approach isn't the recommended one |
| Mar 2025 | Hired | Followed the "How we hire" page exactly; the take-home meant digging into docs, blog and community. Asked: giving feedback, a difficult support case, going out of your way to solve an issue |
| Jun 2020 (two reports) | One hired, one declined | Older process with the CEO. Asked: describe your work experience; do you have remote work experience |

Small sample, so treat it as a guide. Expect no feedback if rejected.

### Every reported question, with an answer to say

These are the 10 questions candidates for this role reported on Glassdoor (nobody posted answers there). Each answer below is a spoken version, about one to two minutes. Anything in [square brackets] only you can fill in. Say them in your own words; he should hear you, not a script.

**1. Is the customer always right?**

> No, but their problem is always real. A developer might be wrong about the cause: they say the SDK is broken when a product ID doesn't match. My job is to find the real cause and show it to them without making them feel foolish. And sometimes they are right and it is our bug, so I take every report seriously until I've proved otherwise.

**2. A customer faces a problem that is rather new. How would you approach solving it and communicating with the customer?**

> First I reply quickly, so they know someone has it and what I'm checking first. Silence is the worst part of a support ticket.
>
> Then I collect what I need: SDK version, platform, the logs with debug logging on, the app user ID and the steps. I try to reproduce it in a small test app, because a reproduction turns a guess into a fact.
>
> If it's beyond me, I bring in the engineer who owns that area with everything already gathered, so they don't start from zero. I keep the customer updated at agreed times, even when the update is "still investigating". If there's a workaround, I give it to them first while the real fix is made.
>
> Afterwards I write it up, an internal note or a docs change, so the next person with the same problem finds the answer.
>
> I did this at Zoho: a contacts problem nobody had seen before, which I traced through server logs and the exact app version, then reproduced in a unit test.

**3. How would you tell a developer that their approach is not the one RevenueCat recommends?**

> I start by asking what they're trying to achieve, because their approach usually has a reason behind it, and sometimes that reason shows a gap in our docs or product.
>
> Then I show the recommended way, explain in one or two sentences what will go wrong with theirs (for example, checking purchases themselves instead of checking the entitlement, which breaks restores and cross-platform access), and give a short code example.
>
> If they still want their way, I respect it, tell them the risks plainly, and help them make it as safe as I can. And if many developers want the same thing, I pass that to the product team as feedback.

**4. Tell us about a time you had to give feedback.**

Use a code review, a design discussion, or feedback to a client. Template:

> [Situation: e.g. a teammate's change to the contacts sync would have cleared local data before the new data arrived.] I didn't want to just block it, so [Action: I reviewed it the same day, wrote a comment that pointed to the exact lines, explained what would happen to a user with a poor connection, and suggested a small change. I offered to pair on it if that was quicker.] [Result: they changed it before release, and we added a test for that case.] What I learned is to make feedback about the code and the user, never the person, and to always suggest a way forward.

If you have no code-review story, the poultry farm client works: you told them their spreadsheet calculations were wrong, using their own numbers.

**5. Tell us about a difficult customer support experience.**

> At Zoho, users reported that people saved in their contacts showed as "Invite to Arattai", as if they weren't on the app. It was hard because nobody knew if it was the app or the server, and the users couldn't tell us more than "it looks wrong".
>
> I checked the server's copy of their contacts: it was correct, so the app was losing data. I matched each sync request in the logs against the code of the exact app version they were on, and found the app saved only the first 500 contacts on a full refresh and ignored the rest. I reproduced it in a unit test.
>
> We gave affected users a workaround straight away, logging out and in, then shipped a fix plus a one-time resync to repair phones already affected. [Add: how many users, or that reports stopped.]

Keep it general: no Zoho user IDs, log contents or code.

**6. Tell us about a time you went out of your way to solve an issue.**

Use the backup story so it doesn't repeat answer 5:

> One user's phone was uploading its whole contact list 80 to 100 times a day, and the server started rate-limiting it. Nobody had asked me to look at it; I saw it while checking logs for something else. I traced it to two sync runs cancelling each other, and the Refresh button told the user it had worked when it hadn't. I fixed it so only one sync runs at a time and the button reports the real result. It mattered beyond that one user, because the same bug was wasting battery and data for anyone who hit it.

**7. Describe your work experience.**

> I've been an Android developer at Zoho since August 2022, on Arattai, our messaging app. My main areas have been sign-in (SIM-based login and re-authentication), multi-device support, moving users from personal to business accounts, and contact sync. I've also worked on our Kotlin Multiplatform SDK and its iOS integration, so I'm comfortable reading Swift.
>
> The part I'm best at is investigation: taking a vague report, following it through client and server logs, finding the root cause and proving it with a test.
>
> Outside work I build my own apps: Health Decoder, where I integrated RevenueCat, and an offline farm-management app for a small business client. Alongside work, I'm finishing a BS in Computer Science.

**8. Do you have experience working remotely?**

> Not as a full-time remote employee; Zoho is office-based. But a lot of how I work already looks remote. With the backend and IAM teams I work mostly through written messages, tickets and shared logs. My side projects are self-directed, with nobody checking on me. And I work with my farm-app client [remotely, by calls and messages]. I know remote work depends on clear writing, so I write up what I find so someone in another time zone can pick it up without a call.

Check the farm-client detail is true before you say it.

**9. Do you have any experience with in-app purchases or subscriptions?**

> Yes, through my own app. I integrated RevenueCat's Kotlin Multiplatform SDK into Health Decoder: an entitlement called `health_decode_pro`, an offering with yearly and monthly plans, and a paywall built in the paywall editor. I completed a purchase in the Test Store on Android and checked the entitlement in customer info.
>
> A few things I learned by hitting them: a paywall change only appears after you publish it, and then it updates without an app release. After reinstalling, Pro disappeared, because a reinstall creates a new anonymous user, which is why restore and logging in matter. And a Test Store key crashes a release build on purpose, so you must switch keys before shipping.
>
> It isn't live with real Google Play products yet; that's my next step.

**10. Take-home (2020): explain at a high level which RevenueCat SDK features you would use for a use case.**

Practise with any app. Example, a fitness app with free workouts and a paid Premium:

> I'd create one entitlement, `premium`, for everything paid. In the stores I'd create a monthly and a yearly subscription, maybe with a 7-day trial on yearly, and attach both products to the entitlement.
>
> I'd group them in an offering, the default one, with a monthly package and an annual package. Offerings let me change what's shown without an app update, and later run an experiment with a second offering at a different price.
>
> For the screen, I'd use a paywall from the paywall editor, so marketing can change it without a release.
>
> In the app I'd configure the SDK once at start, fetch offerings to show the plans, and after purchase check customer info for the `premium` entitlement, never the product itself, so restores and other platforms work. I'd add a Restore button. If the app has accounts, I'd log users in with their own ID so Premium follows them to a new phone.
>
> For testing: the Test Store first, then the Google Play and App Store sandboxes before release.

### Extra practice: guessed questions (Scoutify, lower priority)

Scoutify's RevenueCat page lists "likely" questions it generated from RevenueCat's job postings. They are guesses, not reports from candidates, and its process section describes a generic software-engineering loop. Practise these only after the 10 real ones. Several of its questions are for marketing roles (CRM strategy, billing optimisation) and are skipped here.

**Tell me about working with another team to build something.**

> At Zoho I built SIM-based login and re-authentication with the IAM backend team. We agreed the API and the error cases in writing before either side built anything, and kept one shared list of open questions so nothing was decided in a side chat. [When something broke in testing, how you resolved it together.] I learned that most cross-team delays come from different assumptions about edge cases, so I write those down first.

**How do you prioritise when several deadlines are close?**

> I sort by impact on users first, then by what blocks other people. A bug that loses user data beats a new feature; a question that is blocking a teammate gets answered before my own work. If two things truly clash, I tell my lead early with a proposal, not on the deadline day. [Give one real week where you did this.]

In support, the same rule applies: a developer whose purchases are failing in production comes before a "how do I" question.

**Explain in-app subscriptions, and what makes them succeed.**

> The app sells access for a period, renewed automatically through the App Store or Google Play, and the app has to keep checking whether the user is still entitled. The hard parts are the stores' different rules, renewals, refunds, grace periods and restores, which is the work RevenueCat removes.
>
> What makes them succeed: the user sees the value before the paywall, trials and pricing fit the audience, the paywall is clear about price and renewal, and the team keeps testing prices and paywalls instead of setting them once. Keeping existing subscribers matters as much as getting new ones.

**How do your values fit a remote-first company?**

> Remote work runs on writing and trust. I already work that way with other teams at Zoho: I write up what I found so someone can act without a call. Their values fit how I like to work, especially Own It: when I find a problem nobody owns, like the sync loop, I take it. And Balance matters to me; I'm studying alongside work, so I plan my time instead of working until I'm tired.

**A customer's subscription renewals dropped sharply. How would you troubleshoot it?**

> First I'd check whether it's real or a data problem: compare the dashboard with the store's own reports, and check whether the drop is in one store, one country, one app version or one product.
>
> Then the common causes: Google Play or App Store credentials or server notifications that stopped working, so renewals happen but RevenueCat isn't told; a recent app release that changed product IDs or the SDK setup; a price increase that needed user consent; billing problems moving users into grace periods; or an actual rise in cancellations.
>
> I'd tell the customer what I'd ruled out and what I was checking next, and bring in engineering if it looked like our side.

**Have you led a project?**

> Not as a manager. I've led pieces of work end to end: the personal-to-business account migration flow, from working out the cases with the backend team to shipping it. [Size, outcome.] My style is to write the plan down, make the open questions visible, and check in often. The job posting says the role later interviews and mentors new support engineers, and I'd like to grow into that.

**How do you use AI coding tools, and how do you check their output?**

This came from an engineering posting, but it is likely to come up anyway. Answer honestly:

> I use them daily, for reading unfamiliar code, writing tests and first drafts. I never trust the output without checking it. [Example: a time the tool gave a wrong answer and you caught it, such as a wrong version number or code in the wrong place.] When I integrated RevenueCat into Health Decoder I worked with an AI assistant, then went through the code and broke it on purpose to understand it.

### What current DSEs say about the job (RevenueCat blog, read 25 Nov)

Source: "Life as a developer support engineer at RevenueCat" (published Dec 2024, updated Apr 2025). Seven DSEs, current and former.

- **The daily work:** answering tickets and troubleshooting; debugging mobile apps; writing SQL; reading server logs; app store policies; integrations. Time also goes to docs, internal tools, side projects and hack days.
- **Who they hired:** an iOS indie developer; an indie developer who used RevenueCat for three years; someone straight from graduate school; a bootcamp graduate from ed-tech; people from computational neuroscience and biology. Several came in through a tweet from the CEO. So a non-traditional background is normal.
- **Who you would serve:** large apps including ChatGPT, VSCO, Runna and Ladder, as well as small indie developers.
- **Where people went next:** one of the first DSEs has led the support team for about three years; another moved to Technical Account Management, helping large apps migrate.
- **Their advice to applicants:** set up an app and use the SDK yourself (you have); expect variety across platforms, policies and tools; love helping people.
- **Phrases worth reusing because they are theirs:** "treated like a high-performing adult"; building "an intuition for solving problems, even when you can't see the full code"; "a lifeline for developers who are stuck or frustrated".

**The India story: use it.** One DSE investigated subscription failures caused by India's rules on auto-renewing payments, read the government notices and internal data, and turned it into a blog post. RevenueCat's 2021 post on it says that from 30 September 2021, Play subscriptions priced at ₹5,000 or more became non-renewing for Indian users, banks that had not adopted the RBI e-mandate rules could not authorise new subscriptions, and users had to verify their card or be cancelled. Full notes, and why not to quote the numbers as current, are in `revenuecat_dse_knowledge.md` section 12.

Why it matters for you: you are in India, which is in their APAC region, and Indian developers hit payment problems that a US-based team learns about slowly. A line to use when he asks what you would bring:

> I'm in India, so I know the local payment side as a user: e-mandate approvals, UPI, cards that fail on renewal. I read about the e-mandate investigation on your blog, and that's the kind of regional problem I could pick up quickly for APAC developers.

Only say the parts you can back up; read RevenueCat's India post before the call.

### Pay philosophy (CTO's blog post, read 25 Nov)

"The case for location-independent salaries" by Miguel Carranza, the CTO (2021, updated 2023). He is the person you'd meet in the founder round.

- **Same pay everywhere,** set at the 80th percentile of San Francisco salaries for companies of their size. Reasons given: the work is the same quality anywhere, not everyone can move to the US, good people have remote options anyway, and their mission is helping developers earn a living wherever they live.
- **No negotiation,** from day one. He doesn't want the best negotiator to earn the most, and admits they've lost good candidates over it. So don't negotiate; ask questions about the contract instead.
- **Cons he names:**
  1. It costs more than location-based pay. Their answer is "talent density": fewer people, each expected to produce Silicon Valley-level output. Expect a high bar.
  2. It's much easier to hire in low-cost countries, so the team grows faster outside California. But most customer activity happens during American mornings, so time-zone coverage matters to them.

What this means for you: US Pacific 9 AM is 10:30 PM IST, and US Eastern 9 AM is 7:30 PM IST. Ask what hours the APAC role covers, and how much overlap with American mornings is expected.

Two lines you could use with the CTO, only if they fit the conversation:
> I read your post on location-independent salaries. The no-negotiation part made the process easier for me to trust.

> Your post mentions most activity happens in American mornings. How does the APAC support shift fit with that?

## Reading list (all checked 24–25 Nov)

The full list, with what to take from each page and a day-by-day schedule, is in `revenuecat_reading_list.md`. The short version:

1. [Developer Support Engineer job posting](https://jobs.ashbyhq.com/revenuecat/edcc931f-acd1-49ee-9b58-207cdcf31b43): requirements and the first-year milestones.
2. [How we hire at RevenueCat](https://www.revenuecat.com/blog/company/how-we-hire-at-revenuecat/) and [Interview expectations](https://revenuecat.notion.site/RevenueCat-Interview-Expectations-1bdcb1a108b0800c8941def0f076f9e2).
3. [RevenueCat values](https://www.revenuecat.com/blog/company/values/).
4. [Life as a Developer Support Engineer at RevenueCat](https://www.revenuecat.com/blog/company/developer-support-engineer-stories).
5. [Updates to Google Play subscriptions in India](https://www.revenuecat.com/blog/engineering/indias-new-subscription-rules) (2021; the limits have changed since).
6. [General troubleshooting](https://www.revenuecat.com/docs/revenuecat-support/general-troubleshooting) and [why are offerings or products empty](https://community.revenuecat.com/sdks-51/why-are-offerings-or-products-empty-124).
7. [Glassdoor: DSE interview reports](https://www.glassdoor.co.in/Interview/RevenueCat-Developer-Support-Engineer-Interview-Questions-EI_IE2588117.0,10_KO11,37.htm?sort.sortType=RD&sort.ascending=false).

Skip: Scoutify (generated questions and a generic process).

## Do I need DSA for RevenueCat?

- **For this call (10 or 11 Dec): no.** It is a conversation about your background, the role and their values. No coding.
- **For a later round: yes, a little.** Candidates for this exact role report a live coding interview after the take-home. RevenueCat says its coding problems come from day-to-day work and avoid obscure algorithms, and they judge how you collaborate and explain, not only the answer.
- **What that means in practice:** `dsa-2-weeks/week-0-basics.md` and days 1–4 of `week-1.md` (arrays, strings, hash maps, two pointers, sliding window, stacks) are enough. Trees, graphs, backtracking and dynamic programming are unlikely here.
- **More useful than extra DSA for this company:** reading logs, debugging an Android integration, writing a clear reply to a developer, and knowing the RevenueCat SDK and dashboard.
- **Bjak is different.** Their first step is a proctored test with two LeetCode-style questions, so the full three-week plan still matters there.

Order of priority until the call: stories and spoken answers first, RevenueCat product knowledge second, one hour of DSA basics a day third.

## The night of the call

- If the call is in the evening, eat early and be at your desk 20 minutes before.
- Test camera, microphone, headphones and internet by 11:00 PM. Keep a phone hotspot ready.
- Have on paper: your four stories as bullet points, the five questions, your notice period.
- Close every AI tool and note-taker before joining.
- Speak slowly. If you did not understand a question, ask him to repeat it.

## Plan until then (trip to Meghalaya 26 Nov – 1 Dec)

You're away from the laptop from the evening of 26 Nov until you land in Chennai at 9 PM on 1 Dec. That leaves 25–26 Nov before, and 2 Dec until the call (now 10 or 11 Dec) after.

| Dates | Do |
|---|---|
| **25 Nov (today)** | Confirm the notice period (offer letter or HR). Read `ProAccess.kt` and the `MainActivity` change once, with the app running. |
| **26 Nov (before the 9 PM flight)** | Break the integration on purpose: rename the entitlement, remove the key, and read what the logs say each time. Put `revenuecat_dse_knowledge.md` on your phone so you can read it offline. |
| **26 Nov – 1 Dec (trip)** | Holiday first. Optional, only in dead time like the two flights and long drives: read on your phone. **26 Nov flight (9:00–11:45 PM):** `revenuecat_dse_knowledge.md` sections 1–4, about 40 minutes, then rest. **1 Dec flight (5–9 PM):** sections 5–13, about 1.5 hours, then the 10 real answers in this sheet, read silently. No laptop, no DSA. |
| **2 Dec** | Recover. Fill in the [brackets] in the stories: feedback, mistake, shipping, user numbers. |
| **3 Dec** | Knowledge sections 1–6 again, explaining each aloud with Health Decoder as the example. |
| **4 Dec** | Knowledge sections 7–13, plus ten threads on the community forum. |
| **5 Dec** | Say all 10 real answers aloud; record yourself on your phone; listen back. |
| **6 Dec** | One full mock interview (with me, or a friend reading the questions). Fix what sounded weak. |
| **7 Dec** | Own material: your resume line by line, the billing code, the questions to ask him. |
| **8–9 Dec** | Follow `revenuecat_interview_plan.md` (it is the current plan). |
| **Call day (10 or 11 Dec)** | Light review of the "Say" lines only. Test camera, microphone and internet an hour before. |

DSA: paused for the trip. From 2 Dec, 30 minutes a day only; full plan resumes on 10 Dec.
