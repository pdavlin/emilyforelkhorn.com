# Utopia Fluid Typography Implementation Spec

## Executive Summary

- **Feature Name**: Utopia Fluid Typography System
- **Business Value**: Eliminates breakpoint-dependent typography, reduces CSS complexity, improves visual consistency across all viewport sizes
- **Complexity Score**: 3/10 (CSS-only changes, no dependencies, no DB)
- **Files to Modify**: ~12 (1 global.css + 11 components)

## Current State Analysis

### Already Implemented: Fluid Spacing
The site already uses Utopia-generated fluid spacing in `src/styles/global.css:98-160`:
```css
--space-s: clamp(1rem, 0.8977rem + 0.4545vw, 1.25rem);
--space-m: clamp(1.5rem, 1.3466rem + 0.6818vw, 1.875rem);
/* etc... */
```

### Gap: Static Typography
Typography uses static rem values (`src/styles/global.css:47-58`):
```css
--text-xs: 0.75rem;    /* 12px - static */
--text-sm: 0.85rem;    /* 13.6px - static */
--text-base: 1rem;     /* 16px - static */
/* etc... */
```

### Problem: Breakpoint-Based Responsive Typography
Components use breakpoint classes instead of fluid scaling:
- `src/components/Priorities.astro:48`: `text-4xl md:text-5xl`
- `src/components/About.astro:42`: `text-4xl md:text-5xl`
- `src/components/Hero.astro:29`: Uses inline clamp (inconsistent with system)

## Technical Architecture

### Utopia Type Scale Configuration

**Recommended Settings** (matching existing spacing config):
- Min viewport: 360px
- Max viewport: 1240px
- Min font size: 16px (1rem)
- Max font size: 20px (1.25rem)
- Type scale (min): 1.2 (Minor Third)
- Type scale (max): 1.25 (Major Third)
- Steps: -2 to 5 (xs through 5xl)

### Generated Fluid Type Scale

Replace `src/styles/global.css:44-58` with:

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

### Tailwind Integration

These CSS custom properties work with Tailwind v4 via the `@theme` block. No additional config needed since values are already in `@theme`.

## Implementation Checklist

### Phase 1: Update Global Tokens
- [ ] Replace static `--text-*` values in `src/styles/global.css` with fluid clamp() values
- [ ] Add Utopia calculator link as comment for future reference
- [ ] Remove `--text-8xl` (consolidate to 7xl as max)

### Phase 2: Update Components

#### Hero.astro
- [ ] Line 29: Replace inline `clamp(3rem,6vw,4.5rem)` with `text-5xl` token
- [ ] Line 33: `text-lg` (will be fluid automatically)
- [ ] Line 37: `text-xl` (will be fluid automatically)

#### Priorities.astro
- [ ] Line 48: Replace `text-4xl md:text-5xl` with single `text-4xl`

#### About.astro
- [ ] Line 42: Replace `text-4xl md:text-5xl` with single `text-4xl`
- [ ] Line 60, 66: `text-lg` (will be fluid automatically)

#### PriorityCard.astro
- [ ] Line 21: `text-5xl` (will be fluid automatically)
- [ ] Line 22: `text-2xl` (will be fluid automatically)

#### Endorsements.astro
- [ ] Line 18: Replace `text-4xl md:text-5xl` with single `text-4xl`

#### Action.astro
- [ ] Line 44: Replace `text-4xl md:text-5xl` with single `text-4xl`

#### VoteCTA.astro (most breakpoint-heavy component)
- [ ] Line 10: Replace `text-6xl md:text-8xl` with `text-6xl` or `text-7xl`
- [ ] Line 11: Replace `text-4xl md:text-5xl` with `text-4xl`
- [ ] Line 14: Replace `text-base md:text-lg` with `text-base`
- [ ] Line 38: Replace `text-sm md:text-base` with `text-sm`

#### Newsletter.astro
- [ ] Line 12: Replace `text-3xl md:text-4xl` with `text-3xl`

#### Footer.astro
- [ ] Line 20: Replace `text-sm md:text-base` with `text-sm`
- [ ] Lines 29-32, 39-42, 50: Replace `text-sm md:text-base` with `text-sm`
- [ ] Line 73: Replace `text-xs md:text-sm` with `text-xs`

### Phase 3: Component Classes Update

#### Button Component (global.css)
- [ ] Line 259: Replace `font-size: 0.9rem` with `font-size: var(--text-sm)`
- [ ] Line 349: Replace `font-size: 1rem` with `font-size: var(--text-base)`

#### Nav Links (global.css)
- [ ] Line 323: Replace `font-size: 0.95rem` with `font-size: var(--text-sm)`

### Phase 4: Cleanup
- [ ] Remove any remaining `md:text-*` responsive modifiers where fluid scale handles it
- [ ] Remove duplicate/unused size tokens if consolidation makes sense

## Files to Modify

| File | Changes | Complexity |
|------|---------|------------|
| `src/styles/global.css` | Replace typography tokens | Low |
| `src/components/Hero.astro` | Update h1, paragraphs | Low |
| `src/components/Priorities.astro` | Update h2 | Low |
| `src/components/About.astro` | Update h2, h3 | Low |
| `src/components/PriorityCard.astro` | Audit typography | Low |
| `src/components/EndorsementCard.astro` | Audit typography | Low |
| `src/components/Action.astro` | Audit typography | Low |
| `src/components/VoteCTA.astro` | Audit typography | Low |
| `src/components/Newsletter.astro` | Audit typography | Low |
| `src/components/Footer.astro` | Audit typography | Low |
| `src/components/Header.astro` | Already using tokens | None |

## Risk Analysis

### Technical Risks
- **Visual Regression**: Font sizes will differ slightly at various viewports
- **Mitigation**: Test at 360px, 768px, 1024px, 1240px viewports

### Breaking Changes
- None expected. CSS custom properties maintain same names.

## Testing Strategy

### Visual Testing Checklist
- [ ] Mobile (360px): All text readable, no overflow
- [ ] Tablet (768px): Headings properly scaled
- [ ] Desktop (1024px): Visual hierarchy maintained
- [ ] Large (1240px+): Max sizes respected

### Automated Validation
```bash
# Check for remaining breakpoint typography
rg "md:text-|lg:text-|sm:text-" src/components/
# Should return minimal results after implementation
```

## Summary Statistics

### Breakpoint Typography Patterns Found
| Pattern | Count | Files |
|---------|-------|-------|
| `text-4xl md:text-5xl` | 5 | Priorities, About, Endorsements, Action, VoteCTA |
| `text-6xl md:text-8xl` | 1 | VoteCTA |
| `text-3xl md:text-4xl` | 1 | Newsletter |
| `text-base md:text-lg` | 1 | VoteCTA |
| `text-sm md:text-base` | 6 | VoteCTA, Footer |
| `text-xs md:text-sm` | 1 | Footer |
| **Total** | **15** | |

### After Implementation
- 0 breakpoint-based typography patterns for headings
- All `--text-*` tokens will scale fluidly from 360px to 1240px

## Success Metrics

- Zero breakpoint-based typography classes needed for core headings/body
- Consistent type scale across all components
- Single source of truth for typography sizes in global.css

## Reference Links

- Utopia Type Calculator: https://utopia.fyi/type/calculator
- Current spacing implementation: `src/styles/global.css:98-160`
- Tailwind v4 theme docs: https://tailwindcss.com/docs/theme
