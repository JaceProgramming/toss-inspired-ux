# Motion recipes

Implementation notes for the transitions in `flow-continuity.md`. Jump to your stack. Prefer the platform's own navigation and sheet components: they already carry the right motion, gestures and accessibility behaviour.

**Contents:** Primitive lookup · Web: tokens and CSS · Web: step and shared-element transitions · React with Motion · React Native · SwiftUI · Jetpack Compose · Flutter

## Primitive lookup

| Need | Web | React Native | SwiftUI | Compose | Flutter |
|---|---|---|---|---|---|
| Push / back | View Transitions API, or router transitions | native stack navigator | `NavigationStack` | Navigation Compose | `Navigator` with `MaterialPageRoute` / `CupertinoPageRoute` |
| Side task | `<dialog>` styled as a sheet | stack screen with `presentation: 'modal'` or `'formSheet'` | `.sheet` + `.presentationDetents` | `ModalBottomSheet` | `showModalBottomSheet` |
| Shared element | same `view-transition-name` on both ends | Reanimated layout / shared transitions | `.navigationTransition(.zoom)` or `matchedGeometryEffect` | `SharedTransitionLayout` + `sharedElement` | `Hero` |
| In-place swap | View Transition on the changed region | Reanimated `entering` / `exiting` | `withAnimation` + `.transition` | `AnimatedContent` | `AnimatedSwitcher` |
| Neighbours make room | View Transition, or Motion `layout` | `layout={LinearTransition}` | implicit with `withAnimation` | `Modifier.animateItem()` | `AnimatedList`, `AnimatedSize` |
| Number change | count-up with `requestAnimationFrame` | Reanimated shared value | `.contentTransition(.numericText())` | `AnimatedContent` per digit | `TweenAnimationBuilder` |
| Reduced motion | `prefers-reduced-motion` | `useReducedMotion()` | `accessibilityReduceMotion` | system animator scale | `MediaQuery.disableAnimations` |
| Haptics | `navigator.vibrate` (Android only, use rarely) | `expo-haptics` | `.sensoryFeedback` | `LocalHapticFeedback` | `HapticFeedback` |

## Web: tokens and CSS

```css
:root {
  --dur-instant: 100ms;
  --dur-fast: 150ms;
  --dur-base: 250ms;
  --dur-screen: 350ms;
  --ease-out: cubic-bezier(0.22, 1, 0.36, 1);
  --ease-in: cubic-bezier(0.32, 0, 0.67, 0);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
  --shift: 24px;            /* how far sliding content travels */
  --press-scale: 0.97;
}

/* Reduced motion: movement becomes a short fade. Distances go to zero,
   so every recipe below degrades without a separate code path. */
@media (prefers-reduced-motion: reduce) {
  :root {
    --shift: 0px;
    --press-scale: 1;
    --dur-base: 120ms;
    --dur-screen: 120ms;
  }
}
```

**Press feedback**

```css
.pressable {
  transition: transform var(--dur-fast) var(--ease-out),
              background-color var(--dur-fast) linear;
  -webkit-tap-highlight-color: transparent;
  touch-action: manipulation;          /* no double-tap zoom delay */
}
.pressable:active {
  transform: scale(var(--press-scale));
  transition-duration: var(--dur-instant);
}
```

On iOS Safari, `:active` only applies during a touch if a touch listener exists. Register an empty one once: `document.addEventListener('touchstart', () => {}, { passive: true })`.

**Bottom sheet with the native `<dialog>`**

```html
<dialog class="sheet" aria-labelledby="sheet-title">
  <div class="sheet-body">
    <h2 id="sheet-title">Choose a delivery time</h2>
    <!-- options -->
    <button class="pressable" data-close>Close</button>
  </div>
</dialog>
```

```css
.sheet {
  margin: auto auto 0;                 /* dock to the bottom */
  width: min(100%, 480px);
  max-width: 100%;                     /* the UA default leaves side margins */
  max-height: 85dvh;
  border: 0;
  border-radius: 24px 24px 0 0;
  padding: 0;                          /* pad the inner wrapper, see below */
  overscroll-behavior: contain;
  /* closed state */
  opacity: 0;
  transform: translateY(var(--shift));
  transition: opacity var(--dur-fast) var(--ease-in),
              transform var(--dur-fast) var(--ease-in),
              overlay var(--dur-fast) allow-discrete,
              display var(--dur-fast) allow-discrete;
}
.sheet[open] {
  opacity: 1;
  transform: none;
  transition-duration: var(--dur-screen);
  transition-timing-function: var(--ease-out);
}
@starting-style {
  .sheet[open] { opacity: 0; transform: translateY(var(--shift)); }
}
.sheet-body { padding: 24px 20px max(20px, env(safe-area-inset-bottom)); }
.sheet::backdrop {
  background: rgb(0 0 0 / 0);
  transition: background-color var(--dur-base) linear,
              overlay var(--dur-base) allow-discrete,
              display var(--dur-base) allow-discrete;
}
.sheet[open]::backdrop { background: rgb(0 0 0 / 0.5); }
@starting-style {
  .sheet[open]::backdrop { background: rgb(0 0 0 / 0); }
}
```

```js
const sheet = document.querySelector('.sheet');
opener.addEventListener('click', () => sheet.showModal());       // traps focus, Esc closes
sheet.addEventListener('click', (e) => {                         // tap outside = never mind
  // With no padding on the dialog itself, a click whose target is the dialog
  // can only have landed on the backdrop.
  if (e.target === sheet || e.target.closest('[data-close]')) sheet.close();
});
// showModal() returns focus to the opener on close by itself.
```

`<dialog>` gives focus trapping, Escape, the top layer and focus return for free. A hand-rolled `<div>` sheet has to rebuild all four.

**Skeleton**

```css
.skeleton {
  border-radius: 8px;
  background: var(--color-fill);
  animation: pulse 1.2s var(--ease-in-out) infinite alternate;
}
@keyframes pulse { to { opacity: 0.5; } }
@media (prefers-reduced-motion: reduce) { .skeleton { animation: none; } }
```

**Count to a new number**

```js
function countTo(el, to, { duration = 400, format = (n) => n.toLocaleString() } = {}) {
  const from = Number(el.dataset.value ?? 0);
  el.dataset.value = to;
  if (matchMedia('(prefers-reduced-motion: reduce)').matches) { el.textContent = format(to); return; }
  const start = performance.now();
  const tick = (now) => {
    const t = Math.min(1, (now - start) / duration);
    const eased = 1 - Math.pow(1 - t, 4);                        // ease-out
    el.textContent = format(Math.round(from + (to - from) * eased));
    if (t < 1) requestAnimationFrame(tick);
  };
  requestAnimationFrame(tick);
}
```

Give the element `font-variant-numeric: tabular-nums` so its width doesn't wobble, and put the final value in an `aria-label` or a live region so screen readers hear one number, not every frame.

## Web: step and shared-element transitions

The View Transitions API animates between two DOM states. Wrap the state change and the browser cross-fades or moves named elements. Where it is unsupported the change still happens, just without animation.

```js
function navigate(update, direction = 'forward') {
  const reduce = matchMedia('(prefers-reduced-motion: reduce)').matches;
  if (!document.startViewTransition || reduce) { update(); return; }
  document.documentElement.dataset.nav = direction;
  const transition = document.startViewTransition(update);
  // If the browser skips the animation (rapid taps, a background tab), the
  // update still runs. Swallow the "skipped" rejection so it isn't reported.
  transition.ready.catch(() => {});
  transition.finished.finally(() => { delete document.documentElement.dataset.nav; });
}

// usage: only swap the changing region; push history so browser back works
nextButton.addEventListener('click', () => {
  history.pushState({ step: 2 }, '', '?step=2');
  navigate(() => renderStep(2), 'forward');
});
addEventListener('popstate', (e) => navigate(() => renderStep(e.state?.step ?? 1), 'back'));
```

```css
/* Name only the region that changes. The top bar and bottom button stay in
   the root snapshot, which doesn't move. */
.step { view-transition-name: step; }

::view-transition-old(step) { animation: var(--dur-fast) var(--ease-in) both step-out; }
::view-transition-new(step) { animation: var(--dur-screen) var(--ease-out) both step-in; }
html[data-nav='back']::view-transition-old(step) { animation-name: step-out-back; }
html[data-nav='back']::view-transition-new(step) { animation-name: step-in-back; }

@keyframes step-in       { from { opacity: 0; transform: translateX(var(--shift)); } }
@keyframes step-out      { to   { opacity: 0; transform: translateX(calc(var(--shift) * -0.5)); } }
@keyframes step-in-back  { from { opacity: 0; transform: translateX(calc(var(--shift) * -1)); } }
@keyframes step-out-back { to   { opacity: 0; transform: translateX(calc(var(--shift) * 0.5)); } }
```

**Shared element.** Give the tapped element and its counterpart in the next view the same name, only for the duration of the transition. Names must be unique among rendered elements at the moment of capture.

```js
card.style.viewTransitionName = 'hero';
const t = document.startViewTransition(() => {
  card.style.viewTransitionName = '';
  showDetail(item);                                  // detail header has view-transition-name: hero
});
t.ready.catch(() => {});
t.finished.finally(() => { detailHeader.style.viewTransitionName = ''; });
```

**Separate pages (multi-page site).** Opt in on both pages with `@view-transition { navigation: auto; }` and the same `view-transition-name` rules apply across the navigation.

Framework routers increasingly wrap this API. If the project's router has a view-transition option, use it instead of calling the API by hand.

For all of the above assembled into one working page (steps, history, transitions, a busy button, a toast, reduced motion), see `assets/flow-shell.html`.

## React with Motion

Motion (`motion/react`, formerly `framer-motion`) fits React state changes.

```jsx
import { AnimatePresence, MotionConfig, motion } from 'motion/react';

const step = {
  enter:  (dir) => ({ x: 24 * dir, opacity: 0 }),
  center: { x: 0, opacity: 1, transition: { duration: 0.35, ease: [0.22, 1, 0.36, 1] } },
  exit:   (dir) => ({ x: -12 * dir, opacity: 0, transition: { duration: 0.15, ease: [0.32, 0, 0.67, 0] } }),
};

export function Flow({ current, dir, children }) {
  return (
    <MotionConfig reducedMotion="user">          {/* drops transforms, keeps fades */}
      <AnimatePresence mode="wait" initial={false} custom={dir}>
        <motion.section key={current} custom={dir} variants={step}
          initial="enter" animate="center" exit="exit">
          {children}
        </motion.section>
      </AnimatePresence>
    </MotionConfig>
  );
}
```

- `dir` is `1` going forward and `-1` going back.
- Keep the top bar and the bottom button outside `AnimatePresence` so they don't re-mount.
- **Shared element:** put the same `layoutId` on the list item and on the detail header (`<motion.div layoutId={`card-${id}`} />`).
- **Stacked form:** give each row `layout` so it slides when a new row mounts, and wrap the new row in `AnimatePresence` to fade it in.
- **Press:** `<motion.button whileTap={{ scale: 0.97 }} transition={{ duration: 0.1 }} />`.

## React Native

```tsx
// Navigation: the native stack gives each platform its own push, pop and swipe-back.
<Stack.Navigator>
  <Stack.Screen name="Amount" component={Amount} />
  <Stack.Screen name="Options" component={Options} options={{ presentation: 'modal' }} />
</Stack.Navigator>
```

```tsx
import Animated, {
  FadeInDown, LinearTransition, useAnimatedStyle, useReducedMotion,
  useSharedValue, withSpring, withTiming,
} from 'react-native-reanimated';

// Stacked form: the new field fades in, older rows slide to make room.
<Animated.View entering={FadeInDown.duration(250)} layout={LinearTransition.duration(250)}>
  {/* field */}
</Animated.View>

// Press feedback
function PressableScale({ children, onPress }) {
  const scale = useSharedValue(1);
  const reduce = useReducedMotion();
  const style = useAnimatedStyle(() => ({ transform: [{ scale: scale.value }] }));
  return (
    <Pressable
      onPress={onPress}
      onPressIn={() => { if (!reduce) scale.value = withTiming(0.97, { duration: 100 }); }}
      onPressOut={() => { scale.value = withSpring(1, { stiffness: 400, damping: 40 }); }}>
      <Animated.View style={style}>{children}</Animated.View>
    </Pressable>
  );
}
```

```tsx
import * as Haptics from 'expo-haptics';
Haptics.selectionAsync();                                                  // a choice snapped
Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success);       // something important finished
```

Keep the bottom button above the keyboard with `KeyboardAvoidingView` (`behavior="padding"` on iOS) and pad it with the bottom safe-area inset.

## SwiftUI

```swift
// Push and swipe-back come from NavigationStack. Keep them.
NavigationStack(path: $path) { /* ... */ }

// Side task
.sheet(isPresented: $showOptions) {
    OptionsView().presentationDetents([.medium, .large])
}

// Item grows into its detail (iOS 18+). Earlier: matchedGeometryEffect in one view tree.
@Namespace private var ns
NavigationLink(value: item) {
    CardView(item).matchedTransitionSource(id: item.id, in: ns)
}
.navigationDestination(for: Item.self) { item in
    DetailView(item).navigationTransition(.zoom(sourceID: item.id, in: ns))
}

// In-place change
withAnimation(.snappy) { step = .birthday }       // iOS 17+. Before: .spring(response: 0.35, dampingFraction: 0.9)

// Number change
Text(amount, format: .number)
    .monospacedDigit()
    .contentTransition(.numericText())

// Pinned bottom button that rides above the keyboard
.safeAreaInset(edge: .bottom) { PrimaryButton("Next", action: next).padding() }

// Feedback and reduced motion
.sensoryFeedback(.success, trigger: isDone)       // iOS 17+
@Environment(\.accessibilityReduceMotion) private var reduceMotion
```

## Jetpack Compose

```kotlin
// Step content: only the changing region animates. `step` is an Int.
AnimatedContent(
    targetState = step,
    transitionSpec = {
        val dir = if (targetState > initialState) 1 else -1
        (slideInHorizontally(tween(350)) { it / 8 * dir } + fadeIn(tween(350))) togetherWith
            (slideOutHorizontally(tween(150)) { -it / 16 * dir } + fadeOut(tween(150)))
    },
    label = "step",
) { current -> StepContent(current) }

// Side task
if (showOptions) {
    ModalBottomSheet(onDismissRequest = { showOptions = false }) { Options() }
}

// Rows make room for a new one
LazyColumn { items(fields, key = { it.id }) { FieldRow(it, Modifier.animateItem()) } }

// Pinned bottom button above the keyboard and the navigation bar
Scaffold(bottomBar = {
    PrimaryButton("Next", onClick = next, modifier = Modifier.navigationBarsPadding().imePadding())
}) { padding -> Content(Modifier.padding(padding)) }

// Haptic
val haptics = LocalHapticFeedback.current
haptics.performHapticFeedback(HapticFeedbackType.LongPress)
```

- Shared elements: wrap both destinations in `SharedTransitionLayout` and tag each end with `Modifier.sharedElement(...)` or `sharedBounds(...)` using the same key (Compose 1.7+).
- Compose follows the system animator duration scale, which is how "remove animations" reaches your app. Don't override it.
- Use the default spring for most things: `spring(dampingRatio = Spring.DampingRatioNoBouncy, stiffness = Spring.StiffnessMedium)`.

## Flutter

```dart
// Item grows into its detail: same tag on both routes.
Hero(tag: 'card-${item.id}', child: CardView(item))

// Side task
showModalBottomSheet(
  context: context,
  isScrollControlled: true,
  useSafeArea: true,
  builder: (_) => const OptionsSheet(),
);

// In-place step change
AnimatedSwitcher(
  duration: const Duration(milliseconds: 250),
  switchInCurve: Curves.easeOutCubic,
  switchOutCurve: Curves.easeInCubic,
  child: KeyedSubtree(key: ValueKey(step), child: buildStep(step)),
)

// Respect the system setting, and give light feedback
final reduceMotion = MediaQuery.of(context).disableAnimations;
HapticFeedback.selectionClick();
```

Use `CupertinoPageRoute` on iOS and `MaterialPageRoute` on Android (or `PageTransitionsTheme`) so pushes and the back gesture match the platform. A `Scaffold` already lifts its body above the keyboard. Put the primary button in a `SafeArea` at the bottom of the body.
