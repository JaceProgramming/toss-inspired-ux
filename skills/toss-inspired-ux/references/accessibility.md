# Accessibility

Simple for everyone includes people using large text, a screen reader, one hand, a cracked screen in sunlight, or their first smartphone. Most of what follows also makes the product better for everybody else.

**Contents:** Text size and zoom · Colour and contrast · Touch · Screen readers · Focus and keyboard · Motion, time and sound · Older and first-time users · Platform quick reference · Quick tests

## Text size and zoom

- Set text in units that follow the user's setting: `rem` on the web, Dynamic Type styles on iOS, `sp` on Android. Don't switch font scaling off.
- At the largest setting: text wraps instead of truncating, buttons grow taller, rows grow, and a side-by-side layout stacks. Nothing essential is cut with an ellipsis.
- Fixed heights are the usual culprit. Use minimum heights.
- A long word or number must wrap or shrink before it overflows its container.
- Icons next to text scale with it, or at least stay aligned to the first line.
- Limit scaling only on chrome that would otherwise break (tab bar labels, a compact top bar), and never on content.

Toss describes rebuilding its sign-up form specifically because the layout broke when people enlarged their system text, a setting common among its growing number of older users.

## Colour and contrast

- Body text 4.5:1 against its background. Large text (24px, or 19px bold) and the outlines of controls 3:1.
- Placeholder text counts as text. If it can't reach 4.5:1, use a label instead of a placeholder.
- Don't signal with colour alone: add an icon, a word, a sign, or a shape. Required fields, errors, selected tabs, gains and losses.
- Equal lightness steps should look equally light across hues. A yellow and a blue at "the same step" rarely are. Check each one.
- Check contrast again in dark mode. Passing in light mode says nothing about dark.

## Touch

- Targets at least 44×44pt on iOS and 48×48dp on Android and the web. The visible glyph can be smaller if the hit area isn't.
- At least 8px between separate targets.
- The whole row is the target, not only its text or chevron.
- Primary actions sit where a thumb reaches: the lower half of the screen.
- No action depends on a gesture alone. Swipe-to-delete has a button equivalent. A sheet that closes by swipe also has a close control.
- Hold, drag and multi-finger gestures always have a single-tap alternative.

## Screen readers

Every control tells the user three things: its **label** (what it is), its **role** (button, tab, switch), and its **state** (selected, on, disabled, expanded).

- **Order matters.** People who use screen readers listen fast and skip ahead as soon as they recognise something. Put the identifying word first. Reading order follows visual order, top to bottom.
- **One row, one stop.** Group a list row so it reads as a sentence: "Checking account, 1,204,300 won, button". Not five separate fragments.
- **Name by function, not appearance.** "Close", not "X". "Send to Minji", not "Arrow".
- **Hide decoration.** Purely decorative images and icons are skipped.
- **Numbers read naturally.** An amount is announced as one number with its unit, not digit by digit, and not once per frame while it animates.
- **Announce what changes.** A toast is a polite live announcement. An error that blocks progress is an assertive one. New content that loads in is announced once.
- **Move focus on purpose.** After navigation, focus lands on the new screen's title. When a dialog or sheet opens, focus goes inside and stays inside. When it closes, focus returns to the control that opened it.
- **Give every sheet a real close button**, even if sighted users can swipe it away. It can be visually subtle. It must be reachable.
- **Headings mark structure.** The screen title is a heading. Section headers are headings. People navigate by them.

For flows that depend on a camera or on positioning (face or document capture), sight can't be assumed at all. Toss's accessibility work on face recognition replaced visual cues with a progress sound and a completion sound, removed a button that had to be found and tapped after each failure in favour of retrying automatically, and broke the instructions into stages that are spoken before each one begins.

## Focus and keyboard

Web and desktop, and anyone with a keyboard or switch attached to a phone.

- Everything interactive is reachable with Tab, in visual order.
- A visible focus ring on keyboard focus (`:focus-visible`), with at least 3:1 contrast.
- Enter submits the current step. Escape closes the topmost layer.
- Content behind an open dialog or sheet can't be focused (`inert`, or the native `<dialog>`).
- Custom controls copy the keyboard behaviour of the native ones they replace. Prefer the native element.

## Motion, time and sound

- Honour the reduced-motion setting: movement becomes a short fade.
- Nothing flashes more than three times a second.
- Nothing is conveyed by animation alone. There is always text.
- No time limits on reading or acting. If a limit is unavoidable (a one-time code), show it and offer a new one without starting over.
- A toast with an action stays longer, and the same action is available somewhere that doesn't disappear.
- Sound is never the only signal, and never plays unasked in a normal flow.

## Older and first-time users

Toss's usability research with senior users found five recurring problems. Each fix helps anyone who is distracted, hurried or new.

| Problem observed | Fix |
|---|---|
| **Doesn't recognise what can be tapped.** Bare text, or text with a small chevron, isn't seen as a control. | Give actions a button shape or a clearly tappable row. Don't make people guess. |
| **Takes sample data for real data.** An example screen with a name and amounts is read as their own account, especially when it moves. | Make examples visibly different, label them, and avoid personal-looking details. |
| **Copies an instructional animation literally.** The demonstration is taken as the exact movement to reproduce. | Pair every demonstration with a plain written instruction. Keep the animation schematic. |
| **Reads a question label as a question to them.** "Which documents are needed?" causes confusion about who is asking. | Label navigation with an action: "See required documents". |
| **Doesn't scroll.** If nothing suggests more content, what is below the fold doesn't exist. | Let the next item peek above the fold, or add a visible "more" control. |

Also:

- Prefer an explicit button to auto-advancing. Moving on without being asked is disorienting unless the input is unambiguous, like a complete one-time code.
- Avoid relying on icons alone. Pair them with a word.
- Familiar patterns beat clever ones. If a standard control exists, use it.

## Platform quick reference

| | Label / role / state | Group a row | Announce | Text scaling |
|---|---|---|---|---|
| **Web** | semantic elements first, then `aria-label`, `role`, `aria-pressed` / `aria-selected` / `aria-expanded` | one interactive element wrapping the row | `aria-live="polite"`, `role="alert"` | `rem`, no fixed heights |
| **React Native** | `accessibilityLabel`, `accessibilityRole`, `accessibilityState` | `accessible` on the row container | `AccessibilityInfo.announceForAccessibility` | font scaling on by default, keep it |
| **SwiftUI** | `.accessibilityLabel`, `.accessibilityAddTraits`, `.accessibilityValue` | `.accessibilityElement(children: .combine)` | `AccessibilityNotification.Announcement` | text styles (`.body`, `.title`) |
| **Compose** | `contentDescription`, `Role`, `stateDescription` | `Modifier.semantics(mergeDescendants = true)` | `liveRegion` semantics | `sp` |
| **Flutter** | `Semantics(label:, button:, selected:)` | `MergeSemantics` | `SemanticsService.announce` | respects `MediaQuery.textScaler` by default |

## Quick tests

Do these before calling a screen finished. Each takes under a minute.

1. Set system text to its largest size. Is everything readable and reachable?
2. Turn on the screen reader and complete the main task without looking.
3. On the web, complete it with the keyboard only.
4. Switch to greyscale. Does every state still read?
5. Turn on reduced motion. Does anything still slide or bounce?
6. Use it one-handed, thumb only.
7. Switch to dark mode and check contrast again.

## Sources

- Toss Tech, "시니어 사용자가 어려워하는 UX 5가지": https://toss.tech/article/senior-usability-research
- Toss Tech, "시각장애인에게 '읽는 순서'가 왜 중요할까?": https://toss.tech/article/voiceover_usability
- Toss Tech, "시각 정보를 소리로 번역하는 법": https://toss.tech/article/accessibility_face
- Toss Tech, "토스 디자이너가 제품에만 집중할 수 있는 방법": https://toss.tech/article/toss-design-system
- Toss Tech, "완성 없는 이야기, 가입 과정 개선": https://toss.tech/article/signup
- Toss Tech, "달리는 기차 바퀴 칠하기: 7년만의 컬러 시스템 업데이트": https://toss.tech/article/tds-color-system-update
- Contrast and target-size thresholds follow WCAG 2.2 and the Apple and Android platform guidelines.
