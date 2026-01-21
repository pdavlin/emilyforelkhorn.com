# PRP: Utopia Fluid Typography Implementation

## Overview

**Feature**: Replace static typography tokens with fluid clamp() values using Utopia type scale
**Complexity**: 3/10 (CSS-only changes, no dependencies, no build changes)
**Files to Modify**: 10 (1 global.css + 9 components)
**Source Spec**: docs/specs/utopia-fluid-typography-spec.md

## Context

### Tech Stack
- Astro 6.0.0-beta.1
- Tailwind CSS 4.1.18 (via @tailwindcss/vite)
- Package manager: pnpm
- No testing framework configured

### Current State
The site already uses Utopia-generated fluid spacing (`global.css:99-160`), but typography uses static rem values that require breakpoint classes (e.g., `text-4xl md:text-5xl`) for responsive sizing.

### Pattern to Follow
Existing fluid spacing implementation in `global.css:99-160`:
```css
--space-s: clamp(1rem, 0.8977rem + 0.4545vw, 1.25rem);
```

Apply the same pattern to typography tokens.

## Documentation References

- Utopia Type Calculator: https://utopia.fyi/type/calculator?c=360,16,1.2,1240,20,1.25,5,2
- Tailwind CSS 4 Theme: https://tailwindcss.com/docs/v4-beta
- CSS clamp(): https://developer.mozilla.org/en-US/docs/Web/CSS/clamp

## Implementation Blueprint

### Phase 1: Update Global Typography Tokens

**File**: `src/styles/global.css`

**Lines 44-59**: Replace static `--text-*` values with fluid clamp() values:

```css
/* ================================
 * Typography - Fluid Font Sizes
 * @link https://utopia.fyi/type/calculator?c=360,16,1.2,1240,20,1.25,5,2
 * ================================ */

/* Step -2: 11.11px -> 12.80px */
--text-xs: clamp(0.6944rem, 0.6596rem + 0.1548vw, 0.8rem);

/* Step -1: 13.33px -> 16.00px */
--text-sm: clamp(0.8331rem, 0.7759rem + 0.2543vw, 1rem);

/* Step 0: 16px -> 20px */
--text-base: clamp(1rem, 0.9091rem + 0.4045vw, 1.25rem);

/* Step 1: 19.20px -> 25.00px */
--text-lg: clamp(1.2rem, 1.0682rem + 0.5859vw, 1.5625rem);

/* Step 2: 23.04px -> 31.25px */
--text-xl: clamp(1.44rem, 1.2531rem + 0.8316vw, 1.9531rem);

/* Step 3: 27.65px -> 39.06px */
--text-2xl: clamp(1.7281rem, 1.4696rem + 1.1502vw, 2.4413rem);

/* Step 4: 33.18px -> 48.83px */
--text-3xl: clamp(2.0738rem, 1.7232rem + 1.5586vw, 3.0519rem);

/* Step 5: 39.81px -> 61.04px */
--text-4xl: clamp(2.4881rem, 2.0186rem + 2.0886vw, 3.815rem);

/* Step 6: 47.78px -> 76.29px */
--text-5xl: clamp(2.9863rem, 2.3627rem + 2.7727vw, 4.7681rem);

/* Step 7: 57.33px -> 95.37px */
--text-6xl: clamp(3.5831rem, 2.7613rem + 3.6545vw, 5.9606rem);

/* Step 8: 68.80px -> 119.21px */
--text-7xl: clamp(4.3rem, 3.2211rem + 4.7955vw, 7.4506rem);
```

**Additional changes in global.css**:
- Line 259: Replace `font-size: 0.9rem` with `font-size: var(--text-sm)`
- Line 322: Replace `font-size: 0.95rem` with `font-size: var(--text-sm)`
- Line 349: Replace `font-size: 1rem` with `font-size: var(--text-base)`
- Delete `--text-8xl` (line 59) - consolidate to 7xl as max

**Validation**:
```bash
pnpm build
# Manual: Check dev server at 360px, 768px, 1024px, 1240px
```

---

### Phase 2: Update Hero Component

**File**: `src/components/Hero.astro`

**Line 29**: Replace inline clamp with token
```html
<!-- Before -->
<h1 class="text-[clamp(3rem,6vw,4.5rem)] ...">

<!-- After -->
<h1 class="text-5xl ...">
```

**Validation**:
```bash
pnpm build
# Manual: Verify h1 scales fluidly from mobile to desktop
```

---

### Phase 3: Update Section Headings

**Files**: Multiple components with `text-4xl md:text-5xl` pattern

| File | Line | Before | After |
|------|------|--------|-------|
| `Priorities.astro` | 48 | `text-4xl md:text-5xl` | `text-4xl` |
| `About.astro` | 42 | `text-4xl md:text-5xl` | `text-4xl` |
| `Endorsements.astro` | 18 | `text-4xl md:text-5xl` | `text-4xl` |
| `Action.astro` | 44 | `text-4xl md:text-5xl` | `text-4xl` |

**Validation**:
```bash
pnpm build
# Manual: All section h2 headings scale smoothly
```

---

### Phase 4: Update VoteCTA Component

**File**: `src/components/VoteCTA.astro`

| Line | Before | After |
|------|--------|-------|
| 10 | `text-6xl md:text-8xl` | `text-7xl` |
| 11 | `text-4xl md:text-5xl` | `text-4xl` |
| 14 | `text-base md:text-lg` | `text-base` |
| 38 | `text-sm md:text-base` | `text-sm` |

**Validation**:
```bash
pnpm build
# Manual: Vote date displays prominently at all sizes
```

---

### Phase 5: Update Newsletter Component

**File**: `src/components/Newsletter.astro`

| Line | Before | After |
|------|--------|-------|
| 12 | `text-3xl md:text-4xl` | `text-3xl` |

**Validation**:
```bash
pnpm build
```

---

### Phase 6: Update Footer Component

**File**: `src/components/Footer.astro`

| Line | Before | After |
|------|--------|-------|
| 20 | `text-sm md:text-base` | `text-sm` |
| 29-32, 39-42, 50 | `text-sm md:text-base` | `text-sm` |
| 73 | `text-xs md:text-sm` | `text-xs` |

**Validation**:
```bash
pnpm build
# Manual: Footer text scales appropriately
```

---

### Phase 7: Final Cleanup and Verification

**Tasks**:
1. Run grep to verify no remaining breakpoint typography patterns
2. Visual inspection at key breakpoints

**Validation**:
```bash
# Check for remaining breakpoint typography
rg "md:text-|lg:text-|sm:text-" src/components/

# Full build
pnpm build
```

---

## Files Summary

### Files to Modify (10)

| File | Changes | Phase |
|------|---------|-------|
| `src/styles/global.css` | Replace typography tokens, update button/nav font-sizes | 1 |
| `src/components/Hero.astro` | Replace inline clamp | 2 |
| `src/components/Priorities.astro` | Remove md: breakpoint | 3 |
| `src/components/About.astro` | Remove md: breakpoint | 3 |
| `src/components/Endorsements.astro` | Remove md: breakpoint | 3 |
| `src/components/Action.astro` | Remove md: breakpoint | 3 |
| `src/components/VoteCTA.astro` | Remove md: breakpoints (4 places) | 4 |
| `src/components/Newsletter.astro` | Remove md: breakpoint | 5 |
| `src/components/Footer.astro` | Remove md: breakpoints (multiple) | 6 |
| `src/components/PriorityCard.astro` | No changes needed (already using tokens) | - |

### Files NOT Modified
- `src/components/Header.astro` - already using tokens correctly
- `src/components/EndorsementCard.astro` - no breakpoint typography
- `src/components/ActionCard.astro` - no breakpoint typography
- `src/components/CampaignBadge.astro` - no breakpoint typography
- `src/components/SocialIcon.astro` - no text elements
- `src/components/Logo.astro` - SVG only

---

## Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Visual regression at specific viewports | Medium | Low | Test at 360px, 768px, 1024px, 1240px |
| Unexpected token name conflicts | Low | Low | Variable names unchanged |
| Build failures | Very Low | Low | CSS-only changes, no deps |

---

## Testing Strategy

### Manual Visual Testing
- [ ] Mobile (360px): All text readable, no overflow
- [ ] Tablet (768px): Headings properly scaled
- [ ] Desktop (1024px): Visual hierarchy maintained
- [ ] Large (1240px+): Max sizes respected

### Automated Validation
```bash
# Build validation (run after each phase)
pnpm build

# Check for remaining breakpoint typography
rg "md:text-|lg:text-|sm:text-" src/components/
# Expected: 0 results for typography (some may remain for layout)
```

---

## Success Criteria

1. All `--text-*` tokens use fluid clamp() values
2. Zero breakpoint-based typography classes for headings/body text
3. Build completes without errors
4. Visual appearance matches or improves upon current responsive behavior
5. Single source of truth for typography in global.css

---

## Confidence Score: 9/10

**Rationale**:
- CSS-only changes with no dependencies or build tool modifications
- Existing fluid spacing pattern to follow
- Clear, mechanical transformations
- Each phase independently verifiable
- Low risk of breaking changes

**Deductions**:
- -1: No automated visual regression testing (manual testing required)
