# Respect: avoiding dark patterns

Read this for growth features, ads, consent, subscriptions, cancellation and permission flows, or whenever a request sounds like "make it harder to say no".

A dark pattern gets a tap the user didn't mean to give. It lifts this week's number and spends trust that took years to earn. Toss requires partners building inside its app to avoid the patterns in the first table, and has written about what happened when its own ad placements broke these rules: people mistook an ad among their transactions for a real transaction and phoned support, and a reward button that opened an ad without warning made them distrust the buttons around it.

**Contents:** Five rules about entering, leaving and choosing · Wider catalogue · Ads and rewards · Consent and permissions · Friction that helps · When you are asked to build one

## Five rules about entering, leaving and choosing

| Pattern | What it looks like | Do instead |
|---|---|---|
| **Blocked entry** | A full-screen sheet, ad or notification request the instant a screen opens, before the user sees what they came for | Show the screen. Offer the promotion in its own slot. Ask for notifications when there is something specific to be notified about. |
| **Back-button trap** | Pressing back or close opens "Wait! Before you go" | Back goes back. If unsaved work would be lost, say exactly what, once. |
| **No way out** | The only control on the screen is the one the business wants pressed | Always a visible way to decline or leave, styled as a normal control |
| **Surprise ad** | A full-screen ad appears mid-task, for example after tapping a menu item | Ads never interrupt a task. If viewing an ad is the trade, say so on the button before the tap. |
| **Vague button** | The label doesn't say what tapping does, or only makes sense after reading the paragraph above it | The label names the result on its own |

## Wider catalogue

| Pattern | What it looks like | Do instead |
|---|---|---|
| **Confirmshaming** | "No thanks, I don't like saving money" | "Not now" |
| **Fake urgency** | A countdown that restarts, "offer ends soon" with no end | Real deadlines only, with the actual date and time |
| **Fake scarcity or social proof** | "Only 2 left", "37 people are looking at this" with no data behind it | Show real numbers or none |
| **Pre-checked consent** | Optional marketing or data-sharing boxes ticked by default | Unticked. "Agree to all" visibly includes them and they are one tap to undo. |
| **Bundled consent** | One checkbox covering both the necessary and the optional | Separate items, each marked required or optional |
| **Hidden costs** | Fees, taxes or a minimum term that appear on the last step | Show the full price as early as it is known, and on the confirm screen at the latest |
| **Sneaking** | An extra item, insurance or tip added to the basket by default | Nothing is added that the user didn't choose |
| **Hard to cancel** | Subscribing takes one tap, cancelling takes a phone call | Cancelling is as easy to find and as short as subscribing |
| **Silent renewal** | A free trial converts to paid without a reminder | State the date and price up front, and remind before charging |
| **Disguised ad** | A promotion styled like the user's own data or like a system message | Its own clearly separate slot, labelled as an ad |
| **Nagging** | The same request on every visit after it was declined | Ask once at a relevant moment. Respect "no" for a long time. |
| **Trick wording** | Double negatives, or a toggle where it's unclear which side is "on" | Plain positive statements. A switch that says what it does when on. |
| **Visual interference** | The decline is grey-on-grey, tiny, or placed where it won't be seen | Decline is plainly legible and within reach, even if the accept is the primary |
| **Emotional pressure** | A crying mascot, or copy implying the user is letting someone down | Neutral imagery. State the facts. |
| **Forced permission** | Can't continue without enabling notifications, contacts or location that the task doesn't need | Ask only for what the feature needs, when it needs it. Keep working if declined. |
| **Misleading preview** | Sample results that look like the user's real data | Label examples, and make them visibly generic |

## Ads and rewards

- Say it before the tap. If a button leads to an ad, the label includes it and the length: "Watch a 30-second ad for 10 points".
- The stated length is the real length.
- Ads live in their own slots. Never inside a list of the user's transactions, messages or files.
- An ad never stands between the user and the task they are in the middle of.
- The reward has to be worth the time, or don't offer the trade.
- There is always a way to skip the trade and continue without the reward.

## Consent and permissions

- **Ask in context.** At the moment the feature is used, after one line on what it's for.
- **Ask for the minimum.** Removing a permission you don't strictly need is a valid design improvement.
- **Declining works.** The feature degrades gracefully, with a quiet note on how to enable it later. No repeated prompts.
- **Required and optional are separate and labelled.**
- **Legal text is reachable, not forced.** One tap opens each document. Nobody is made to scroll it to enable the button unless the law requires it.
- **Consent is as easy to withdraw as it was to give**, from one obvious place in settings.

## Friction that helps

Sometimes slowing the user down serves them: before an irreversible action, or in a product whose point is self-control, such as savings a user has chosen to lock. Toss's team, designing a lock for teenagers' savings, settled on three tests for when making something harder is acceptable:

1. **It is fair.** It works the same for everyone regardless of age, literacy, memory or dexterity. They rejected "type your account number from memory" because it would be far harder for a seven-year-old than for a seventeen-year-old.
2. **It doesn't shame.** No guilt, no sense of failure, no punishment imagery. They rejected an animation of the piggy bank breaking.
3. **It leaves the user in charge.** It never requires someone else's permission to use what is yours. They rejected needing a friend's approval.

What passed: pressing and holding a button for a few seconds. A pause to reconsider, with no penalty.

Good friction looks like:

- press and hold, or a second deliberate tap, to confirm something irreversible,
- typing the name of the thing being permanently deleted,
- a plain summary of what will be lost, shown once,
- an undo window after the action instead of a warning before it.

It never looks like extra screens, hidden controls, waiting timers or repeated "are you sure?" dialogs on ordinary actions.

## When you are asked to build one

Requests arrive as "add an exit pop-up", "make the free plan harder to find", "pre-select the annual plan", "put a timer on it".

1. **Find the goal behind it.** Usually retention, conversion or revenue.
2. **Say what the pattern costs**: complaints, refunds, churn, app-store and regulatory exposure, and the user's trust in every other button.
3. **Offer the honest version that serves the same goal.**

| Asked for | Offer |
|---|---|
| Exit pop-up with a discount | Show the discount on the page before they decide to leave, or follow up later by a channel they agreed to |
| Hide the cancel option | A clear cancel flow that also offers pausing or a cheaper plan, once |
| Pre-select the expensive plan | Recommend one plan with the reason it suits this user, and leave the choice open |
| Countdown timer | The real end date, if there is one |
| Force notification permission | Explain what they'd be notified about and ask at that moment |

Where to hold the line: things that deceive the user (fake timers, invented stock counts, ads disguised as content, charges that weren't disclosed) shouldn't be built, and you should say so plainly. Things that are merely pushy but honest are the product owner's decision. Raise the cost, offer the alternative, and if they still want it, build the gentlest version: easy to dismiss, shown once, clearly labelled.

## Sources

- Apps in Toss, "UI/UX 가이드" (dark pattern prevention policy): https://developers-apps-in-toss.toss.im/design/consumer-ux-guide
- Toss Tech, "토스에서 가장 안 좋은 경험 만들기": https://toss.tech/article/42221
- Toss Tech, "사용자를 위해 일부러 어렵게 만드는 경험, 어디까지 괜찮을까?": https://toss.tech/article/lockbank
- Toss Tech, "토스의 8가지 라이팅 원칙들" (suggest over force): https://toss.tech/article/8-writing-principles-of-toss
- The wider catalogue uses pattern names that are common in the design community.
