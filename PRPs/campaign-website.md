# PRP: Emily for Elkhorn Campaign Website

## Overview

**Feature**: Complete campaign website implementation for Emily Poeschl school board campaign
**Complexity**: 5/10 (Medium)
**Estimated Components**: 15 new files, 4 modified files
**Current Progress**: ~15% complete

## Context

### Tech Stack
- Astro 6.0.0-beta.1
- Tailwind CSS 4.1.18 (via @tailwindcss/vite)
- Package manager: pnpm
- No testing framework configured
- No TypeScript strict mode (vanilla Astro)

### Existing Patterns
- Components use `.astro` single-file format
- Tailwind classes inline with component markup
- CSS variables defined in `@theme` block (Tailwind 4 syntax)
- Component classes in `@layer components`
- No props typing in existing components
- Layout wraps content with `<slot />`

### Design System (Already Configured)
Colors: navy (#1B2B5A), red (#E31837), cream (#F5F3EF), gold (#D4B87A)
Fonts: Playfair Display (display), Outfit (body) - NOT loaded yet
Spacing: Fluid scale via clamp() (--space-xs through --space-8xl)
Buttons: .btn, .btn-primary, .btn-secondary, .btn-outline, .btn-white

## Documentation References

- Astro Components: https://docs.astro.build/en/basics/astro-components/
- Astro Font Loading: https://docs.astro.build/en/guides/fonts/
- Tailwind CSS 4: https://tailwindcss.com/docs/v4-beta

## Implementation Blueprint

### Phase 1: Infrastructure (Files 1-3)

**Goal**: Font loading and reusable logo component

#### 1.1 Update Layout.astro
```astro
---
// Add preconnect and font links in <head>
---
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700&family=Playfair+Display:wght@400;600;700;800&display=swap" rel="stylesheet">
```

#### 1.2 Create Logo.astro
```astro
---
interface Props {
  size?: 'sm' | 'md' | 'lg';
  variant?: 'color' | 'white';
}
const { size = 'md', variant = 'color' } = Astro.props;
const sizes = { sm: 40, md: 60, lg: 80 };
---
<svg viewBox="0 0 100 100" width={sizes[size]} height={sizes[size]}>
  <!-- Sunburst rays + flag motif -->
  <!-- Navy/red/gold based on variant -->
</svg>
```

#### 1.3 Create favicon.svg
- Simple version of logo for browser tab
- Export to public/favicon.svg

**Validation**:
```bash
pnpm build
# Check that fonts load in browser dev tools Network tab
# Verify favicon appears in browser tab
```

---

### Phase 2: Header Completion (File 4)

**Goal**: Working header with logo and mobile menu

#### 2.1 Update Header.astro
- Import and use Logo component
- Add mobile menu button (hamburger icon)
- Add mobile menu panel (hidden by default)
- Client-side JS for toggle

```astro
---
import Logo from './Logo.astro';
---
<nav class="fixed z-(--z-fixed) w-full bg-white shadow-sm">
  <div class="max-w-(--container-xl) mx-auto px-(--space-m) py-(--space-s) flex justify-between items-center">
    <a href="/" aria-label="Home">
      <Logo size="md" />
    </a>
    <!-- Desktop nav (unchanged) -->
    <!-- Mobile menu button -->
    <button id="mobile-menu-btn" class="md:hidden" aria-label="Toggle menu">
      <!-- Hamburger SVG -->
    </button>
  </div>
  <!-- Mobile menu panel -->
  <div id="mobile-menu" class="hidden md:hidden">
    <!-- Mobile nav links -->
  </div>
</nav>

<script>
  const btn = document.getElementById('mobile-menu-btn');
  const menu = document.getElementById('mobile-menu');
  btn?.addEventListener('click', () => menu?.classList.toggle('hidden'));
</script>
```

**Validation**:
```bash
pnpm build
# Manual: Resize browser to mobile, verify hamburger appears
# Manual: Click hamburger, verify menu toggles
```

---

### Phase 3: Hero Section (File 5)

**Goal**: Complete hero with candidate image card

#### 3.1 Rename Welcome.astro to Hero.astro
- Add image placeholder in right card
- Add slogan overlay on image area
- Verify responsive stacking

**Validation**:
```bash
pnpm build
# Manual: Verify hero displays correctly at desktop/tablet/mobile
```

---

### Phase 4: Priorities Section (Files 6-7)

**Goal**: Campaign priorities grid with styled cards

#### 4.1 Create PriorityCard.astro
```astro
---
interface Props {
  number: string;
  title: string;
  description: string;
  bullets: string[];
  variant: 'navy' | 'red' | 'dark';
  featured?: boolean;
}
---
<article class={`priority-card bg-${variant} ${featured ? 'col-span-full' : ''}`}>
  <span class="priority-number">{number}</span>
  <h3>{title}</h3>
  <p>{description}</p>
  <ul>
    {bullets.map(b => <li>{b}</li>)}
  </ul>
</article>
```

#### 4.2 Create Priorities.astro
- Section container with grid
- Map over priorities data
- First item featured (full width)

**Validation**:
```bash
pnpm build
# Manual: Verify 5 priority cards render
# Manual: First card spans full width
# Manual: Cards stack on mobile
```

---

### Phase 5: About Section (Files 8-9)

**Goal**: Candidate bio with campaign badge

#### 5.1 Create CampaignBadge.astro
- Circular badge with decorative border
- Logo + slogan text
- Responsive sizing

#### 5.2 Create About.astro
- Two-column layout
- Badge on left, bio text on right
- Credentials blocks (Education, Recognition)

**Validation**:
```bash
pnpm build
# Manual: Verify badge displays with border
# Manual: Text wraps correctly
# Manual: Stacks on mobile (badge above)
```

---

### Phase 6: Endorsements Section (Files 10-11)

**Goal**: Grid of endorsement cards

#### 6.1 Create EndorsementCard.astro
```astro
---
interface Props {
  name: string;
  icon: 'education' | 'person' | 'group';
  placeholder?: boolean;
}
---
<div class={`endorsement-card ${placeholder ? 'border-dashed' : ''}`}>
  <span class="icon">{/* SVG based on icon prop */}</span>
  <span class="name">{name}</span>
</div>
```

#### 6.2 Create Endorsements.astro
- Section with auto-fit grid
- Mix of confirmed and placeholder cards

**Validation**:
```bash
pnpm build
# Manual: Verify grid layout
# Manual: Placeholder cards have dashed border
```

---

### Phase 7: Action Section (Files 12-13)

**Goal**: Get involved CTAs with glassmorphism

#### 7.1 Create ActionCard.astro
```astro
---
interface Props {
  title: string;
  description: string;
  icon: string;
  buttonText: string;
  buttonUrl: string;
}
---
<div class="action-card-glass">
  <!-- icon, title, description, button -->
</div>
```

#### 7.2 Create Action.astro
- Red background section
- Three cards: Donate, Volunteer, Yard Sign
- Glassmorphism effect on cards

**Validation**:
```bash
pnpm build
# Manual: Verify red background
# Manual: Cards have blur/transparency effect
# Manual: Buttons link correctly
```

---

### Phase 8: Vote CTA Section (File 14)

**Goal**: Election date callout

#### 8.1 Create VoteCTA.astro
- Large "May 12, 2026" display
- Two buttons: polling place + early voting
- Links to Douglas County election sites

**Validation**:
```bash
pnpm build
# Manual: Date displays prominently
# Manual: Buttons link to correct external URLs
```

---

### Phase 9: Newsletter Section (Files 15-16)

**Goal**: Email signup and social links

#### 9.1 Create SocialIcon.astro
```astro
---
interface Props {
  platform: 'facebook' | 'instagram' | 'threads' | 'tiktok';
  url: string;
}
---
<a href={url} aria-label={platform}>
  <!-- SVG icon for platform -->
</a>
```

#### 9.2 Create Newsletter.astro
- Email input + subscribe button (form only, no backend)
- Row of social icons below

**Validation**:
```bash
pnpm build
# Manual: Form renders (won't submit anywhere)
# Manual: Social icons link correctly
```

---

### Phase 10: Footer (File 17)

**Goal**: Site footer with navigation

#### 10.1 Create Footer.astro
- Four columns: Brand, Campaign, Get Involved, Contact
- Social icons row
- Bottom bar: copyright + legal links
- Responsive stacking

**Validation**:
```bash
pnpm build
# Manual: Four columns on desktop
# Manual: Stacks on mobile
# Manual: All links work
```

---

### Phase 11: Integration (File 18)

**Goal**: Wire all sections into index.astro

#### 11.1 Update index.astro
```astro
---
import Layout from '../layouts/Layout.astro';
import Hero from '../components/Hero.astro';
import Priorities from '../components/Priorities.astro';
import About from '../components/About.astro';
import Endorsements from '../components/Endorsements.astro';
import Action from '../components/Action.astro';
import VoteCTA from '../components/VoteCTA.astro';
import Newsletter from '../components/Newsletter.astro';
import Footer from '../components/Footer.astro';
---
<Layout>
  <Hero />
  <Priorities />
  <About />
  <Endorsements />
  <Action />
  <VoteCTA />
  <Newsletter />
  <Footer />
</Layout>
```

**Validation**:
```bash
pnpm build
# Manual: All sections appear in order
# Manual: Scroll through entire page
# Manual: No layout breaks
```

---

### Phase 12: CSS Additions (File 19)

**Goal**: Add component classes to global.css

#### 12.1 Update global.css
Add to `@layer components`:
- `.hero-image-card`
- `.priority-card`
- `.section-container`
- `.endorsement-card`
- `.action-card-glass`

**Validation**:
```bash
pnpm build
# Manual: Verify component styles apply correctly
```

---

### Phase 13: Polish

**Goal**: Accessibility and final touches

#### 13.1 Add skip link to Layout
#### 13.2 Verify focus states on interactive elements
#### 13.3 Add ARIA labels where missing
#### 13.4 Test keyboard navigation

**Validation**:
```bash
pnpm build
# Manual: Tab through page, verify focus visible
# Manual: Screen reader test (VoiceOver)
# Manual: Check color contrast (already good per design)
```

---

## Files Summary

### Files to Create (17)
| File | Phase | Purpose |
|------|-------|---------|
| `src/components/Logo.astro` | 1 | SVG logo component |
| `public/favicon.svg` | 1 | Browser tab icon |
| `src/components/Hero.astro` | 3 | Hero section (rename from Welcome) |
| `src/components/PriorityCard.astro` | 4 | Individual priority card |
| `src/components/Priorities.astro` | 4 | Priorities section |
| `src/components/CampaignBadge.astro` | 5 | Circular badge element |
| `src/components/About.astro` | 5 | About section |
| `src/components/EndorsementCard.astro` | 6 | Individual endorsement |
| `src/components/Endorsements.astro` | 6 | Endorsements section |
| `src/components/ActionCard.astro` | 7 | Individual action card |
| `src/components/Action.astro` | 7 | Get involved section |
| `src/components/VoteCTA.astro` | 8 | Vote date section |
| `src/components/SocialIcon.astro` | 9 | Reusable social icon |
| `src/components/Newsletter.astro` | 9 | Newsletter + social |
| `src/components/Footer.astro` | 10 | Site footer |

### Files to Modify (4)
| File | Phase | Changes |
|------|-------|---------|
| `src/layouts/Layout.astro` | 1 | Add Google Fonts, skip link |
| `src/components/Header.astro` | 2 | Logo component, mobile menu |
| `src/pages/index.astro` | 11 | Import and compose all sections |
| `src/styles/global.css` | 12 | Add component classes |

### Files to Delete (1)
| File | Phase | Reason |
|------|-------|--------|
| `src/components/Welcome.astro` | 3 | Renamed to Hero.astro |

---

## Content Data

### Priorities Content
```typescript
const priorities = [
  {
    number: "01",
    title: "Accountable Governance",
    description: "Transparent decision-making that serves our students and community.",
    bullets: ["Open board meetings", "Clear financial reporting", "Community input sessions"],
    variant: "navy",
    featured: true
  },
  {
    number: "02",
    title: "Educational Excellence",
    description: "Maintaining high academic standards across all schools.",
    bullets: ["Rigorous curriculum", "Teacher support", "Student achievement"],
    variant: "red"
  },
  {
    number: "03",
    title: "Responsible Growth",
    description: "Planning for enrollment growth without compromising quality.",
    bullets: ["Facility planning", "Class size management", "Resource allocation"],
    variant: "dark"
  },
  {
    number: "04",
    title: "Future-Ready Students",
    description: "Preparing students for careers and citizenship.",
    bullets: ["Career pathways", "Technology skills", "Critical thinking"],
    variant: "navy"
  },
  {
    number: "05",
    title: "Community Partnership",
    description: "Building bridges between schools and families.",
    bullets: ["Parent engagement", "Local business connections", "Volunteer programs"],
    variant: "red"
  }
];
```

### Action Cards Content
```typescript
const actions = [
  {
    title: "Donate",
    description: "Every dollar helps spread Emily's message to voters.",
    icon: "heart",
    buttonText: "Contribute",
    buttonUrl: "https://secure.actblue.com/donate/emilyforelkhorn"
  },
  {
    title: "Volunteer",
    description: "Join our team of dedicated supporters.",
    icon: "users",
    buttonText: "Sign Up",
    buttonUrl: "#volunteer"
  },
  {
    title: "Yard Sign",
    description: "Show your support in your neighborhood.",
    icon: "home",
    buttonText: "Request Sign",
    buttonUrl: "#yard-sign"
  }
];
```

### Social Links
```typescript
const socials = [
  { platform: "facebook", url: "https://facebook.com/emilyforelkhorn" },
  { platform: "instagram", url: "https://instagram.com/emilyforelkhorn" },
  { platform: "threads", url: "https://threads.net/emilyforelkhorn" },
  { platform: "tiktok", url: "https://tiktok.com/@emilyforelkhorn" }
];
```

---

## Validation Commands

```bash
# Build validation (run after each phase)
pnpm build

# Development server (for manual testing)
pnpm dev

# Check for TypeScript errors (if any)
pnpm astro check
```

---

## Risk Assessment

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Astro 6 beta breaking changes | Medium | Pin version, check changelogs |
| Tailwind 4 syntax issues | Low | Theme already working, follow patterns |
| Mobile menu JS complexity | Low | Use simple toggle, no external deps |
| Font loading FOUT | Low | Use `display=swap`, preconnect |
| Responsive layout bugs | Medium | Test at each breakpoint per phase |

---

## Confidence Score: 8/10

**Rationale**:
- Design system fully established in CSS
- Clear component boundaries from spec
- No backend complexity
- Existing code patterns to follow
- Main uncertainty: Astro 6 beta quirks

**Deductions**:
- -1: No test framework (manual validation only)
- -1: Astro 6 beta may have undocumented behaviors

---

## Success Criteria

1. All 12 sections render without errors
2. Mobile menu toggles correctly
3. All external links work (ActBlue, voter info)
4. Fonts load (Playfair Display, Outfit)
5. Responsive at 1200px, 1024px, 768px breakpoints
6. Keyboard navigation works throughout
7. Build completes with no errors
