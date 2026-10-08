# Screen patterns

Building blocks for flows and hub screens. Each pattern states what it is for, how it is laid out, and the rules that keep it simple. Wireframes are mobile width. For wide screens see the last section.

**Contents:** Choosing a container · Question step · Stacked form · Choice step · Amount entry · Picking from a long list · Offer and explain screens · Consent · Review and confirm · Result · Hub screens and list rows · Detail screens · Navigation · Inputs · Toast, dialog and sheet rules · Wide screens

## Choosing a container

Pick the lightest container that does the job. Heavier containers interrupt more.

| The user needs to | Use | Notes |
|---|---|---|
| Do the next step of the main task, or give something full attention | **New screen** | Pushed, with back |
| Make a quick side choice or see extra detail without leaving (pick from a handful of options, a one-field edit, "are these details right?") | **Bottom sheet** | Closing it means "never mind" and changes nothing |
| Pick from a few options right where the finger already is (sort order, more actions) | **Menu** anchored to the trigger | Less travel than a sheet |
| Decide or acknowledge something before going on (irreversible action, an error that needs explaining) | **Dialog** | One or two buttons, never three |
| Know that something worked, or about a small non-blocking problem | **Toast** | One or two lines, disappears by itself |
| Fix a particular field | **Inline text** under the field | |
| Stay aware of a condition affecting the whole screen (offline, account restricted) | **Inline banner** at the top of the content | Not a pop-up |

Never stack containers: no dialog on a sheet on a sheet. If that seems necessary, the side task has grown into a screen.

## Question step

The workhorse of any flow. One question, one answer, one button.

```
┌──────────────────────────────┐
│ ←                            │
│ What's your phone number?    │  the question is the title
│ We'll text you a code.       │  why we ask, or what happens next
│                              │
│ 010-1234-5678                │  large input, focused, right keyboard
│ ──────────────────────────   │
│                              │
│ [          Next            ] │  enabled when the answer is valid
│ [ keyboard                 ] │
└──────────────────────────────┘
```

- The label is the title. Don't repeat it as a field label or a placeholder.
- Focus the input when the screen appears and open the right keyboard (numeric, email, one-time code).
- Format as they type (spaces in card numbers, hyphens in phone numbers) and accept pasted input in any format.
- If the answer has a fixed length and can't be wrong (a 6-digit code), advance automatically when it is complete. Otherwise wait for the button.
- Keep the button above the keyboard.

## Stacked form

Use this when several short answers belong to one thought, such as identity details. The screen stays, the question changes, and answers accumulate.

```
 step 1                    step 2                    step 3
┌───────────────────┐     ┌───────────────────┐     ┌───────────────────┐
│ What's your name? │     │ When were you     │     │ Which carrier?    │
│                   │     │ born?             │     │                   │
│ Kim Minji▏        │     │ 1996.04.12▏       │     │ ( pick one )      │
│ ───────────────   │     │ ───────────────   │     │ ───────────────   │
│                   │     │ Kim Minji         │     │ 1996.04.12        │
│                   │     │ ───────────────   │     │ ───────────────   │
│                   │     │                   │     │ Kim Minji         │
│ [     Next      ] │     │ [     Next      ] │     │ ───────────────   │
└───────────────────┘     └───────────────────┘     └───────────────────┘
```

- The newest field enters at the top, where the eye and the keyboard focus are. Earlier answers slide down and stay visible and editable.
- The title changes to the current question. Nothing else on the screen jumps.
- Earlier answers show their label (small, above the value) once they are no longer the active question.
- This is one screen in the navigation stack. Back leaves the form unless you deliberately handle it step by step. If you do, be consistent.
- Stop at four or five fields. Beyond that, start a new screen with a new theme.

A tested web implementation of the surrounding plumbing (one history entry per step, answers kept on Back and reload, busy button, toast) is in `assets/flow-shell.html`.

## Choice step

```
┌──────────────────────────────┐
│ ←                            │
│ How should we deliver it?    │
│                              │
│  🚚  Standard      Free      │  whole row tappable
│      Arrives Thu             │
│  ─────────────────────────   │
│  ⚡  Express      ₩3,000     │
│      Arrives tomorrow        │
│                              │
└──────────────────────────────┘
```

- Two to five options visible without scrolling. More than that is a long list (below).
- Each option says what choosing it leads to, in a word or a line.
- Put the recommended or most common option first, and mark it only if you can say why.
- With a single choice that is easy to undo with back, tapping the option moves on. No extra "Next". With multiple choice, or when the choice commits to something, keep the button.
- Say "pick one" or "pick all that apply" in the question when it isn't obvious.

## Amount entry

```
┌──────────────────────────────┐
│ ←  To Minji · Kakao 3333…    │  earlier answer stays in view
│                              │
│ How much?                    │
│                              │
│        50,000 won            │  the biggest thing on screen
│   Balance 1,204,300 won      │  the fact needed to decide
│                              │
│ [+10k] [+50k] [+100k] [All]  │  shortcuts for common answers
│ ┌──────────────────────────┐ │
│ │  1    2    3             │ │
│ │  4    5    6             │ │
│ │  7    8    9             │ │
│ │  00   0    ⌫             │ │
│ └──────────────────────────┘ │
│ [          Next            ] │
└──────────────────────────────┘
```

- Format with separators and a unit as the user types. Use tabular figures so the number doesn't wobble.
- Show the limit or balance next to the number. When the number goes over, say so there, at once, in words that tell them the maximum.
- For large values in Korean, echo the amount in words below ("5만 원").

## Picking from a long list

Banks, countries, schools, products.

- Before the list: try to make the question unnecessary (infer it, or offer the top guesses).
- Then: recent and most common choices at the top, then search, then the full list in a predictable order.
- Search matches the way people actually type: partial words, initial consonants in Korean, common nicknames.
- An empty search result offers a next step ("Can't find it? Enter it yourself").

## Offer and explain screens

Use before asking for effort, data or permission.

```
┌──────────────────────────────┐
│ ←                            │
│ See how your spending        │  the value, concretely
│ compares to people your age  │
│                              │
│   ┌──────────────────────┐   │
│   │  [sample chart]      │   │  realistic preview,
│   │         Example      │   │  labelled as an example
│   └──────────────────────┘   │
│                              │
│ We need to verify it's you   │  the cost, with its reason
│ first. Takes about a minute. │
│                              │
│ [    Verify and see mine   ] │
│          Not now             │  a real way to decline
└──────────────────────────────┘
```

- Value first, then the ask, then the reason and the size of the ask (time, steps).
- One focal graphic at most, and only if it shows the outcome. Decoration doesn't earn the space.
- The decline is an ordinary, visible control. Declining returns the user to where they were, without penalty or a second pitch.
- For OS permissions: this screen comes first, the system prompt follows the tap. If the user declines the system prompt, keep the feature reachable with a note on how to turn it on later.

## Consent

```
┌──────────────────────────────┐
│ Agree to get started         │
│                              │
│ ☑  Agree to all              │  one tap for people who trust you
│ ─────────────────────────    │
│ ☑  Terms of service (required)        ›
│ ☑  Privacy policy (required)          ›
│ ☐  Marketing messages (optional)      ›
│                              │
│ [    Agree and continue    ] │
└──────────────────────────────┘
```

- Mark every item required or optional in words, not with an asterisk. Each one opens to its full text.
- Nothing is pre-checked. "Agree to all" visibly includes the optional items, and unticking any optional item afterwards is one tap.
- The button label says what agreeing leads to.
- Only ask for consent that this step needs. Ask for the rest when the feature that needs it is first used.

## Review and confirm

Use before anything that moves money, sends something to another person, or can't be undone.

```
┌──────────────────────────────┐
│ ←                            │
│ Send ₩50,000 to Minji?       │  the whole action in one sentence
│                              │
│ To          Kim Minji        │
│ Account     Kakao 3333-01…   │
│ From        My checking   ›  │  editable rows open the earlier step
│ Fee         Free             │
│                              │
│ [        Send ₩50,000      ] │
└──────────────────────────────┘
```

- Show everything the user decided, plus anything you added (fees, dates), in plain rows.
- Rows that can change are tappable and return to the confirm screen afterwards.
- The total cost appears here at the latest, and preferably earlier.
- The button repeats the essential fact (amount, recipient, plan).
- Routine, reversible actions don't need this screen. Offer undo in a toast instead.

## Result

```
┌──────────────────────────────┐
│                              │
│            ✓                 │  one figure: drawn check, or illustration
│                              │
│   Sent ₩50,000 to Minji      │  what happened, in past tense
│   It usually arrives         │  what happens next
│   within a minute.           │
│                              │
│ [          Done            ] │  returns to where the flow started
│       Send another           │  optional, quieter
└──────────────────────────────┘
```

- Title states the outcome. The description states what happens next or what the user can now do.
- Failure uses the same layout, an honest title, the reason in the user's terms, and a primary action that fixes it or tries again.
- One primary action. No cross-selling here: the user just trusted you with something.
- Small results (saved, copied, added) get a toast, not a screen.

## Hub screens and list rows

Home, lists and settings hold many things but one purpose each.

```
┌──────────────────────────────┐
│ Wallet                    ⚙  │  large title, one utility action
│                              │
│ Accounts                     │  section header
│ ┌──┐ Checking       1,204,300│
│ └──┘ Kakao · 3333     Send › │  row: leading visual, two lines,
│ ┌──┐ Savings        5,000,000│       trailing value or action
│ └──┘ 3.2% a year             │
│                              │
│ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  │  thick quiet band between sections
│ This month                   │
│  Spent              482,000  │
└──────────────────────────────┘
```

- **Row anatomy:** leading visual (icon, logo, avatar) · title · optional second line · trailing value, chevron or small button. Keep one arrangement throughout a list.
- The whole row is the tap target and shows a pressed state. A small trailing button is a separate, larger-than-it-looks target.
- Section headers are short nouns. Sections are separated by space or a thick quiet band, not by boxes around each row.
- Show the three to five most useful items, then "See all". Don't paginate a dashboard.
- Numbers align right, in tabular figures. Negative or outgoing amounts need a sign or a word, not only a colour.
- Promotions get their own clearly separate slot. They never sit inside a list of the user's own data, where they will be mistaken for it.

## Detail screens

- Lead with the fact people came for (the amount, the status, the date). Everything else follows in order of usefulness.
- Status is a word plus a short explanation, not only a coloured dot.
- At most one primary action. Secondary actions go in a menu or a quieter row.
- Raw identifiers and technical data go at the bottom, with a copy button.

## Navigation

- **Top bar:** back on the leading side, at most one action on the trailing side. In flows, leave the centre empty and let the large title speak. Don't add a close button and a back button to the same bar unless they do different things, and then label the close.
- **Bottom tabs:** two to five destinations, each a place and not an action. Icon plus a one- or two-word label. The current tab is marked by more than colour. Tabs don't appear inside a flow.
- **Progress:** in short flows the changing title is enough. For four or more steps, a thin bar or "2 of 4" helps. Never show a step count that can change mid-flow.
- **Leaving a flow:** if progress would be lost, confirm once with a dialog that names what is lost and offers to keep going. If nothing is lost, just leave.

## Inputs

- Text large enough to read back at a glance (17px or more). Generous height. A clear button once there is content.
- One input in focus. Move focus for the user when the next field is obvious.
- Pick the keyboard, autofill hint and capitalisation to match the content. Let the system fill one-time codes.
- Validate when it can help: as soon as the value is certainly wrong, or when the user leaves the field. Not on the first keystroke.
- Error text replaces the helper text under the field and says how to fix it.
- Never clear what the user typed because of an error elsewhere.

## Toast, dialog and sheet rules

**Toast**
- One at a time. A new one replaces the old.
- It always goes away by itself. A toast that stays on screen, or an empty toast container that is visible, is a bug.
- About 3 seconds. About 5 seconds when it carries an action (Undo, View).
- Sits above the bottom button, never over it.
- One sentence. If it needs two buttons or a paragraph, it is not a toast.

**Dialog**
- Title says what is being decided. Body adds only what is needed to decide.
- Two buttons at most. The one that just dismisses says "Close". The one that acts names the action ("Delete photo"). Use "Cancel" only when it truly cancels something in progress.
- Destructive action is styled as destructive and is not the default focus.
- Tapping outside does the same as "Close".

**Bottom sheet**
- A title, then the content, then at most one primary button.
- Closes by swipe down, tap on the dimmed area, or system back. All three mean "never mind".
- Rises above the keyboard when it contains an input.
- Height fits the content, up to most of the screen. Longer content scrolls inside, with a visible cue.
- Never opens by itself when a screen appears.

## Wide screens

- Keep flows as one column, 360 to 480px wide, centred, with the button at the end of the column or in a sticky footer of the same width.
- Hub screens can use two columns: a list on the left and the selected item's detail on the right. This replaces the push transition with an in-place update.
- Sheets become centred dialogs or side panels. Toasts move to a bottom corner.
- Hover and keyboard focus states are required. Enter submits the step, and Escape closes the topmost layer.

## Sources

- Toss Tech, "제품이 커지면 디자인 시스템 가이드는 어떻게 개선돼야 할까?": https://toss.tech/article/toss-design-system-guide
- Toss Tech, "이런 것도 컴포넌트로 만들어도 될까?" (menu at the point of interaction): https://toss.tech/article/tds-component-making
- Toss Tech, "토스 디자이너가 제품에만 집중할 수 있는 방법": https://toss.tech/article/toss-design-system
- Toss Tech, "토스에서 가장 안 좋은 경험 만들기" (ads inside the user's own data): https://toss.tech/article/42221
- Apps in Toss, "UI/UX 가이드": https://developers-apps-in-toss.toss.im/design/consumer-ux-guide
- General structure of toast, dialog, sheet and result components follows conventions documented publicly for TDS Mobile: https://tossmini-docs.toss.im/tds-mobile/
