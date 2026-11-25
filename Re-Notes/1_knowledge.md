# RevenueCat knowledge for the DSE interview

Notes from RevenueCat's own docs, blog and community, read on 25 Nov 2026 and written in plain English. This is what a Developer Support Engineer is expected to know. Each section ends with what to say if it comes up, using Health Decoder as your example.

Where a source didn't cover something, it says "not confirmed". Don't present those parts as fact in the interview.

For the list of pages and a reading schedule, see `revenuecat_reading_list.md`.

---

## 1. The core model: product, entitlement, package, offering

| Thing | What it is | Health Decoder |
|---|---|---|
| **Product** | What the store sells: a subscription or one-time purchase, with an ID set in Play Console or App Store Connect | `monthly`, `yearly` (Test Store products) |
| **Entitlement** | A level of access, such as "pro". The app checks this, never the product | `health_decode_pro` |
| **Package** | A slot that holds equivalent products across platforms, e.g. "monthly" holds the Android and iOS monthly product | Monthly, Annual |
| **Offering** | The set of packages shown together on one paywall | `default` |

### Entitlements

- Most apps need **one** entitlement that unlocks everything paid. Add more only for real tiers (Gold, Platinum) or content sold separately.
- Products are **attached** to entitlements. A subscription grants access while active; a lifetime (non-consumable) purchase grants it forever. Consumables usually aren't attached.
- **Many-to-many:** one product can unlock several entitlements, and one entitlement can have several products.
- **Attaching is retroactive.** Attach a product and past buyers get access; detach it and past buyers lose it.
- **Classic mistake:** add a new product but forget to attach it, and users pay and get nothing.
- Entitlements are shared by every app in the same project, which is what makes cross-platform access work.

### Offerings and packages

- Offerings are optional but needed for Paywalls, Experiments and Targeting.
- The **default offering** is returned as `current`. Apps should show `current`, not a hard-coded offering ID, so the paywall can change from the dashboard without an app update.
- An offering's identifier **can't be changed** after creation.
- The package order in the dashboard is the order the SDK returns.
- **Removing a package** hides the plan from new buyers only. Existing subscribers keep renewing unless the product is removed from sale in the store.
- Offerings are usually pre-fetched at launch and **cached for about 5 minutes**, so a dashboard change can take a relaunch to appear.
- Standard package identifiers such as `$rc_monthly` exist (you saw `$rc_monthly` and `$rc_annual` in your logs), but the overview page doesn't list them. Not confirmed from the docs.

**Say:** "In Health Decoder I have one entitlement, `health_decode_pro`, with a monthly and a yearly product attached, grouped in the default offering as two packages. The app only checks the entitlement, so a restore or a purchase on another platform unlocks it too."

---

## 2. CustomerInfo: what the app knows about a user

- **Contains:** the original app user ID, first-seen date, a management URL, all purchased product IDs, expiry and purchase dates, active subscriptions, and entitlements.
- **Each entitlement has:** `isActive`, product ID, will-renew, period type (trial, intro, promotional, normal), expiry date (null for lifetime), store, a sandbox flag, and billing-issue and unsubscribe dates.
- **Important:** an unsubscribe date or a billing-issue date does **not** mean the entitlement is inactive. A cancelled user keeps access until the period ends.
- **Check access with** `entitlements["id"]?.isActive`, or check the active map isn't empty when there are several entitlements.
- **Empty CustomerInfo** is normal before any purchase or sync. An entitlement that exists in the dashboard won't appear for a user who hasn't bought it.

### Caching and updates

- The SDK caches CustomerInfo. It refreshes only when the cache is **older than 5 minutes**, and only when the app calls `getCustomerInfo`, buys or restores.
- **Pull:** `getCustomerInfo`. Call it whenever premium content opens.
- **Push:** the update listener fires at launch and after any purchase, restore or refresh. It is **not** pushed from the server: it only fires after the app makes a request.
- Kotlin uses `getCustomerInfoWith(onError, onSuccess)` and the `updatedCustomerInfoListener` property.

### Offline

If RevenueCat's servers are down, the SDK can still grant entitlements for a new store purchase from a cached product-to-entitlement map, then sync later. It doesn't cover one-time products or cross-platform purchases, and it's off if the app finishes transactions itself.

### Google Play gotcha

On Google Play, entitlements can briefly look empty around renewal, before Google applies a grace period or deferred billing. A "my user lost access for an hour" ticket can be this.

**Say:** "If a user says they paid but don't have access, I check their CustomerInfo in the dashboard first: is the entitlement there and active, is it the app user ID I expect, and is the product attached to the entitlement."

---

## 3. Users and identity: the cause of many tickets

### Anonymous users

- With no app user ID, the SDK creates a random ID starting `$RCAnonymousID:` and caches it on the device.
- **Uninstalling clears it**, so a reinstall gets a new anonymous ID. This is exactly why Pro disappeared after you reinstalled Health Decoder.
- Anonymous IDs can't share access across platforms or apps.

### Your own app user IDs

- Set at configure time or later with `logIn`. Users then keep access on reinstall and on other devices without restoring, and cross-platform works.
- IDs are case-sensitive and scoped to the project.
- **`logIn` from an anonymous user** can merge (alias) the anonymous ID into the custom one, but only if that custom ID doesn't already have an anonymous alias.
- Logging in while already logged in just switches users; there's no need to log out first.
- **`logOut` creates a new anonymous ID.** If an app only uses its own IDs, it should never call `logOut`.

### IDs to never use

- **A hard-coded string:** every install becomes one user, and people who never paid get access. Fix: ship an update that sets proper IDs, then delete the old shared customer.
- **Email or advertising ID:** guessable and a privacy problem.
- **Blocked values:** `null`, `nil`, `none`, `undefined`, `unknown`, `anonymous`, `guest`, an empty string, `0`, `-1`, `[]`, `{}`, `[object Object]`, and anything containing `/`.
- **Recommended:** a non-guessable UUID v4, at most 100 characters.

**Say:** "When I reinstalled my test app, Pro disappeared, because the anonymous ID is stored on the device and a reinstall creates a new one. That's why apps need a Restore button, or their own user IDs."

---

## 4. Restore and transfer behaviour

### Restore versus sync

- **`restorePurchases`:** only from a user action, such as a Restore button, because it can show the store's sign-in prompt.
- **`syncPurchases`:** the programmatic version with no prompt, mainly for migrations. It carries the same risk of moving purchases between users.
- Every app should have a Restore button, even with accounts.

### Restore behaviour setting (project settings, applies to all apps)

| Option | What happens | Watch out for |
|---|---|---|
| **Transfer to new App User ID** (default) | Access moves to whoever restores; only one user holds it | User A loses access when user B restores on the same device |
| Transfer if there are no active subscriptions | Transfers unless the purchase is an active subscription | One-time purchases still transfer |
| Keep with original App User ID | A different user gets a `receipt_already_in_use` error | Blocks legitimate restores; only for apps where everyone must log in first |
| Share between App User IDs (legacy) | Aliases users on restore | Can't be turned back on after switching away |

Purchases owned by an anonymous user are always aliased, never transferred.

### Android-specific

- Consumables and non-renewing subscriptions can only be restored with your own app user IDs.
- From Play Billing Library 8 (`purchases-android` 9.0.0 and later), consumed one-time purchases can't be queried again, so an anonymous user's consumables can be lost. The mitigation is backing up RevenueCat's SharedPreferences file with Android Auto Backup or `RevenueCatBackupAgent`, which isn't guaranteed on every device.

**Say:** "'My purchase moved to another account' is usually the default transfer setting doing its job: whoever restores gets it. I'd explain that and, if it doesn't suit their app, point them to the restore behaviour setting."

---

## 5. Paywalls

- A paywall is built in the dashboard (from a template, from scratch, or with the AI editor) and **attached to an offering**. Each offering can have its own.
- It changes **without an app release**. It goes live only after **Publish**; until then the app shows a fallback. In your app, the log said "Displaying default template because paywall is missing".
- Needs the RevenueCat UI SDK. Paywalls work on Android 7.0 (API 24) and later and iOS 15 and later, not on watchOS, tvOS or visionOS.
- Multi-step paywalls need newer SDKs (Android 10.16.0+). On older SDKs, only the last screen shows.
- Some features need newer Android SDKs, for example discount variables need 10.29.0+; on older SDKs they show as empty values.
- Paywall events can be sent to analytics tools or your own server.

**Say:** "A 'my paywall looks wrong' ticket: first check the paywall is published and attached to the current offering, then check the SDK version supports the features used."

---

## 6. Testing: Test Store and the Google Play sandbox

### Test Store

- RevenueCat's own test environment. **No store setup needed.** Purchases update CustomerInfo and show in the dashboard as sandbox data.
- A separate **Test Store API key**, for debug builds only.
- Purchases show a fake sheet with **success, failure and cancel** buttons. Good for UI tests.
- Test Store products **can't be edited**. To change a price, create a new product, swap it into the offering, and archive the old one.
- **A Test Store key in a release build crashes the app on purpose.** On Android that means any non-debuggable build. Play internal, closed and open tracks count as release, and so does TestFlight. An override exists on native Android (10.21.0+) and iOS, but not in Kotlin Multiplatform, Flutter or React Native.
- Restore does nothing in the Test Store (you saw this in your logs; the Test Store page doesn't cover it).
- **Renewals:** up to 5, then the subscription ends. Weekly or monthly renews every 5 minutes, 2-month every 10, 3-month every 15, 6-month every 30, yearly every hour.
- Minimum SDKs include Android 9.9.0 and Kotlin Multiplatform 2.2.2.

### Google Play sandbox

1. **License testers:** add the tester's Google account under Play Console > Settings > License testing.
2. **One Google account** on the device. Several accounts can break purchases. A screen lock PIN may be needed, or purchases fail with "Something went wrong".
3. **Upload a signed build to a closed track.** You don't have to roll it out, but the release must be approved and available.
4. **The tester must open the opt-in link** and accept, or products won't load.
5. The **application ID must match** the build being tested.
6. Use a **real device** if possible; an emulator needs Google Play Services.
7. Check the purchase in the dashboard with **View Sandbox Data** on.

**Sandbox renewals:** up to 6. Weekly or monthly every 5 minutes, 3-month every 10, 6-month every 15, yearly every 30. **Note:** these differ from the Test Store's timings, so don't mix them up.

**Which card appears:** a closed-track tester who is also a license tester gets a test card. Not a closed-track tester: a real card is offered, or "item could not be found" in debug. A release build without license testing: "unable to download app".

**Say:** "I tested with the Test Store first because it needs no Play Console setup. Before release I'd move to the Play sandbox: closed track, license testers, opt-in link. And swap the Test Store key, because it crashes release builds on purpose."

---

## 7. Troubleshooting: the daily job

### Debug logs

- Set **before** configure: `Purchases.logLevel = LogLevel.DEBUG`.
- Every SDK line starts with `[Purchases]`, so filter Logcat on that.
- **Emoji prefixes:** 🤖‼ Google Play error, 🍎‼ Apple error, 📦‼ Amazon error, 😿‼ RevenueCat error, 😻 RevenueCat success, 😻💰 purchase info received, 💰 product messages, ℹ general, 👤 app user ID, ⚠ implementation warning.
- **Red flags:** any ERROR line, any HTTP status other than 200, "Invalid Product Identifiers".
- **Android debug overlay** (SDK 6.9.2+, Compose only): add `purchases-debug-view` in debug and `purchases-debug-view-noop` in release. It shows diagnostics, configuration, customer info and offerings.

### What to ask a developer for

The docs don't publish a fixed list. From the pages, a sensible first reply asks for:
- debug logs from app launch to the failure;
- the SDK name and version, and the platform;
- the app user ID (from the logs, 👤);
- the product and offering identifiers;
- whether it happens in sandbox, production or both;
- the steps to reproduce.

### General troubleshooting checklist (RevenueCat's page)

| Ticket | Usual cause and fix |
|---|---|
| Products can't be fetched / offerings empty | "Nearly always configuration." See section 8 |
| User lost their entitlement | App user ID changed (reinstall, new account) or the subscription expired. Restore purchases |
| Purchase "transferred" to another user | Normal under the default restore behaviour; review the setting |
| Everyone shares one user | Hard-coded app user ID. Ship a fix, delete the shared customer |
| Integration events missing | The integration often needs a customer attribute set first |
| Revenue differs from store reports | Expected: RevenueCat estimates prices and doesn't import store reports |
| Android crash `NoCoreLibraryDesugaringException` or `NoClassDefFoundError ... billingclient` | Play Billing 7.1.x on older devices. Enable core library desugaring |

### Error messages seen in the docs

- **"There are no Play Store products registered for your offerings"** (a configuration error): add a Play product to every package in the current offering.
- **`ITEM_UNAVAILABLE`** on a real device: a Play-side problem; see section 8.
- **"None of the products could be fetched"**: product IDs, package name, product status or the wrong API key.
- **"Invalid Play Store credentials"** with 503 or 521: credentials not valid yet.
- `PRODUCT_NOT_AVAILABLE_FOR_PURCHASE` as an exact code: not confirmed on these pages.

---

## 8. "My offerings are empty" on Android: the full checklist

The most common ticket. Go through it in this order:

1. **Product IDs match exactly** between RevenueCat and Play Console: case-sensitive, no spaces. Subscriptions may need the `productId:basePlanId` form.
2. **Package name / applicationId matches** Play Console exactly, with no `.debug` suffix.
3. **The right API key:** the public Android key starting `goog_`, never a secret `sk_` key.
4. **Products are Active** in Play Console, not Draft or Inactive, and marked backwards compatible.
5. **The app is on at least an internal or closed track**, and the tester opted in and installed it from Play once.
6. **The device:** a real device, or an emulator with a Play Store image, signed in as the tester.
7. **Country and pricing:** the product and base plan are available in the tester's country.
8. **Developer agreements and banking** are complete in Play Console.
9. **Service credentials** are valid and more than 36 hours old.
10. **The offering is Current**, and every package has a product for the Play store.
11. **Configure runs early**, in `Application.onCreate`, before any fetch.
12. **Caching:** offerings cache for about 5 minutes; relaunch after a change.
13. **Network:** a VPN or firewall isn't blocking RevenueCat.
14. **Read the debug logs.**
15. **Isolate:** try the same key and package name in RevenueCat's sample app, to tell an app problem from a dashboard problem.

One real case from the community forum: a developer had copied an *offering* ID where the *subscription product* ID belonged.

---

## 9. Google Play service credentials

- **What they're for:** RevenueCat calls the Google Play Developer API on the developer's behalf to validate purchases, read orders, manage subscriptions (refunds, deferrals) and manage products. They also enable real-time notifications.
- **Setup, briefly:**
  1. In Google Cloud, enable the Android Publisher and Play Developer Reporting APIs. Create a service account with Pub/Sub Editor and Monitoring Viewer roles, and download a JSON key. RevenueCat also offers a setup script.
  2. In Play Console, invite the service account's email, add the app, and grant four permissions: view app information, view financial data, manage orders and subscriptions, manage store presence.
  3. Upload the JSON in RevenueCat and check the validation result.
- **Takes up to 36 hours** to become valid. Editing and saving a product description in Play Console often speeds it up.
- **Common problems:** Pub/Sub Editor can't create topics (try Pub/Sub Admin); a company policy blocks creating service accounts or keys; the key was deleted in Google Cloud after uploading.

---

## 10. Real-time developer notifications (RTDN)

- Google's push messages through Google Cloud Pub/Sub. RevenueCat works without them using only credentials, but RevenueCat **strongly recommends** them.
- **With RTDN:** cancellations and renewals arrive in near real time; webhooks and integrations are faster; charts lag less; prices are more accurate.
- **Without RTDN:** cancellations are found within a few hours, and a refunded one-time purchase may only show after the user reopens the app.
- **Setup:** enable Pub/Sub, choose a topic in RevenueCat, paste it into Play Console's monetisation setup, and include subscriptions, voided purchases and one-time products. Send a test notification and check "Last received".
- If the test fails, Google's notification account needs the Pub/Sub Publisher role.
- The **deferred** replacement mode (for plan changes) needs RTDN to work properly.

---

## 11. Subscription states and events

| State or event | What it means | Access |
|---|---|---|
| **Unsubscribe** (`UNSUBSCRIBE`) | The user turned off auto-renew | Keeps access until the period ends |
| **Billing issue** (`BILLING_ISSUE`) | A charge failed; may carry a grace period end date | Not expired. Access continues during any grace period |
| **Expiration** (`EXPIRATION`) | The subscription ended; the reason says why, e.g. `BILLING_ERROR` after grace ran out | Remove access now |
| **Paused** (`SUBSCRIPTION_PAUSED`, Google Play only) | A pause is scheduled | Don't remove access yet; only on EXPIRATION with reason `SUBSCRIPTION_PAUSED` |
| **Cancellation with `CUSTOMER_SUPPORT`** | A refund | Access removed |

- **Refunds on Google Play:** a refund made through RevenueCat's dashboard or API removes access immediately. A refund made by Google or in Play Console can take up to 24 hours to show.
- Other cancellation reasons include `DEVELOPER_INITIATED`, `PRICE_INCREASE` and `UNKNOWN`.
- **Account hold** (Google's state after grace ends without payment): not covered on the pages read. Not confirmed.
- **Webhooks:** respond with 200 quickly. RevenueCat retries up to 5 times over about 80 minutes, so handle each event ID only once.

**Say:** "'My user cancelled but still has Pro' is correct behaviour: cancelling only turns off renewal, and access lasts until the period ends. Remove access on expiration, not on unsubscribe."

---

## 12. India and the RBI e-mandate rules

From RevenueCat's 2021 blog post. This is history; check what applies today before quoting numbers.

- India's central bank (RBI) required extra authentication for recurring card payments. The deadline moved from 31 March to **30 September 2021**.
- **From 4 May 2021**, Google paused new auto-renewing subscriptions, free trials and intro prices in India.
- **From 30 September 2021**, Play subscription products priced at **₹5,000 or more** became non-renewing for Indian users. Cheaper ones kept auto-renewing.
- Banks that hadn't adopted the rules couldn't authorise new subscriptions. Google sold a non-renewing version instead, and the app couldn't tell which users were affected.
- Users with existing subscriptions had to **verify their card** in Play (Payments and subscriptions > Subscriptions > the alert) or be cancelled.
- **RevenueCat's advice to developers:** detect Indian users by the price's currency code (INR), remove "free trial" and "auto-renew" wording from the paywall for them, and serve them a separate offering.
- **Today's limit:** Google Pay's India help page mentions automatic payments up to **₹15,000** per transaction without extra approval, but the page's own dates are from 2021. Not confirmed as current.
- Note the two figures are different things: ₹5,000 was a Play **product price** threshold in 2021; ₹15,000 is a **per-payment** limit on automatic charges.

**Say:** "I'm in India, so I know this side first-hand: e-mandates, card verification prompts, renewals that fail. Your team once investigated subscription failures caused by these rules, and that's the kind of regional problem I could pick up for APAC developers."

---

## 13. Ten likely tickets and your first move

| Ticket | First thing to check |
|---|---|
| "My offerings are empty" | Section 8, starting with product IDs, package name and product status |
| "User paid but has no access" | Is the product attached to the entitlement? Is it the expected app user ID? |
| "User lost Pro after reinstalling" | Anonymous ID changed. Restore, or use their own user IDs |
| "Purchase moved to another account" | Default transfer behaviour |
| "Cancelled user still has access" | Correct until the period ends |
| "Paywall didn't change" | Not published, or not attached to the current offering; 5-minute cache |
| "App crashes in the internal test track" | A Test Store key in a non-debuggable build |
| "Sandbox purchase fails" | License tester, opt-in link, one account, closed track available |
| "Renewals look delayed or missing" | Service credentials and RTDN set up and valid |
| "Revenue doesn't match Play Console" | Expected; RevenueCat estimates and doesn't import store reports |

---

## Sources

- [Entitlements](https://www.revenuecat.com/docs/getting-started/entitlements)
- [Offerings overview](https://www.revenuecat.com/docs/offerings/overview)
- [Customer info](https://www.revenuecat.com/docs/customers/customer-info)
- [Identifying customers](https://www.revenuecat.com/docs/customers/identifying-customers)
- [Restoring purchases](https://www.revenuecat.com/docs/getting-started/restoring-purchases)
- [Restore behavior](https://www.revenuecat.com/docs/projects/restore-behavior)
- [Paywalls](https://www.revenuecat.com/docs/tools/paywalls)
- [Test Store](https://www.revenuecat.com/docs/test-and-launch/sandbox/test-store)
- [Google Play sandbox testing](https://www.revenuecat.com/docs/test-and-launch/sandbox/google-play-store)
- [Debugging](https://www.revenuecat.com/docs/test-and-launch/debugging)
- [General troubleshooting](https://www.revenuecat.com/docs/revenuecat-support/general-troubleshooting)
- [Google Play service credentials](https://www.revenuecat.com/docs/service-credentials/creating-play-service-credentials)
- [Google server notifications](https://www.revenuecat.com/docs/platform-resources/server-notifications/google-server-notifications)
- [Refunds](https://www.revenuecat.com/docs/subscription-guidance/refunds)
- [Webhook event types and fields](https://www.revenuecat.com/docs/integrations/webhooks/event-types-and-fields)
- [Community: why are offerings or products empty?](https://community.revenuecat.com/sdks-51/why-are-offerings-or-products-empty-124)
- [Codelab: fetching offerings errors](https://revenuecat.github.io/codelabs/troubleshooting/fetching-offerings)
- [Updates to Google Play subscriptions in India](https://www.revenuecat.com/blog/engineering/indias-new-subscription-rules)
- [Google Pay India: RBI auto-payment changes](https://support.google.com/pay/india?p=rbi_auto)
