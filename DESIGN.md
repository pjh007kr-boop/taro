# TARO Design Guide

> Updated from the attached “명지대학교 타로 디자인 팩.fig” on 2026-11-19. The pack was inspected through its `thumbnail.png`, `meta.json`, and 22 extracted `images/*` assets. Its `canvas.fig` is a compressed Figma binary and its editable layer/style metadata could not be decoded in this environment. Values below distinguish sampled colors from visual observations; exact typography, spacing, component names, and component variants still need confirmation from the source file.

## 1. Design direction

TARO follows the attached university tarot pack: a clean white and pale-gray canvas, near-black typography, vivid green actions, and the traditional illustrated tarot deck as the main visual. Keep the experience mobile-first, with one clear action at a time, generous whitespace, and concise Korean copy. Apply the pack’s bright, straightforward interface to the existing privacy-first tarot flow. The reference thumbnail shows form, result, sharing, and card collection patterns; the exact source component properties could not be read from the packed canvas binary.

- **Theme:** light only for MVP
- **Personality:** direct, bright, approachable, trustworthy
- **Primary visual:** one illustrated Major Arcana card, centered and given room to breathe
- **Avoid:** occult fear cues, prediction certainty, dense dashboards, gradients, glow, glitter, heavy shadows, and decoration that competes with the card
- **Privacy is visible:** place the short privacy promise beside the form and on the result screen; never make it a hidden legal footnote
- **Entertainment notice:** show “오락용 타로 해석입니다” on the result card, saved image, and share text

## 2. Color tokens

Use semantic tokens in components. The pack’s interface palette is white, pale gray, ink, and vivid green. Keep saturated green for key actions and selected states so it remains distinctive.

| Role | Hex | CSS token | Use |
|---|---|---|---|
| White | `#FFFFFF` | `--color-white` | Main canvas and card face; sampled from pack thumbnail |
| Pale gray | `#F3F3F5` | `--color-canvas` | Main screen background; sampled from pack thumbnail |
| Soft gray | `#F9F9FA` | `--color-surface-muted` | Quiet inset surfaces; sampled from pack thumbnail |
| Border gray | `#D2D2D3` | `--color-border` | Control borders and dividers; sampled from pack thumbnail |
| Mid gray | `#BFBFBF` | `--color-disabled` | Disabled controls and low emphasis; sampled from pack thumbnail |
| Ink | `#232427` | `--color-ink` | Main text; sampled from pack thumbnail |
| Primary green | `#00D959` | `--color-primary` | Main action and selected state; sampled from pack thumbnail |
| Pale green | `#E6FAEF` | `--color-green-soft` | Selected/quiet green surface; sampled from pack thumbnail |
| Mint | `#80ECAC` | `--color-green-mid` | Secondary green accent; sampled from pack thumbnail |
| Light mint | `#F2FCF7` | `--color-green-tint` | Very subtle green surface; sampled from pack thumbnail |
| Error red | `#E5484D` | `--color-error` | Form errors only; provisional semantic value, not confirmed as a pack token |

### Color rules

- Use `--color-ink` for text and primary button labels on `--color-primary`; black text keeps the bright green action readable.
- Use `--color-green-soft` as a selected surface with an additional border or icon cue. Keep the card illustration’s own palette intact.
- The sample shows gray disabled controls; make disabled state legible through text and shape as well as color.
- Disabled controls use gray surfaces and muted text, with a visible disabled state; do not rely on color alone.
- Error text must name the correction, e.g. “날짜를 다시 확인해 주세요”.
- Keep contrast at WCAG AA for normal text (4.5:1) and large text (3:1); verify final combinations when implementing.

## 3. Typography

The source thumbnail shows a clean Korean sans-serif with heavier section headings and regular supporting text. The exact family and type ramp are not legible in the raster preview and are not exposed by the compressed canvas file. Use the existing Korean-capable system stack below until the source Figma typography styles can be inspected; do not claim a specific pack font or exact sizes.

```css
font-family: -apple-system, BlinkMacSystemFont, "Apple SD Gothic Neo", "Noto Sans KR", "Malgun Gothic", sans-serif;
```

| Role | Size / line height | Weight | Tracking | Use |
|---|---:|---:|---:|---|
| Display | `36px / 1.12` mobile, `48px / 1.08` wide | 600 | `-0.035em` | Existing TARO fallback; confirm pack style |
| Section title | `24px / 1.25` | 600 | `-0.02em` | Existing TARO fallback; confirm pack style |
| Card title | `20px / 1.3` | 600 | `-0.01em` | Existing TARO fallback; confirm pack style |
| Body | `16px / 1.55` | 400 | `-0.01em` | Existing TARO fallback; confirm pack style |
| Control | `15px / 1.35` | 500 | `-0.01em` | Existing TARO fallback; confirm pack style |
| Label | `14px / 1.4` | 500 | `0` | Existing TARO fallback; confirm pack style |
| Caption | `12px / 1.45` | 400 | `0` | Existing TARO fallback; confirm pack style |

- Do not set Korean body text below `12px`.
- Keep interpretation text easy to scan; use separate lines rather than a compressed paragraph.
- Names and card names may wrap; do not truncate user-provided names without notice.

## 4. Spacing, layout, and shape

**Spacing source status:** the thumbnail suggests compact, consistent spacing and generous separation between screens and major sections, but exact numeric spacing tokens cannot be read from the packed source. Keep the existing 4px-based scale as a TARO implementation fallback until confirmed against editable source styles.

**Base unit:** 4px. Use a small consistent scale and `gap-*` for layout.

| Token | Value | Typical use |
|---|---:|---|
| `--space-1` | 4px | Icon-to-label micro gap |
| `--space-2` | 8px | Label and helper text |
| `--space-3` | 12px | Compact control contents |
| `--space-4` | 16px | Default field and button gap |
| `--space-5` | 20px | Topic group spacing |
| `--space-6` | 24px | Card content groups |
| `--space-8` | 32px | Screen section gap |
| `--space-10` | 40px | Result card breathing room |
| `--space-12` | 48px | Large section spacing |
| `--space-16` | 64px | Wide-screen section spacing |

- Mobile page gutter: `20px` (minimum `16px` on narrow screens); maximum content width: `480px` for the reading flow.
- Keep the main action visible near the bottom of the input flow; account for safe-area insets.
- Input and topic controls should be at least `48px` tall. Tap targets must be at least `44 × 44px`.
- Use `16px` minimum input text to prevent mobile browser zoom.
- **Radii:** input `12px`, standard button `9999px`, topic option `16px`, reading card `24px`, image frame `24px`, notice `14px`.
- Surfaces are flat. Use a 1px line to separate surfaces; avoid ambient shadows. A restrained shadow is acceptable only if it clarifies a floating overlay.

## 5. Tailwind v4 and shadcn conventions

Use Tailwind v4 CSS-first theme tokens and shadcn semantic variables. Components should consume semantic utilities (`bg-background`, `text-foreground`, `bg-primary`, `text-muted-foreground`, `border-input`) instead of hardcoded palette utilities. If the project uses Tailwind v3, map the same token names through its config rather than changing the visual roles.

```css
@import "tailwindcss";

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--card-foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
  --color-secondary: var(--secondary);
  --color-secondary-foreground: var(--secondary-foreground);
  --color-muted: var(--muted);
  --color-muted-foreground: var(--muted-foreground);
  --color-accent: var(--accent);
  --color-accent-foreground: var(--accent-foreground);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);
  --radius-card: 1.5rem;
}

:root {
  color-scheme: light;
  --background: #f3f3f5;
  --foreground: #232427;
  --card: #ffffff;
  --card-foreground: #232427;
  --primary: #00d959;
  --primary-foreground: #232427;
  --secondary: #e6faef;
  --secondary-foreground: #232427;
  --muted: #f3f3f5;
  --muted-foreground: #68686d;
  --accent: #e6faef;
  --accent-foreground: #232427;
  --border: #d2d2d3;
  --input: #d2d2d3;
  --ring: #00d959;
  --radius: 0.75rem;
}
```

### shadcn implementation rules

- Compose existing primitives; do not build a parallel design system from styled `div`s.
- Use semantic `Button` variants and the standard `Card` composition (`CardHeader`, `CardTitle`, `CardDescription`, `CardContent`, `CardFooter`).
- Use `FieldGroup` / `Field` for forms, with `FieldLabel`, `Input`, and `FieldDescription`.
- Use `ToggleGroup` + `ToggleGroupItem` for the six mutually exclusive topics; set an accessible label and selected state.
- Use `Alert` for the privacy note and a toast (`sonner`) for save/copy feedback.
- Use `Separator` for quiet dividers; avoid hand-built border-only separators.
- Use `cn()` for conditional class composition. Prefer `gap-*`; avoid `space-x-*` / `space-y-*`. Use `size-*` for equal width and height. Use semantic component variants before adding custom styles.
- Keep labels and descriptions connected to controls. Put `aria-invalid` on invalid controls and expose errors in text.
- Do not add dark mode in the MVP; the experience and saved image are designed for light surfaces.

## 6. Component rules

### Buttons

| Variant | Appearance | Use |
|---|---|---|
| Primary | Violet fill, white text, pill radius, 48px minimum height | 타로 시작, 카드 한 장 뽑기 |
| Secondary | White fill, ink text, 1px gray outline | 이미지 저장, 결과 공유 |
| Tertiary | Transparent, ink/muted text, no heavy outline | 다시 뽑기, 보조 동작 |
| Destructive | Error text and restrained error outline | Only for an explicit destructive confirmation; not used in normal tarot flow |

- Primary action is full width on narrow screens. Use 16px semibold text and a visible pressed state.
- Keep “처음으로” distinct from “다시 뽑기”; it clears in-memory values and returns to start.
- Disabled draw action remains legible and visibly disabled until name, valid date, and topic are present.
- Provide a keyboard focus ring using the ring token. Never remove focus indication.

### Form fields

- Order: name (up to 10 characters), birth date, optional major, then topic selection.
- Use visible labels above fields. Use native date controls where reliable; validate real calendar dates and show the inline error next to the date field.
- Placeholder examples must not substitute for labels. Use `autocomplete="off"` where suitable and never write values to browser storage.
- Topic options: six labeled icon choices (커리어, 재물, 연애, 건강, 학업, 종합). Use an obvious selected border and pale green fill; expose selection to assistive technology.
- Keep the privacy alert immediately below the fields, before the main draw action: “입력한 정보는 해석을 만드는 데만 쓰고 저장하지 않아요. 창을 닫거나 [처음으로]를 누르면 바로 지워져요.”

### Tarot card / result card

- The S-03 reading card is the visual center. Use one white card with a 24px radius and a hairline border; keep its contents centered and spacious.
- Present, in order: `{이름}님의 타로 · {주제}`, card illustration, card number/name and zodiac, then the interpretation lines.
- Illustration must be original or clearly licensed. Keep a stable portrait aspect ratio and do not use an unlicensed Rider–Waite image.
- Interpretation is at most 3 lines and about 90 Korean characters total. The base line is always present; zodiac is based on date; major line appears only when a supported major category is recognized.
- Keep entertainment notice inside the card. Place the privacy disposal reminder below the actions on the result screen.
- The result image captures only the card area and is exported as PNG at `1080 × 1350`; include name, topic, card artwork/name, zodiac, interpretation, entertainment notice, and service name. Never include birth date or raw major text.
- On the result view, offer two secondary actions (save/share) and two tertiary actions (draw again/start over). Keep these controls visually subordinate to the reading.

### Alerts, status, and share fallback

- Use an informational `Alert` for the privacy promise. Keep it calm and plain; no alarm-red lock imagery.
- Save success: “이미지를 저장했어요”. If download is unsupported, show the image and “이미지를 길게 눌러 저장해 주세요”.
- Share uses the device share menu with image + text where supported. On unsupported devices, copy text and announce “결과를 복사했어요. 원하는 앱에 붙여 넣어 주세요”. If copy fails, expose selectable text and explain how to copy it.
- Status updates must be announced politely to assistive technology; do not move focus unexpectedly.

## 7. Motion and interaction

- Card reveal uses one quick flip, completing within 1 second. Keep reduced-motion users on a simple fade or immediate reveal (`prefers-reduced-motion`).
- Respect reduced-motion settings for all transitions. Avoid parallax, looping animation, and animated decorative particles.
- Button feedback is immediate; maintain the entered data when returning from the result with “다시 뽑기”.
- “처음으로” clears name, birth date, major, topic, and result from application memory before showing S-01. A page refresh and tab close should leave no restored values because persistence is not used.

## 8. Accessibility and responsive behavior

- Design mobile-first for a single-column flow; at wider widths, keep the same narrow reading column instead of stretching the form or card.
- Support keyboard operation, visible focus, logical focus order, screen-reader labels, and status announcements.
- Icons are supplementary; every topic and action has a text label.
- Do not use color alone for selected, disabled, error, or success states.
- Respect safe areas and browser zoom. Avoid horizontal scrolling at 320px width.
- Ensure the card illustration has useful alt text when meaningful; hide purely decorative stars and ornaments from assistive technology.

## 9. Saved image and share content

Use the same visual hierarchy as the on-screen reading card. Saved and shared content must retain the entertainment notice and omit date of birth. Share links always point to S-01 and contain no user name or result.

```text
🔮 {이름}님의 타로 · {주제}
{카드 이름} · {별자리}
{해석 3줄}
※ 오락용 타로 해석입니다
나도 한 장 뽑기 → {시작 링크}
```

## 10. Do and avoid

### Do

- Keep one dominant action per screen and short, direct Korean copy.
- Give the card artwork generous whitespace and a repeatable share-image layout.
- Make the privacy promise visible before input and again on the result screen.
- Use semantic tokens, shadcn primitives, and consistent `gap-*` spacing.
- Keep health and money interpretations reflective and non-deterministic.

### Avoid

- Do not send personal data to a server, AI service, analytics provider, URL, or browser storage.
- Do not put birth date or raw major text in saved images or shared messages.
- Do not imply a certain future or offer medical/financial advice.
- Do not overuse saturated green, gradients, glow, shadows, or decorative symbols.
- Do not ship copyrighted card art without clear rights.

---

