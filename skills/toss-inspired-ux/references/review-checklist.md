# Reviewing existing UI

Use this when asked to review, audit, critique or "clean up" a screen or flow that already exists, from code, screenshots or a description.

**Contents:** How to review · Quick tests · Rubric · Severity · Report format · Applying fixes

## How to review

1. **Find the job.** What did the user come here to do? If you can't tell from the screen, that is the first finding.
2. **Walk the flow in order**, as a first-time user who has never seen the product and is slightly distracted. Note where you hesitate.
3. **Structure before surface.** Check the flow and the purpose of each screen before copy, and copy before spacing and colour. A well-worded screen that shouldn't exist is still a problem.
4. **Fix, don't just flag.** Every finding comes with the specific change, including rewritten text.
5. **Keep it short.** Report the findings that change the user's experience. Ten real findings beat forty nitpicks. Don't pad with praise.

If you only have code, read the rendered strings, the order of elements, the handlers for back and close, and what happens while requests are pending and when they fail. If you can run it, run it.

## Quick tests

Each takes seconds and catches a whole class of problem.

| Test | How | Catches |
|---|---|---|
| **One sentence** | Finish "this screen is for ___" without using "and" | Screens doing two things |
| **Squint** | Blur your eyes. What stands out? | Wrong emphasis, competing buttons |
| **Count the primaries** | Count filled or accent buttons | More than one primary action |
| **Three seconds** | Read each question. Can it be answered at once? | Hard questions, unclear options |
| **Button alone** | Read only the button. Do you know what it does? | Vague labels |
| **Read aloud** | Say every string out loud | Jargon, stiffness, filler |
| **Why?** | For each input: why is this asked, and could the system know it? | Unnecessary and unexplained asks |
| **Back** | Press back at every step | Lost input, traps, wrong destination |
| **Cold start** | Arrive from a notification or link | Entry that doesn't match the promise |
| **Slow network** | Throttle, and tap things twice | Blank screens, double submits, layout jumps |
| **Break it** | Wrong input, no connection, empty account | Missing error and empty states |
| **Biggest text** | Maximum system font size | Truncation, overlap, unreachable buttons |
| **To their face** | Would you explain this element to the user directly, without embarrassment? | Dark patterns |

## Rubric

**Purpose and flow**
- Does each screen have one job, stated by its title?
- Is anything asked that could be known, inferred, defaulted or deferred?
- Does value appear before the first ask? Is every ask explained?
- Could a step, option or setting be removed entirely?
- Is the same thing called by the same name throughout?

**Action**
- Exactly one primary action per screen, in a consistent place?
- Do labels name the result?
- Do tappable things look tappable, with targets of at least 44pt / 48dp?
- Are destructive actions distinct and never the default?

**Continuity**
- Is each screen predicted by the previous one?
- Do earlier answers stay visible? Is anything asked twice?
- Does anything interrupt: pop-ups on entry, prompts before they're needed, upsells mid-task?
- Does back always return to the previous state with input intact?
- Do transitions follow one spatial model? Does the frame stay still?
- Does the flow end with a clear result and return the user to where they began?

**Waiting and states**
- Any blank screen, bare spinner or frozen moment?
- Feedback within 100ms of every tap? Double submission prevented?
- Are loading, empty, error and success designed for every screen with data?
- Does every error say what to do next and offer the control to do it?

**Words**
- Plain, spoken language? Any internal terms, codes or legalese?
- Any filler that could be cut?
- Active, positive, specific?
- Anything pressuring, blaming or shaming?
- Korean: consistent 해요체, light honorifics, "닫기" not "취소" for dismissal? (see `ux-writing-ko.md`)

**Respect**
- Anything from `dark-patterns.md`?
- Is declining as easy as accepting?
- Are ads labelled and outside the user's own data?

**Accessibility**
- Holds up at the largest text size?
- Contrast at 4.5:1 for text?
- Name, role and state on every control? Sensible reading order?
- Reduced motion honoured? Nothing conveyed by colour or animation alone?

**Visual hierarchy** (only where the project's own design system doesn't already decide it)
- Is the most important thing the most prominent?
- Is grouping done with space rather than boxes?
- Is the accent colour used for one purpose?

## Severity

| Level | Meaning | Examples |
|---|---|---|
| **Blocker** | Stops people finishing, misleads them, loses their data, or shuts some people out | No way to decline; input lost on back; an error with no way forward; unreadable at large text; a disguised ad |
| **Major** | Makes many people hesitate, mis-tap or abandon | Two competing primary buttons; a question nobody can answer quickly; a blank screen while loading; a vague primary label |
| **Minor** | Friction or polish | Filler text; inconsistent wording; a transition that goes the wrong direction |

Order findings by severity, then by where they occur in the flow.

## Report format

```markdown
## Summary
Two or three sentences: what the flow is for, the most important problem, and what already works.

## Findings

### 1. [Blocker] Short name of the problem
- **Where:** screen, element, and `file:line` if reviewing code
- **Principle:** the one it breaks (One Thing, Clear Action, Context Based, ...)
- **What the user experiences:** one or two sentences from their side
- **Fix:** the specific change

### 2. [Major] ...

## Rewritten copy
| Where | Before | After |
|---|---|---|

## Suggested flow
(only if the structure should change: the storyboard, one line per screen)

## Not checked
What you couldn't verify and why (no device, no backend, screenshots only).
```

- Quote existing strings exactly, so they can be found.
- Give the replacement text, not a description of it.
- If a finding depends on a guess about behaviour you couldn't see, say so.

## Applying fixes

When the code is available and the user wants changes made rather than listed:

- Fix blockers and majors first. Make structural changes before rewording.
- Keep the project's components, tokens and conventions. Don't restyle what wasn't a finding.
- Change copy in the place it actually lives (string files, a CMS key), not only where it is rendered.
- After the changes, run the quick tests again and report which now pass.
- If a fix requires a product decision (dropping a field, changing a policy), make the case and ask, instead of deciding silently.
