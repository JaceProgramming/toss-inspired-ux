# Visual defaults

Use this file only when the project has no design system, tokens or component library. If it has one, use that and skip this file.

These values were written for this guide as a neutral starting point. They are not Toss's tokens. The accent colour is a placeholder for the product's own brand colour.

**Contents:** What the look is made of · Tokens · Colour rules · Type rules · Spacing, shape and size · The pinned bottom button · Graphics · Dark mode · Korean text

## What the look is made of

- **Space instead of lines.** Groups are separated by whitespace or a quiet band of background, not by borders and boxes.
- **One accent.** A single colour marks the primary action and the thing to notice. Everything else is neutral.
- **Big, confident type for the one thing.** Quiet greys for everything supporting it.
- **Soft, flat shapes.** Generous corner radius, almost no shadows.
- **Content over chrome.** No decorative headers, gradients or dividers that carry no information.

## Tokens

```css
:root {
  color-scheme: light dark;

  /* neutrals */
  --color-bg: #FFFFFF;
  --color-bg-subtle: #F4F5F7;       /* page behind grouped content, section bands */
  --color-surface: #FFFFFF;         /* sheets, dialogs, cards */
  --color-fill: #EEF0F3;            /* inputs, secondary buttons, skeletons */
  --color-fill-pressed: #E2E5E9;
  --color-border: #E3E6EA;          /* hairlines, rarely needed */
  --color-control-border: #8B94A1;  /* outlines that must be seen (3:1) */

  /* text */
  --color-text: #17191C;
  --color-text-secondary: #4B5360;
  --color-text-tertiary: #5F6876;
  --color-text-disabled: #A9B0BA;

  /* accent: replace with the brand colour, then re-check contrast */
  --color-accent: #2563EB;
  --color-accent-pressed: #1D4FD0;
  --color-accent-subtle: #EAF1FF;
  --color-accent-text: #2563EB;     /* links and accent-coloured text */
  --color-on-accent: #FFFFFF;

  /* meaning */
  --color-danger: #CC2431;
  --color-danger-subtle: #FDECEE;
  --color-success: #0E7A52;
  --color-warning: #A85A00;
  --color-scrim: rgb(0 0 0 / 0.5);

  /* type: rem so the reader's text-size setting applies */
  --font-sans: "Pretendard Variable", Pretendard, -apple-system, BlinkMacSystemFont,
               "Apple SD Gothic Neo", "Noto Sans KR", "Malgun Gothic", "Segoe UI",
               Roboto, system-ui, sans-serif;
  --text-display: 700 1.875rem/1.3 var(--font-sans);   /* 30px  hero numbers */
  --text-title:   700 1.5rem/1.35 var(--font-sans);    /* 24px  screen title */
  --text-heading: 600 1.25rem/1.4 var(--font-sans);    /* 20px  sheet title, section */
  --text-body:    400 1.0625rem/1.5 var(--font-sans);  /* 17px  default */
  --text-sub:     400 0.9375rem/1.5 var(--font-sans);  /* 15px  secondary */
  --text-caption: 400 0.8125rem/1.4 var(--font-sans);  /* 13px  smallest allowed */

  /* space */
  --space-1: 4px;  --space-2: 8px;  --space-3: 12px; --space-4: 16px;
  --space-5: 20px; --space-6: 24px; --space-8: 32px; --space-10: 40px;
  --gutter: 20px;                   /* screen side padding */

  /* shape */
  --radius-sm: 8px;                 /* chips, small controls */
  --radius-md: 12px;                /* inputs, list cards */
  --radius-lg: 16px;                /* buttons, cards */
  --radius-xl: 24px;                /* sheet top corners, dialogs */
  --radius-full: 999px;

  /* size */
  --size-tap: 48px;                 /* smallest tap target */
  --size-button: 56px;              /* primary button and input height */
  --size-row: 56px;                 /* single-line list row (two-line: 72px) */

  /* elevation: only floating layers get a shadow */
  --shadow-float: 0 4px 16px rgb(0 0 0 / 0.12);
  --shadow-modal: 0 8px 32px rgb(0 0 0 / 0.16);
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-bg: #111215;
    --color-bg-subtle: #0B0C0E;
    --color-surface: #1B1C21;       /* raised layers are lighter, not shadowed */
    --color-fill: #25272D;
    --color-fill-pressed: #30333A;
    --color-border: #2B2E35;
    --color-control-border: #6F7783;

    --color-text: #F3F4F6;
    --color-text-secondary: #B7BDC7;
    --color-text-tertiary: #8E96A3;
    --color-text-disabled: #5A616C;

    --color-accent: #2F6BEB;
    --color-accent-pressed: #2559CC;
    --color-accent-subtle: #17233D;
    --color-accent-text: #7EA8FF;   /* lighter tint so accent text stays readable */
    --color-on-accent: #FFFFFF;

    --color-danger: #FF6B76;
    --color-danger-subtle: #3A1A1E;
    --color-success: #3FCB94;
    --color-warning: #F2A93B;
    --color-scrim: rgb(0 0 0 / 0.6);
  }
}
```

Every text colour above reaches 4.5:1 on the backgrounds it is meant for, in both modes. Disabled text is exempt by definition. Use the same names on other platforms (a `theme` object, asset catalogue colours, a Compose `ColorScheme`).

## Colour rules

- **Three levels of text**, no more: primary for what must be read, secondary for support, tertiary for what can be skipped. Nothing essential is ever tertiary.
- **Accent is scarce.** Primary button, selected state, links, one highlight. If a tenth of the screen is accent-coloured, that is already a lot.
- **Meaning colours mean something.** Danger for errors and destructive actions only. Success sparingly, usually just the check mark. No green "positive" buttons competing with the accent.
- **Never colour alone.** Add an icon, a sign or a word. This matters doubly for money: Korean markets show gains in red and losses in blue, the reverse of many others.
- **After replacing the accent, check it.** White text on the accent needs 4.5:1. If the brand colour is too light for that, either darken the button fill or set `--color-on-accent` to the dark text colour.

## Type rules

| Style | Size / line | Weight | Use |
|---|---|---|---|
| display | 30 / 1.3 | 700 | The hero number or a result headline |
| title | 24 / 1.35 | 700 | The screen title |
| heading | 20 / 1.4 | 600 | Sheet and dialog titles, section headers |
| body | 17 / 1.5 | 400, or 600 for emphasis | Default text, inputs, buttons (600) |
| sub | 15 / 1.5 | 400 | Second lines, helper text |
| caption | 13 / 1.4 | 400 | Labels above values, fine print |

- Six styles are enough. If you want a seventh, change weight or colour instead.
- Nothing smaller than 13px.
- Amounts and anything that updates use tabular figures (`font-variant-numeric: tabular-nums`).
- No all-caps, no italics for Korean, underline only for links inside running text.
- Keep lines to roughly 45 to 70 Latin characters or 25 to 40 Korean characters.

## Spacing, shape and size

- Everything sits on a 4px grid. Most gaps are 8, 12, 16, 20 or 24.
- Screen side padding is 20px. Title to content: 24 to 32px. Between unrelated sections: 32 to 40px, or a 12 to 16px band of `--color-bg-subtle`.
- Related things are closer together than unrelated things, always. This does most of the work that borders would otherwise do.
- Buttons and inputs are 56px tall. Compact buttons are 48px. Nothing tappable is smaller than 48px in either direction, even when the visible glyph is 24px.
- List rows are at least 56px, or 72px with a second line.
- Corners: 16px on buttons and cards, 12px on inputs, 24px on sheets and dialogs.
- Cards don't get shadows. They sit on `--color-bg-subtle` instead. Only layers that float over content (toast, sheet, dialog) cast one.

## The pinned bottom button

```css
.screen { min-height: 100dvh; display: flex; flex-direction: column; background: var(--color-bg); }
.screen > main { flex: 1; padding: 0 var(--gutter); }

.cta-bar {
  position: sticky; bottom: 0;
  padding: 12px var(--gutter) max(16px, env(safe-area-inset-bottom));
  background: linear-gradient(to top, var(--color-bg) 75%, transparent);
}
.cta {
  width: 100%; min-height: var(--size-button);
  border: 0; border-radius: var(--radius-lg);
  background: var(--color-accent); color: var(--color-on-accent);
  font: var(--text-body); font-weight: 600;
}
.cta:disabled { background: var(--color-fill); color: var(--color-text-disabled); }
```

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover, interactive-widget=resizes-content">
```

- Use `dvh`, not `vh`, so mobile browser bars don't hide the button.
- `viewport-fit=cover` plus `env(safe-area-inset-bottom)` keeps it clear of the home indicator.
- `interactive-widget=resizes-content` makes Chrome on Android shrink the layout when the keyboard opens, so the button rides above it. iOS Safari doesn't resize the layout. There, listen to `window.visualViewport` and move the bar up by the height the keyboard covers. Test this on a real device.
- Native: SwiftUI `.safeAreaInset(edge: .bottom)`, Compose `Scaffold(bottomBar)` with `imePadding()`, React Native `KeyboardAvoidingView` plus the bottom inset, Flutter a `SafeArea` at the foot of the `Scaffold` body.
- Content scrolls under the bar. Leave bottom padding in the scroll area equal to the bar's height so the last item is reachable.

## Graphics

- One focal graphic per screen at most, and only when it carries meaning: the outcome, the object in question, the state.
- Size follows information. A simple symbol stays small. Only a picture with detail worth reading gets large.
- A graphic never covers key information or pushes the primary action off screen.
- It tells the truth. No error symbol when nothing went wrong. No loading animation when nothing is loading. No sad or pleading characters to steer a choice.
- No decorative particles, glows or heavy gradients.
- Mid-tones, so it reads on both light and dark backgrounds.
- Icons are 24px in rows and bars, up to 40px as a row's leading visual. One icon per spot, not several side by side.
- Use one icon family throughout, from the project or a permissively licensed set. Don't use Toss's icons, emoji or illustrations.

## Dark mode

- It is not an inversion. Backgrounds go near-black, and each raised layer gets lighter, because shadows are invisible on dark.
- Contrast needs to be a little stronger than in light mode. The dim surroundings make the same ratio feel weaker.
- Accent fills can stay. Accent text needs a lighter tint.
- Check photos and illustrations with white backgrounds. Give them a neutral container or a dark variant.
- Check both modes before finishing. A design verified in one mode only is half verified.

## Korean text

```css
body {
  font: var(--text-body);
  color: var(--color-text);
  word-break: keep-all;        /* break between words, not inside them */
  overflow-wrap: anywhere;     /* but never overflow on a long string */
  -webkit-font-smoothing: antialiased;
}
h1, h2 { text-wrap: balance; } /* even line lengths in titles */
```

- Korean needs more line height than Latin text: 1.5 for body, 1.3 to 1.4 for titles.
- `keep-all` stops a word being split across lines. At very large text sizes, splitting is better than overflowing, which is what `overflow-wrap` allows.
- Leave letter-spacing at zero for body text. Large titles can take a slightly tighter setting.
- A title that wraps so that one syllable sits alone on the last line needs rewording or `text-wrap: balance`.
