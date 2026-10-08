---
name: toss-inspired-ux
description: "Product UX guidance inspired by the design principles Toss (Korean fintech) has shared publicly - one thing per screen, value before cost, questions answerable in 3 seconds, one clear action, screens that flow into each other, purposeful motion, plain friendly copy (Korean 해요체 rules included), humane error messages, no dark patterns. Use whenever you design, build, restyle or review user-facing UI - screens, forms, onboarding, sign-up, checkout, payment, settings, bottom sheets, dialogs, toasts, loading/empty/error states, screen transitions, or UX copy - on web or mobile (React, Next.js, React Native, Flutter, SwiftUI, Compose, plain HTML). Use it even if Toss is never mentioned, e.g. 'make this simpler', 'clean up this flow', '토스처럼', '토스 스타일', '화면 만들어줘', '문구 다듬어줘', 'UX 리뷰해줘', '에러 메시지 써줘'. Unofficial, not affiliated with Toss."
license: MIT
---

# Toss-inspired UX

Guidance distilled from what Toss has published about how it designs products: articles, conference talks, partner guidelines. It teaches the way of thinking, so it works with any brand and any stack.

Two boundaries:

- **Unofficial.** This is not Toss's design system. Don't put Toss's name, logo, fonts, icons or components into the product you build, and don't describe the result as "Toss design". Use the product's own brand. (Building a mini app that runs inside the Toss app? Then Toss's own docs are the rules: https://developers-apps-in-toss.toss.im/)
- **The project's design system wins on looks.** If the codebase already has components and tokens, keep them. This skill decides structure, flow, words and behaviour. Only when nothing exists, start from `references/visual-defaults.md`. Dense professional tools (admin tables, editors) keep their density; apply this to their flows, words and states.

## The idea underneath everything

Nobody should have to study a product in order to use it. Every rule here removes something the user would otherwise have to read, decide, remember, wait for, or worry about. When two rules pull in different directions, choose whichever leaves the user with less to figure out.

## How to work

Do these in order. Steps 1 to 3 are thinking, not UI, and they matter most: a "messy screen" is usually a flow problem. Scale the process to the task. A single button label needs only the Words section.

1. **Name the job.** One sentence in the user's words: what did they come to get done? ("Send money to a friend", not "Transfer module".) Anything that doesn't serve that job is a candidate for removal.
2. **Storyboard the flow before drawing a screen.** List the screens. Each gets one job: one message to take in, one question to answer, or one decision to make. For each, write the title the user reads, the one action, and what they see next:

   ```
   1. "Who are you sending to?"   recent people, or type an account   -> amount
   2. "How much?"                 keypad, balance in view             -> confirm
   3. "Send ₩50,000 to Minji?"    summary of both answers             -> [Send]
   4. "Sent"                      what happens next                   -> back where they started
   ```

   Open with what the user gets and close with the result. For three or more screens, put this storyboard at the top of your reply so the user can correct the flow and not only the pixels.
3. **Remove questions.** For every input ask: does the system already know this, can it infer it, default it, or ask later? Only what survives gets a screen.
4. **Build each screen** from the anatomy below.
5. **Write the words.** Titles, buttons and errors are the design, not decoration added afterwards.
6. **Design the unhappy states**: loading, empty, error and success, for every screen that talks to a server.
7. **Join the screens** so the flow reads as one continuous motion.
8. **Run the checklist** at the end and fix what fails before reporting done.

When asked to **review** existing UI instead of building, read `references/review-checklist.md` and report in its format.

## Ten principles and what they make you do

Toss describes its products with ten principles. In practice they turn into these moves:

| Principle | Do this |
|---|---|
| **One Thing** | One purpose and one primary action per screen, with a title you could read aloud as "this screen is for ___". Split a screen that asks two unrelated things. Don't split things answered in one thought (card number and expiry, street and city). |
| **Value First** | Show what the user gets before asking for effort, data, permission or money. A preview with sample data beats a paragraph of promises. |
| **Easy to Answer** | Every question answerable in about 3 seconds: concrete wording, few options, a recommended default. Breaking one hard question into a few easy ones is an improvement. Removing the question is better still. |
| **Clear Action** | The next step is obvious without reading: one full-width primary button, labelled with what happens ("Pay ₩12,000"), in the same place on every step. |
| **Explain Why** | Before any ask that feels like a cost (identity check, permission, personal data), one line on why it's needed and what the user gets for it. |
| **Context Based** | The whole journey reads as one story. Each screen is predicted by the one before, what the user already told you stays in view, and nothing interrupts. |
| **No More Loading** | Remove waits (preload, reorder, update optimistically) before decorating them. Never a blank screen or a bare spinner. |
| **Casual Concept** | Everyday words instead of internal or industry terms. If a word would need explaining, replace it. |
| **Simple Policy** | If a rule needs a paragraph, simplify the rule. Good UI can't rescue complicated conditions and exceptions. |
| **Minimum Features** | A new feature, option or setting is the last resort, because each one taxes every user. Prefer one good default to a preference. |

Tests, red flags and worked examples for each: `references/principles.md`.

## Screen anatomy

Defaults for a mobile-width flow:

```
┌──────────────────────────────┐
│ ←                            │  top bar: back, at most one extra action
│                              │
│ Who are you sending to?      │  title: big, bold, left-aligned sentence
│ Pick someone recent or       │  optional help line, quieter colour
│ type an account number.      │
│                              │
│  ┌────────────────────────┐  │
│  │ the one thing to do    │  │  content: one input, option list, or info
│  └────────────────────────┘  │
│                              │
│ ┌──────────────────────────┐ │
│ │           Next           │ │  one primary action, pinned to the bottom
│ └──────────────────────────┘ │
└──────────────────────────────┘
```

- **The title is the screen's one thing**, written as a sentence to the user. Not a noun label ("Recipient"), and not repeated in the top bar.
- **One primary action**, full width, pinned above the keyboard and the safe area. A second action is visually quieter (a text button). Two equal buttons means the screen has two things. If the button is disabled, the reason is visible on screen.
- **Hierarchy comes from size, weight and grey level**, not from boxes and lines. Separate groups with whitespace. No cards inside cards.
- **One accent colour**, reserved for the primary action and the thing to notice. Everything else is neutral. Red only for errors and destructive actions.
- **The number or name that matters most is the biggest thing on the screen.**
- **Tap targets are at least 44pt / 48dp.** A list row is tappable across its full width, and anything tappable looks tappable.
- **Understandable in three seconds.** If people must scroll to learn what the screen is for, it holds too much.
- **On wide screens** keep the single column, cap its width (about 360 to 480px for flows) and centre it. Don't spread a flow into a multi-column form.

Lists, dashboards and settings are still "one thing": one purpose per screen, grouped into clear sections, with one primary action if any.

Patterns (question step, stacked form, choice list, amount entry, consent, confirm, result, list rows) and how to choose between a new screen, sheet, dialog, menu and toast: `references/screen-patterns.md`.

## Flow continuity

The user should feel one motion from start to finish, never a pile of separate pages. Most of continuity is not animation. It is prediction, memory, and the absence of interruption.

- **Predict.** The label the user taps says what comes next, and the next screen's title picks up that thread. No screen is a surprise.
- **Remember.** Carry earlier answers forward where the user can see them: the recipient stays at the top while they type the amount, answered fields stay on screen as new ones appear. Never ask twice, never make them hold something in their head.
- **Keep the frame still.** Top bar and bottom button stay put between steps. Only the part that changes moves.
- **One spatial model.** Forward goes deeper (the new screen arrives from the trailing edge), back reverses it exactly, a side task rises as a sheet and returns you to the same spot, an in-place change morphs or cross-fades. Use the platform's native navigation transitions where they exist.
- **Connect the thing that was tapped.** When an item opens, grow or move that item into its new place instead of cutting to a new page. When a value changes, animate the value.
- **No dead air.** Acknowledge every touch within 100ms. Never show an empty screen: keep the previous content until the next is ready, or show a skeleton shaped like the result. If a long wait can't be avoided, show real progress.
- **No interruptions.** Nothing appears between the user and what they came for: no sheet or ad on entry, no pop-up on back, no permission prompt before the moment it's needed.
- **Don't lose their place.** Back restores scroll position and typed input. On the web each step is a history entry with its own URL, so the browser's back button moves one step instead of leaving the flow.
- **Land the ending.** Finish with a result that says what happened and what happens next, then return the user to where they started, with the changed item visibly updated.

Motion earns its place only when it does a job a still screen can't: confirm a touch, show where something came from or went, direct attention, or make a wait honest. If a static design does the job, ship static. Defaults: 100 to 150ms for touch feedback, 200 to 250ms for small elements, 300 to 350ms for screens and sheets. Things entering decelerate, things leaving accelerate, one motion per element, and movement becomes a plain fade when the user has asked for reduced motion.

Transition table, motion tokens and loading strategies: `references/flow-continuity.md`. Code for web, React Native, SwiftUI, Compose and Flutter: `references/motion-recipes.md`. A complete, tested web example of a step flow (history, transitions, busy button, toast): `assets/flow-shell.html`.

## Words

Write interface copy in the product's language. For Korean copy read `references/ux-writing-ko.md` first: the tone rules there (해요체, active, positive, light honorifics) are specific and easy to get wrong.

- **Write it the way you'd say it out loud** to someone who has never seen the product. Short sentences, everyday words, no internal terms, no error codes.
- **Cut** every word and sentence that doesn't change the meaning. Don't fill empty space with text.
- **One message per screen.** Leave out what the user already knows.
- **Buttons name the result**: "Send ₩50,000", "Save address", "Start free trial". Not "OK", "Submit" or "Confirm" when a specific verb exists. A button that only dismisses says "Close", not "Cancel", because "Cancel" makes people wonder whether their order was just cancelled.
- **Active and positive.** Say what the user can do rather than what is forbidden: "You can book again from 9 a.m." beats "Booking is unavailable."
- **Specific numbers** beat vague promises: "2 steps left", "takes about 1 minute", "₩5,000 off".
- **Suggest, never pressure.** No fear, guilt or fake urgency, and no shaming decline buttons.
- **Meet the moment.** Celebrate a real achievement, reassure when money or data is involved, apologise plainly when the fault is yours.

**Errors** follow one template: what happened, why (when it helps), what to do now, plus a button that does it. Better still, design the error out of existence: don't offer options that are going to fail, accept input in any format and tidy it yourself, and when the app can see a problem before the tap, say it inline before the tap instead of in a dialog after it. Promise only what is certain: "you haven't been charged" has to be true.

Full rules, error components, notification copy and before/after examples: `references/ux-writing.md`.

## Every state, not just the happy one

| State | Minimum bar |
|---|---|
| **Loading** | A skeleton in the shape of the content, or the previous content kept visible. The tapped button shows progress in place and can't be double-submitted. Waits over about 3 seconds say what is happening. |
| **Empty** | Says what will appear here and offers the one action that fills it. |
| **Error** | Field problem: inline under the field, as soon as it's knowable but not while the user is still typing. Brief and non-blocking: toast. Needs a decision or an explanation: dialog or full screen with a way forward. Never a dead end. |
| **Success** | Proportional: a toast for a small save, a result screen for something that matters. Says what happens next. |
| **Offline, denied, expired** | Explained in the user's terms, with the route back in. |

## Respect

Trust is the product. Don't build these, even when they would lift a metric this week:

- Anything that blocks entry: a sheet, an ad or a permission wall the moment a screen opens.
- Hijacking back or close to make one more pitch.
- A screen with no way to say no, or a decline that is hidden, tiny or shaming.
- Ads or upsells that appear mid-task without warning, or that are dressed as content.
- Buttons whose label hides what they do, pre-checked consent, costs revealed at the last step, fake urgency or scarcity.

Friction is fine only when it protects the user's own goal (hold to confirm something irreversible), works the same for everyone, shames no one, and leaves them in control. If you're asked to build something on this list, say what it costs and offer the honest version that serves the same goal. Alternatives for each: `references/dark-patterns.md`.

## Accessibility baseline

- The layout survives the system's largest text size and 200% zoom: text wraps, buttons grow, nothing essential is clipped.
- Text contrast is at least 4.5:1 (3:1 for large text and control outlines). Nothing is conveyed by colour alone.
- Every control exposes a name, a role and a state to screen readers. A row reads as one sensible sentence. Icon buttons are named for what they do.
- Dialogs and sheets trap focus and give it back. Toasts are announced. Nothing important lives only in a toast.
- The reduced-motion setting is honoured, nothing is communicated by animation alone, and sample data is unmistakably labelled as an example.
- When content continues below the fold, let part of it peek so people know to scroll.

More, including what older users trip on: `references/accessibility.md`.

## Before you say it's done

Walk the flow once as a first-time user:

1. Can I state each screen's one job in a sentence, and does its title say it?
2. Is there exactly one primary action per screen, labelled with its result?
3. Did I ask for anything the system could have known, inferred or defaulted?
4. Is every question answerable in three seconds?
5. Before each ask that costs something, did I show the value and the reason?
6. Does every tap respond within 100ms, with no blank or frozen moment anywhere?
7. Does each screen follow visibly from the last, with earlier answers still in view?
8. Does back (the app's, the browser's and the system's) always go back one step, with my input intact?
9. Are loading, empty, error and success designed for each screen?
10. Would the copy sound natural read aloud? Any jargon, blame, pressure or filler left?
11. Does it hold up at the largest text size, with a screen reader, and with reduced motion?
12. Is there anything here I'd be uncomfortable explaining to the user's face?

In your final message, don't recite this list. Say in a line or two what you actually ran and what you only read, then name anything that failed or that you couldn't check. Call something verified only if you ran it.

## Reference files

Open only what the task needs. Most tasks need one to three of these, and reading all of them spends context the work itself needs.

| Task | Read |
|---|---|
| Write or fix copy, error messages, notifications | `references/ux-writing.md` |
| Any Korean interface text | `references/ux-writing-ko.md` as well |
| Build a screen or a component | `references/screen-patterns.md` |
| Build a flow of several screens | `references/screen-patterns.md` and `references/flow-continuity.md` |
| Write transitions or animation | `references/motion-recipes.md`, your stack's section only |
| A plain HTML/JS flow, or unsure how Back, toasts and busy buttons should behave | `assets/flow-shell.html` |
| The project has no design system or tokens | `references/visual-defaults.md` |
| Review or audit existing UI | `references/review-checklist.md` |
| Growth, ads, consent, subscription, cancellation, permissions | `references/dark-patterns.md` |
| Forms, custom controls, or an accessibility question | `references/accessibility.md` |
| Deciding what to cut, or needing the reasoning behind a principle | `references/principles.md` |
