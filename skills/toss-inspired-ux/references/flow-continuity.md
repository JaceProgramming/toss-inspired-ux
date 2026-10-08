# Flow continuity

How to make a task that spans several screens feel like one motion. Read this for anything with navigation, more than one step, or a wait.

The numbers in this file are sensible defaults drawn from common platform practice. They are not Toss specifications. Toss has published its reasoning about motion, not its timing values.

**Contents:** The continuity contract · Transition grammar · Motion tokens · Patterns · Waiting · Interruptions · Keeping the user's place · Endings · Reduced motion, haptics and sound · When not to animate · Anti-patterns

## The continuity contract

Seven promises to the user. Animation helps with some of them. None of them can be fixed with animation alone.

1. **Every screen is predicted by the one before it.** The tapped label names what comes next, and the next title picks up the thread. Test: cover the next screen and ask what you expect to see.
2. **Nothing you gave us is lost or asked again.** Earlier answers stay visible, as a summary line, a chip in the top bar, or fields that remain on screen.
3. **The frame holds still.** Top bar, bottom button and progress indicator don't blink, re-mount or shift between steps. Only the content area changes.
4. **Space is consistent.** The same kind of move always uses the same direction. People build a map from this, and it tells them how to get back.
5. **Something is always on screen.** No blank page, no frozen frame, no content that jumps after loading.
6. **Nobody cuts in.** Nothing appears between the user and what they came to do.
7. **The end connects to the start.** The flow closes with a clear result and puts the user back where they began, with the change visible.

## Transition grammar

Choose by the relationship between the two states, not by taste. On native platforms the system's navigation transitions already follow this. Use them and don't replace them.

| From → to | Transition | Duration | Curve |
|---|---|---|---|
| Next step, or one level deeper | New content arrives from the trailing edge. Old content retreats slightly toward the leading edge and fades. | 300–350ms | decelerate |
| Back | The exact reverse | same or a little shorter | decelerate |
| Side task (pick, confirm, short edit) | Sheet rises from the bottom over a dimmed background | 300–400ms in, 200–250ms out | soft spring in, accelerate out |
| Blocking message | Dialog fades in, scaling up from about 96% | 200ms in, 150ms out | decelerate / accelerate |
| Item → its detail | The item itself expands or moves into the detail layout (shared element) | 300–400ms | decelerate or soft spring |
| Same screen, new content (tab, filter, stacked-form step) | Only the changed region cross-fades or slides a short distance. Neighbours move smoothly to make room. | 200–250ms | decelerate |
| A value changes (amount, count, progress) | Animate the value: digits roll, the bar grows. Width stays stable. | 200–400ms | decelerate |
| Touch feedback | Scale to about 97% or darken, immediately | 100ms down, 150–200ms up | decelerate |
| Toast | Slides a short distance and fades in from the edge it sits on | 200–250ms | decelerate / accelerate |
| First appearance of a list | Rows fade and rise 8–12px, staggered 30–40ms, first six rows at most | 250ms each | decelerate |
| Success | A check mark draws once, with one haptic tick | 400–600ms | decelerate |

Rules that apply to all of them:

- **Things entering decelerate** (fast start, soft landing). **Things leaving accelerate** and take less time than entering.
- **Short distances.** Slide web content 16–32px with a fade, not the full screen width, unless you are reproducing a native shell.
- **One motion per element.** If something moves and scales and fades with different timings, simplify.
- **Nothing longer than 400ms in navigation.** Something done many times a day should feel instant.
- **Animate only transform and opacity** where you can. Animating size or position properties causes jank on modest phones.
- **Input is never blocked by an animation.** A tap during a transition still counts.

## Motion tokens

Name them once and use them everywhere, so motion is as consistent as colour.

| Token | Value | Use |
|---|---|---|
| `duration.instant` | 100ms | press-down feedback |
| `duration.fast` | 150ms | fades, exits, release |
| `duration.base` | 250ms | small elements, in-place changes, toasts |
| `duration.screen` | 350ms | screens, sheets, shared elements |
| `duration.slow` | 500ms | one-off moments such as success |
| `ease.out` | `cubic-bezier(0.22, 1, 0.36, 1)` | entering, responding to a touch |
| `ease.in` | `cubic-bezier(0.32, 0, 0.67, 0)` | leaving |
| `ease.inOut` | `cubic-bezier(0.65, 0, 0.35, 1)` | moving from one place on screen to another |
| `spring.snappy` | stiffness 400, damping 40 (no overshoot) | default spring for UI |
| `spring.soft` | stiffness 300, damping 24 (slight overshoot) | sheets, cards, playful accents |

Use the bouncy spring sparingly. A slight settle at the end makes a floating card feel approachable. The same bounce on every button makes a product feel like a toy.

## Patterns

**Shared element.** The tapped card, thumbnail, amount or title travels to its position in the next view while the rest fades in around it. Tag exactly one or two elements. More than that turns into a swarm.

**Stacked reveal.** In a stacked form, existing fields slide to make room, the new field fades in at the insertion point, focus moves to it, and the title cross-fades to the new question. Everything else stays still.

**Large title collapse.** On scroll, the big title shrinks into the top bar instead of scrolling away, so the user never loses the name of the place they are in.

**Overlay instead of reflow.** When animating every row of a long list would be expensive, animate a single overlay that covers the area and swap the content underneath. Toss describes using this illusion to reorganise a home list cheaply.

**Entrance staging.** When a screen has a hero element, bring things in by importance: the headline first, then supporting detail, 60–100ms apart. Use this on landing and result screens, never on a step someone repeats daily.

**Shrink, don't vanish.** A floating prompt that the user scrolls past shrinks to a small handle that can reopen it. It signals "optional" without disappearing forever.

**Number change.** Count from the old value to the new, or roll the digits that changed. Use tabular figures and keep the container width fixed so the layout doesn't twitch.

**Skeleton to content.** Cross-fade over 150–200ms. The skeleton has the same sizes as the real content, so nothing shifts.

## Waiting

Work down this ladder. Only reach for the next rung when the one above is exhausted.

1. **Remove the wait.** Preload the likely next screen while the user reads this one. Fetch on press-down or hover instead of on release. Run requests in parallel. Start slow background work early, for example begin verification while the user fills the next field.
2. **Hide the wait.** Update optimistically for actions that are reversible and almost always succeed (like, toggle, add, reorder): change the UI at once, sync in the background, and on failure roll back with a toast that says why. Keep the previous content visible while refreshing.
3. **Shape the wait.** Skeletons in the layout of the result. Progress shown in the button that was tapped.
4. **Make the wait honest.** For a long operation, show what is really happening and reveal partial results as they arrive. Let the user leave and tell them when it's done.

| Wait | Show |
|---|---|
| under 100ms | nothing, it feels instant |
| 100–300ms | nothing new, the pressed state is enough (an indicator would only flash) |
| 300ms–1s | in-place indicator on the tapped control, or a skeleton |
| 1–3s | skeleton |
| 3–10s | skeleton or progress, plus a line saying what is happening |
| over 10s | real steps or partial results, a way to leave, a notification on completion |

Details that matter:

- Delay showing a skeleton by about 200ms, and once shown keep it at least about 400ms. Both prevent flicker.
- Render what you already know at once: titles, labels and buttons don't need a skeleton.
- Don't update optimistically for payments or anything irreversible. Show progress in the button, then a result.
- After a tap, the control is busy until the response. A second tap does nothing. Make the request safe to retry.
- After about 10 seconds with no response, offer to retry or go back, with every input preserved.
- Reserve space for images and late content so nothing jumps when it arrives.

## Interruptions

Things that cut into a task, and what to do instead:

| Tempting | Do instead |
|---|---|
| Permission prompts on first launch | Ask at the moment the feature is used, after a line of explanation |
| A promo or event sheet when a screen opens | A dedicated, clearly separate slot on a hub screen |
| A rating request during a task | Ask after a success, at most occasionally, with an easy "not now" |
| An announcement dialog | An inline banner that can be dismissed |
| A tutorial carousel before first use | Let the first screen be usable, and add a hint in context after the first success |
| "Are you sure you want to leave?" on back | Just go back. Confirm only when real work would be lost. |
| An upsell between two steps | Offer it on the result screen or the hub, never mid-flow |

## Keeping the user's place

- Back restores the previous screen exactly: scroll position, typed text, selected options.
- Leaving the app to copy a code or check a message, and coming back, changes nothing.
- Drafts of anything longer than a line are saved without asking.
- Closing a sheet or dialog returns focus to the control that opened it.
- A link from outside opens the specific screen, with a back stack that makes sense.
- **Web:** each step is a history entry with its own URL. Reload keeps the step. The browser's back button moves one step, and closes an open sheet or dialog before it leaves the page. A flow that swaps screens without touching history sends the user out of the product on their first Back. `assets/flow-shell.html` shows the wiring.
- **Android:** system back closes the top layer first (keyboard, then sheet, then screen).

## Endings

- A result screen or toast confirms what happened, in past tense, and says what happens next.
- "Done" returns to where the flow started, not to a generic home.
- The changed item is already updated when the user gets there. If it's in a list, scroll it into view and let a soft highlight fade over about a second.
- Offer one sensible next action at most. The moment after success is not an advertising slot.

## Reduced motion, haptics and sound

- When the system asks for reduced motion, replace slides, scales and springs with a fade of 150ms or less. Keep progress indicators. Stop anything that loops or autoplays.
- Never make an animation the only carrier of information. Pair it with text.
- **Haptics:** a light tick when a selection snaps or a toggle flips, a success pattern when something important completes, an error pattern when it fails. Not on every tap. Follow the system setting.
- **Sound:** off unless the context needs it (hands or eyes busy, or as an accessibility aid), and always paired with a visual signal.

## When not to animate

Toss's own interaction designers frame motion as something that must earn its cost: add it when it tells the user something a still screen can't, and skip it when a static design does the job equally well or the build cost outweighs the benefit.

Skip it for:

- actions repeated many times a session, and anything keyboard-driven,
- dense data views where movement makes values harder to compare,
- decoration: floating shapes, parallax, looping illustrations,
- anything that delays the content the user is waiting for.

## Anti-patterns

- A full-page fade through white, or a spinner, between every step.
- The same navigation going right-to-left in one place and bottom-to-top in another.
- A sheet opening another sheet, or a dialog on top of a sheet.
- The bottom button jumping as the keyboard opens or closes.
- Content appearing, then shifting when images or data arrive.
- Bounce on everything.
- Entrance animation replaying every time the user comes back to a screen.
- Auto-advancing when the user hasn't clearly finished (a full one-time code is fine, a half-typed name is not).
- An animation that must finish before the next tap registers.

## Sources

- Toss Tech, "인터랙션, 꼭 넣어야 해요?" (when motion earns its place, one motion per target, the overlay illusion): https://toss.tech/article/interaction
- Toss Tech, "첫 인터랙션 디자이너가 문제를 해결하는 법" (real information during long waits): https://toss.tech/article/1st_interaction_designer
- Toss Tech, "방해하지 않고, 눈에 띌 수 있을까" (layering, shrink on scroll, fast start with a soft settle): https://toss.tech/article/asset_management
- Toss Tech, "인터랙션으로 만드는 몰입 경험" (entrance staging, uninterrupted progression): https://toss.tech/article/interaction_simplicity
- Toss, "What makes a good product?" (Context Based, No More Loading): https://toss.im/tossfeed/article/tossproductprinciples
- Apps in Toss, "UI/UX 가이드" (entry, back and mid-flow interruptions): https://developers-apps-in-toss.toss.im/design/consumer-ux-guide
