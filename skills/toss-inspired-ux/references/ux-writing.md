# UX writing

Rules for every word in the interface: titles, buttons, helper text, errors, notifications. They apply in any language. For Korean text also read `ux-writing-ko.md`, which covers tone and grammar that don't translate.

Examples are this guide's own. English and Korean versions are equivalents, not translations of each other.

**Contents:** Voice · Eight rules · Titles · Buttons and links · Numbers, money and time · Error messages · Empty, loading and success text · Notifications and promotional copy · Before shipping

## Voice

Toss names five values for its writing. Aim for all five at once:

- **Clear.** One reading is enough. A word means one thing everywhere.
- **Concise.** Only what is needed right now, in a form that can be scanned.
- **Casual.** How a helpful person talks, not how a contract reads.
- **Respectful.** Honest about costs and limits. Never trades trust for a tap.
- **Empathetic.** Aware of what the user is feeling at this moment.

When they pull apart: clear beats concise, and respectful beats persuasive.

## Eight rules

Toss publishes eight writing principles. Their names are in brackets.

**1. Hint at what comes next** (predictable hint). The user should know what the next screen is before tapping.

| Before | After |
|---|---|
| Continue | Verify my identity |
| Benefits | See my 3 coupons |
| 계속하기 | 본인 인증하기 |
| 혜택 보기 | 쿠폰 3장 보기 |

**2. Pull the weeds** (weed cutting). Delete any word whose removal doesn't change the meaning.

| Before | After |
|---|---|
| In order to proceed, you will first need to enter your email address below. | Enter your email. |
| 앞으로 매달 자동으로 결제가 진행될 예정이에요. | 매달 자동으로 결제돼요. |

**3. No filler sentences** (remove empty sentences). Text is not there to fill a gap in the layout, and one instruction is not said twice.

| Before | After |
|---|---|
| "Welcome to Settings! Here you can manage all of your settings." | (nothing) |
| 제목 "배송 방법을 골라 주세요" 아래 "아래에서 원하는 항목을 선택해 주세요." | (제목만) |

**4. Only the key message** (focus on key message). Give what the user needs for this decision. Leave out what they already know and what they can look up.

| Before | After |
|---|---|
| Your subscription will automatically renew every month on the day you first subscribed unless you cancel beforehand, and the payment method can be changed in settings. | Renews Nov 8 · ₩9,900 |
| 구독은 처음 가입한 날짜를 기준으로 매월 자동 갱신되며 해지 전까지 계속 결제됩니다. | 11월 8일에 9,900원이 결제돼요. |

**5. Easy to say** (easy to speak). If you wouldn't say it aloud to a friend's parent, rewrite it. No system terms, no stacked nouns, no sentences that need a second breath.

| Before | After |
|---|---|
| Authentication failed due to invalid credentials. | That password doesn't match. Try again. |
| 인증 정보가 유효하지 않아 인증에 실패하였습니다. | 비밀번호가 맞지 않아요. 다시 입력해 주세요. |

**6. Suggest, don't force** (suggest over force). Offer and inform. No threats, no guilt, no invented deadlines.

| Before | After |
|---|---|
| Don't miss out! Last chance! | You can get ₩3,000 off until tonight. |
| No thanks, I don't want to save money | Not now |
| 지금 안 하면 손해예요! | 오늘 밤까지 3,000원 할인받을 수 있어요. |
| 혜택 포기하기 | 다음에 할게요 |

**7. Words for everyone** (universal words). Understandable at any age and background, and harmless to all of them. No slang, memes, in-jokes or idioms that exclude.

| Before | After |
|---|---|
| This deal slaps, no cap | ₩5,000 off your first order |
| 핵이득 혜택 받아가세요 | 첫 주문 5,000원 할인 |

**8. Find the feeling** (find hidden emotion). Behind the transaction someone is relieved, worried, proud or annoyed. Say the thing a decent person would say.

| Before | After |
|---|---|
| Payment failed. | The payment didn't go through. You haven't been charged. |
| Goal complete. | You saved every week for 6 months. Well done. |
| 결제 실패 | 결제하지 못했어요. 돈은 빠져나가지 않았어요. |
| 목표 달성 | 6개월 동안 한 주도 빠짐없이 모았어요. 정말 대단해요. |

## Titles

- A title is a sentence addressed to the user. A question when you are asking ("What's your phone number?"), a statement when you are telling ("Your card is on its way").
- Two lines at most at normal text size.
- The title carries the screen. If the title were deleted and the screen still made sense, the title wasn't doing its job.
- Don't use a question as the label of a menu item or link. Some users read it as someone asking them something. "Which documents do I need?" becomes "See required documents".
- Don't repeat the title in the button.

## Buttons and links

- Verb plus object, naming the result: "Send ₩50,000", "Save address", "Delete 3 photos".
- One to three words where the language allows. Never wrap to two lines at normal size.
- The same action has the same label everywhere.
- "Next" is fine mid-flow when the following step is obvious. The final step names what actually happens.
- "Close" dismisses. "Cancel" is reserved for cancelling something that is in progress. Don't offer "Cancel" next to "Cancel order".
- Avoid "OK" and "Yes / No" for decisions. Name both outcomes: "Keep editing" / "Discard draft".
- Links describe their destination ("See fee details"), never "Click here".

## Numbers, money and time

- Be specific: "takes about 1 minute", "2 steps left", "3 questions".
- Prefer the amount to the percentage when you know it: "₩5,000 off" rather than "30% off".
- Prefer what is certain to what is possible. "Up to" claims read as advertising. Give the guaranteed or typical figure.
- Group digits and always show the unit or currency.
- Recent times are relative ("3 minutes ago", "tomorrow at 9 a.m."). Records use dates.
- Deadlines are exact, including the time: "until Oct 8, 11:59 p.m."
- When something is limited, put the limit in the message: "You can send up to ₩2,000,000 a day. ₩500,000 left today."

## Error messages

Six principles, in the order to apply them.

1. **Prevent it.** The best error message is the one that never appears. Remove options that would fail (don't offer a time slot that is already full). Accept input in any reasonable format and normalise it. Disable only what is impossible, and say why. If the app can see the problem before the tap (a minimum not reached, a limit exceeded, a field left empty), it is not an error dialog at all: say it inline, next to the control, before the user taps.
2. **Pick the right container.** A toast for something brief that needs no action. Inline text for a field. A dialog or a full screen when the user needs the situation, the reason and a way out. A paragraph squeezed into a toast will not be read in three seconds.
3. **Say what to do next.** This is the point of the message. "Biometric login failed" leaves people retrying forever. "Couldn't read your fingerprint. Use your PIN instead" moves them on.
4. **Use the user's words.** No codes, no exception names, no provider terminology. "Invalid input (422)" becomes "This phone number is already registered".
5. **Make the fix one tap.** If the fix is contacting support, the button opens support. If it is a setting, the button opens that setting.
6. **Soften the landing.** No blame. Say what is possible. Prefer "you can" to "you must".

One more rule sits over all six: **promise only what is certain.** "You haven't been charged" is the most reassuring sentence in a payment error, and it must be true. When the outcome is unknown (the connection dropped mid-request, a timeout), say that you couldn't confirm it, and say what is safe instead: "Trying again won't charge you twice." Then make sure the system keeps that promise.

**Template**

```
Title    what happened, in plain words
Body     why, if it helps; when it will be fine again; what is safe ("you haven't been charged")
Button   the fix, named
```

Write one message per situation and reuse it. The same problem should read the same on every screen.

| Situation | Title | Body | Button |
|---|---|---|---|
| Maintenance, end known | Bank maintenance until 2:30 a.m. | You can send again after that. | Close |
| Offline | Check your connection | What you entered is still here. | Try again |
| Connection lost while submitting | We couldn't confirm your order | Your basket is still here. Trying again won't charge you twice. | Try again |
| Card declined | This card couldn't be charged | Your card issuer didn't approve it. You haven't been charged. | Use another card |
| Session expired | Please sign in again | We signed you out to keep your account safe. | Sign in |
| Our fault, cause unknown | Something went wrong on our side | Try again in a moment. If it keeps happening, tell us and we'll look into it. | Try again |

| 상황 | 제목 | 내용 | 버튼 |
|---|---|---|---|
| 점검, 종료 시각 있음 | 지금은 은행 점검 시간이에요 | 오전 2시 30분부터 다시 보낼 수 있어요. | 닫기 |
| 오프라인 | 인터넷 연결을 확인해 주세요 | 입력한 내용은 그대로 있어요. | 다시 시도 |
| 요청 중 연결 끊김 | 주문이 됐는지 확인하지 못했어요 | 장바구니는 그대로 있어요. 다시 시도해도 두 번 결제되지 않아요. | 다시 시도 |
| 카드 승인 거절 | 이 카드로는 결제하지 못했어요 | 카드사에서 승인하지 않았어요. 결제된 금액은 없어요. | 다른 카드로 결제 |
| 세션 만료 | 다시 로그인해 주세요 | 안전을 위해 자동으로 로그아웃했어요. | 로그인 |
| 원인 모를 내부 문제 | 문제가 생겼어요 | 잠시 후 다시 시도해 주세요. 계속되면 알려 주세요. 바로 확인할게요. | 다시 시도 |

Inline messages are one short sentence that says how to fix it: "Enter all 16 digits", "Use at least 8 characters", "Add ₩2,500 more to reach the ₩15,000 minimum". They need no title, and they appear before the tap whenever the app already knows.

Context changes the rules. On a shared device or kiosk people look at one spot and don't read text elsewhere. Put the feedback where their eyes already are and use almost no words.

## Empty, loading and success text

- **Empty:** what will appear here, and the one action that fills it. "No payments yet. They'll show up here after your first one."
- **Loading:** no text for short waits. For long ones say what is actually happening ("Checking with your bank"), not "Please wait".
- **Success:** past tense, the outcome, then what happens next. "Sent ₩50,000 to Minji. It usually arrives within a minute."

## Notifications and promotional copy

Toss tested what makes people open a message. Six patterns held up, and all of them are compatible with being honest:

1. **One message.** A single reason to tap, not a list of benefits.
2. **Promise what is certain.** A small guaranteed reward outperforms a large "up to".
3. **Lighter action words.** "Take a look" asks less of the reader than "Register" or "Apply".
4. **Say what kind of thing it is first.** "New", "This month's list", "For you".
5. **Concrete numbers.** "3 questions", "5 photos to pick from" instead of "a survey" and "some photos".
6. **Everyday verbs.** A physical, familiar action ("pick a card", "scratch to reveal") is easier to picture than "participate" or "check".

Guardrails: the message must match what the user finds after tapping. Advertising is labelled as advertising. Nothing implies urgency or scarcity that isn't real.

## Before shipping

Read every string aloud, then check:

- Could any word go without changing the meaning?
- Would a first-time user know what happens when they tap each button?
- Is there a term here that only people inside the company use?
- Does each error say what to do next, and offer a button that does it?
- Is anything pressuring, blaming or shaming the user?
- Is the same thing called the same name everywhere?

## Sources

- Toss Tech, "토스의 8가지 라이팅 원칙들": https://toss.tech/article/8-writing-principles-of-toss
- Toss Tech, "좋은 에러 메시지를 만드는 6가지 원칙": https://toss.tech/article/21021
- Toss Tech, "가이드라인을 시스템으로 만드는 법": https://toss.tech/article/introducing-toss-error-message-system
- Toss Tech, "마케팅 문구 클릭률을 올리는 6가지 원칙": https://toss.tech/article/Marketing_Writing
- Toss Tech, "왜 아무도 에러 메시지를 읽지 않을까?": https://toss.tech/article/undercover-silo-4
- Toss Tech, "시니어 사용자가 어려워하는 UX 5가지": https://toss.tech/article/senior-usability-research
- Apps in Toss, "UI/UX 가이드" (UX writing section): https://developers-apps-in-toss.toss.im/design/consumer-ux-guide
