# The ten principles, in practice

Toss describes its products with ten principles (sources at the end). The names are theirs. The tests, red flags and examples below are this guide's own reading of them. Use this file to plan a flow, to decide what to cut, and to explain a design choice to someone else.

**Contents:** One Thing · Value First · Easy to Answer · Clear Action · Explain Why · Context Based · No More Loading · Casual Concept · Simple Policy · Minimum Features · Thinking from the user's side · When polish doesn't work

## One Thing

Each screen delivers a single message or asks for a single decision.

- Write the screen's purpose as one sentence. If the sentence needs "and", it is two screens, or one half doesn't need to exist.
- One primary action. Other routes are quieter, or live behind "more".
- Split by thought, not by field. Things read off the same card or pulled from the same memory can share a screen. Things that need a different kind of thinking get their own.
- On hub screens (home, settings) the unit is the section: one purpose per section, each leading to a focused screen.
- When recommending, lead with the single best option and say why it is the best ("chosen most often", "fits how you spend"). Put the rest behind "see all". Toss reported that showing only the top-ranked card converted better than showing a ranked top ten.

Red flags: two buttons of equal weight; a title that is a category name; tabs inside a step of a flow; helper text explaining how to use the screen; an unrelated promo inside a task screen.

Don't over-apply it. Twelve trivial screens are not simpler than four sensible ones. If two questions are always answered together in a second, keep them together.

## Value First

Communicate the benefit before the cost. Cost means effort, time, personal data, permission, money or commitment.

- Open with what the user will get, shown concretely: a preview with realistic sample data (clearly marked as an example), a number, an outcome.
- Order the steps so the first payoff comes before the first ask where possible. Let people look around before sign-up. Show the result, then ask them to save it.
- Make the value specific: "₩5,000 off today" rather than "great discounts".
- Budget the cost. Toss's rule of thumb: fatigue sets in around three or four questions, a question that needs more than about five seconds of thought makes people hesitate, and sensitive data such as income raises resistance sharply. When a flow goes past that, show more value first or cut asks.

Example: Toss's insurance lookup needed several verification steps. Showing a sample of the comparison chart before asking for verification raised completion (they reported 67% more people finishing).

Red flags: a sign-up wall as the first screen; permission prompts at launch; a form before any explanation; a paywall before any preview.

## Easy to Answer

Every question should be answerable within about three seconds, without hesitation.

- Ask about facts the user has to hand, not about process choices whose consequences they can't picture. Toss replaced "get expert help or do it yourself?" with "do you have the documents?" and reported that drop-off at that step roughly halved.
- Use concrete wording, and where it helps, a recognisable picture or icon per option.
- Show only the options that apply to this user. Route everyone else somewhere else.
- Pre-select the likely answer. Recommend one. Offer recent choices first.
- Replace typing with picking, and picking with confirming a guess. Toss's transfer flow takes the account number first and then suggests the bank, instead of making people search a long bank list. When a single guess wasn't accurate enough they offered three, rather than forcing a wrong answer.
- Put detail behind "learn more" instead of inside the question.
- People follow the question's wording, not the control's shape. In a Toss test most participants chose one or several answers according to the question even when radio buttons and checkboxes were swapped. So make the question itself say "pick one" or "pick all that apply".

Red flags: a free-text field for an answer with a known set of values; options that need definitions; a choice the user must make before knowing what the options mean; a yes/no where it is unclear what "yes" sets in motion.

## Clear Action

The next step should be obvious without reading anything.

- One primary button, at the bottom, full width, in the same place on every step.
- The label is a verb plus the result. On the step that actually pays, the button says "Pay ₩12,000", not "Next".
- The button doesn't repeat the title and doesn't make a vague promise. "Check benefits" becomes "See my 3 coupons".
- Destructive actions look different and are never the default.
- Tappable things look tappable. Don't rely on bare text plus a chevron for the main route: older users in Toss's research often didn't recognise it as something to tap.

Red flags: the body copy has to be read to learn what the button does; two calls to action; a primary action styled as a link.

## Explain Why

Tell people why they are being asked, so they can go on with confidence.

- One sentence of purpose before identity verification, permissions and sensitive fields, placed where the ask happens, not in the terms.
- State the scope of anything ambiguous: "Only you can see this note."
- For OS permissions: explain in your own UI first, trigger the system prompt at the moment the feature is used, and give a way forward if they decline.

Examples: Toss tried removing steps from Android sign-up and saw almost no change. Completion rose only after a line was added saying identity verification was needed, before the name field. Adding context beat removing steps. Separately, users kept writing questions for merchants into a memo field until the label said that only they could see it.

Red flags: system permission dialogs at launch; "Enter your ID number" with no reason; required fields that look irrelevant to the task.

## Context Based

The whole journey must feel natural from the first tap to the last. Fragments drive people away.

- Design entry to exit as one piece, including where the user came from. The first screen must match the promise of the push, banner or link they tapped.
- Echo the tapped label in the next title. Carry data forward. Use one name for one thing all the way through ("send" does not become "transfer" and then "remit").
- At the end, return people to where they started.
- Organise by what users do, not by how the company is organised. Toss rebuilt a catch-all "my documents" area, which mirrored internal teams, into separate products for checking, issuing and paying.

Red flags: landing on a generic home after tapping a specific offer; re-entering something already given; a term changing name mid-flow; a modal from another context cutting in.

## No More Loading

Waiting makes people leave. Attack the wait in this order:

1. **Remove it.** Preload on intent, run requests in parallel, cache, and reorder the flow so slow work happens while the user is busy with the next step.
2. **Hide it.** Update optimistically, keep the previous content on screen, show a skeleton in the shape of what's coming.
3. **Make it honest.** For long waits, show what is really happening. Toss replaced a decorative loop during a 20 to 30 second loan check with results appearing as they came in, which made the wait feel credible and nudged completion up.

Red flags: a full-screen spinner; a spinner after every tap; "please wait" with no end in sight; a progress bar that isn't measuring anything.

## Casual Concept

Replace technical vocabulary with words anyone already uses.

- Name features after what people do with them. Toss renamed a "certificate issuance" entry to the everyday phrase people use for fetching that document.
- Test: would the user say this word to a friend? If not, replace it or explain it once in passing.

Red flags: a feature name that needs a subtitle; internal code names; legal terms used as labels.

## Simple Policy

The rules underneath must be intuitive. Polished design can't compensate for a complicated policy.

- When a screen needs a paragraph of conditions, push back on the rule: fewer tiers, no exceptions, one limit.
- Standardise instead of mirroring every backend's quirks. Toss stopped reproducing each agency's response wording and normalised the messages.
- Collapse internal states into the ones the user cares about. Toss reduced nine internal remittance states to three: processing, arrived abroad, done.

Red flags: asterisks; "except when"; eligibility matrices; tooltips that explain rules.

## Minimum Features

Every feature adds complexity, slows releases and breeds bugs. Adding one is the last resort.

- Before adding, ask: can an existing feature do it? Can a default replace the option? Who needs this, and how often?
- Settings are features too. Prefer one good default.
- Remove features and entry points that are barely used.

Red flags: a settings page that keeps growing; "advanced options"; three entry points to the same thing.

## Thinking from the user's side

- **Familiar beats novel.** Use patterns people know from every other app before inventing a gesture. Toss swapped a custom pull-down drawer for ordinary tabs and saw calendar usage rise about fourfold.
- **Four sentences that mean you've stopped seeing it as a user:** "isn't this obvious?", "it's a bit hard but fine for now", "this is just inherently complex", "it's only an experiment". Treat each as a to-do. Users can't tell an experiment from the product.
- **Reduce anxiety, not just cost.** When money or data goes somewhere the user can't see, show where it is. Confirm an input by showing what it means (type a bank code, see the bank's name). Prefer search-and-pick over free typing for addresses.
- **Don't trust your assumptions about users.** Watch someone use it, and check the numbers for signs of confusion, such as a field being used for something it wasn't meant for.

## When polish doesn't work, change the structure

If rewording and restyling barely move the result, two purposes are probably competing for the same space. Toss's asset report sat inside the account list and was ignored. Rewriting it in a friendlier voice helped only slightly. Moving it into its own floating layer above the list tripled both taps and follow-through.

A prompt can be noticeable without interrupting when it:

- lives in its own layer instead of competing with the content,
- gets out of the way as the user scrolls,
- can be closed at any time and reopened later,
- carries little decoration, so the content is what gets read.

## Sources

- Toss, "What makes a good product?" (the ten product principles): https://toss.im/tossfeed/article/tossproductprinciples
- Toss Tech, "토스 디자인 원칙 Value first, Cost later": https://toss.tech/article/value-first-cost-later
- Toss Tech, "토스 디자인 원칙, Easy to answer": https://toss.tech/article/insurance-claim-process
- Toss Tech, "추천할 때는 제일 좋은 것 하나면 된다": https://toss.tech/article/recommend-just-one
- Toss Tech, "송금할 때 은행 이름을 꼭 입력해야 할까요?": https://toss.tech/article/toss-money-transfer-bank-recommendation
- Toss Tech, "완성 없는 이야기, 가입 과정 개선": https://toss.tech/article/signup
- Toss Tech, "우리에게 당연하지만 사용자는 아닌 것들": https://toss.tech/article/thinking-user-perspective
- Toss Tech, "크고 복잡한 제품, 과감하게 갈아엎기": https://toss.tech/article/mydoc
- Toss Tech, "도메인 지식 없는 디자이너가 팀의 기준을 바꾼 방법": https://toss.tech/article/remittance_transfer
- Toss Tech, "방해하지 않고, 눈에 띌 수 있을까": https://toss.tech/article/asset_management
- Toss Tech, "[미디클] 라디오 vs 체크박스 뭐가 좋을까? EP.1": https://toss.tech/article/mysterydesignclub1
- Toss Tech, "시니어 사용자가 어려워하는 UX 5가지": https://toss.tech/article/senior-usability-research
- Toss Tech, "인터랙션, 꼭 넣어야 해요?": https://toss.tech/article/interaction
