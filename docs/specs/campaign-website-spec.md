# Emily for Elkhorn Campaign Website - Feature Specification

## Executive Summary

- **Feature Name**: Campaign Website Implementation
- **Business Value**: Complete campaign website for school board candidate with donation conversion, voter engagement, and brand communication
- **Complexity Score**: 5/10 (Medium - single page, no backend, multiple sections with responsive styling)
- **Current State**: ~15% complete (Header, Hero partial, Tailwind config established)

## Current Implementation Analysis

### What Exists

| Component | Status | File | Notes |
|-----------|--------|------|-------|
| Layout | Complete | `src/layouts/Layout.astro` | Base HTML, font loading missing |
| Header/Nav | Partial | `src/components/Header.astro` | Logo placeholder, mobile menu missing |
| Hero | Partial | `src/components/Welcome.astro` | Text content done, image card incomplete |
| Tailwind Theme | Complete | `src/styles/global.css` | Colors, typography, spacing, buttons |
| Priorities | Missing | - | - |
| About | Missing | - | - |
| Endorsements | Missing | - | - |
| Action/CTA | Missing | - | - |
| Vote CTA | Missing | - | - |
| Newsletter | Missing | - | - |
| Footer | Missing | - | - |
| SVG Logo | Missing | - | - |
| Public assets | Missing | - | No favicon, no images |

### Design System Already Configured

**Colors** (in `--color-*`):
- Navy: `#1B2B5A`
- Red: `#E31837`
- Cream: `#F5F3EF`
- Gold: `#D4B87A`
- Gray-light: `#E8E6E1`

**Fonts** (need Google Fonts link):
- Display: Playfair Display (700, 800)
- Body: Outfit (300-700)

**Component Classes** (in `@layer components`):
- `.btn`, `.btn-primary`, `.btn-secondary`, `.btn-outline`, `.btn-white`
- `.nav-links`
- `.donate-btn`
- `.animate-fade-in-up`, `.animate-fade-in-up-delayed`

**Utility Classes** (in `@layer utilities`):
- `.text-caps`, `.text-section-label`
- `.heading-display`
- `.card-hover`
- `.gradient-bar-top`

## Implementation Plan

### Phase 1: Infrastructure

1. **Add Google Fonts to Layout** (`src/layouts/Layout.astro`)
   - Preconnect to fonts.googleapis.com
   - Load Outfit (300,400,500,600,700) and Playfair Display (400,600,700,800)

2. **Create SVG Logo Component** (`src/components/Logo.astro`)
   - Encapsulate the sunburst + flag SVG from the design
   - Props: `size`, `variant` (light/dark)

3. **Create public assets directory**
   - `public/favicon.svg` - logo as favicon
   - `public/images/` - placeholder for candidate photo

### Phase 2: Complete Existing Components

4. **Fix Header** (`src/components/Header.astro`)
   - Replace "logo" text with Logo component
   - Add mobile menu button
   - Add mobile menu state (client-side JS)

5. **Complete Hero** (`src/components/Welcome.astro` or rename to `Hero.astro`)
   - Add hero image card with gradient overlay
   - Add slogan overlay on image
   - Responsive: stack on mobile, image above text

### Phase 3: New Sections

6. **Priorities Section** (`src/components/Priorities.astro`)
   - 5 priority cards with background images
   - Featured card (full-width) for first priority
   - Color overlays: navy, red, dark variants
   - Responsive: single column on mobile

7. **About Section** (`src/components/About.astro`)
   - Two-column layout: badge + text
   - Circular campaign badge with decorative border
   - Credentials blocks (Education, Recognition)
   - Responsive: badge centered above text on mobile

8. **Endorsements Section** (`src/components/Endorsements.astro`)
   - Grid of endorsement cards
   - Icon + name format
   - Placeholder style for pending endorsements
   - Responsive: auto-fit grid

9. **Action Section** (`src/components/Action.astro`)
   - Three-column grid: Donate, Volunteer, Yard Sign
   - Glassmorphism cards on red background
   - Icon + title + description + button

10. **Vote CTA Section** (`src/components/VoteCTA.astro`)
    - Large date display (May 12, 2026)
    - Two buttons: polling place + early voting

11. **Newsletter Section** (`src/components/Newsletter.astro`)
    - Email input + subscribe button (form, no backend)
    - Social media links row

12. **Footer** (`src/components/Footer.astro`)
    - Four-column layout: brand, Campaign links, Get Involved links, Contact
    - Social icons
    - Bottom bar: copyright + legal links
    - Responsive: stack on mobile

### Phase 4: Polish

13. **Mobile Menu Implementation**
    - Hamburger animation
    - Slide-down menu
    - Close on link click

14. **Accessibility Audit**
    - Skip link
    - Focus states
    - ARIA labels
    - Color contrast (already good per design)

15. **Performance**
    - Image optimization
    - Font subsetting (optional)
    - Preload critical resources

## Component Architecture

```
src/
  components/
    Logo.astro              # SVG logo, reusable
    Header.astro            # Nav with mobile menu
    Hero.astro              # Hero section (rename Welcome.astro)
    Priorities.astro        # Campaign priorities grid
    PriorityCard.astro      # Individual priority card
    About.astro             # About Emily section
    CampaignBadge.astro     # Circular badge element
    Endorsements.astro      # Endorsements grid
    EndorsementCard.astro   # Individual endorsement
    Action.astro            # Get involved section
    ActionCard.astro        # Individual action card
    VoteCTA.astro           # Vote date callout
    Newsletter.astro        # Newsletter + social
    Footer.astro            # Site footer
    SocialIcon.astro        # Reusable social icon
  layouts/
    Layout.astro            # Base layout (update fonts)
  pages/
    index.astro             # Compose all sections
  styles/
    global.css              # Theme + components (done)
public/
  favicon.svg
  images/
    emily-headshot.jpg      # Candidate photo (placeholder)
```

## Data Structures

### Priorities Data
```typescript
interface Priority {
  number: string;       // "01", "02", etc.
  title: string;
  description: string;
  bullets: string[];
  variant: 'navy' | 'red' | 'dark';
  backgroundImage: string;
  featured?: boolean;
}
```

### Endorsements Data
```typescript
interface Endorsement {
  name: string;
  icon: 'education' | 'person' | 'group';
  placeholder?: boolean;  // Dashed border style
}
```

### Action Cards Data
```typescript
interface ActionCard {
  title: string;
  description: string;
  icon: string;
  buttonText: string;
  buttonUrl: string;
}
```

### Social Links Data
```typescript
interface SocialLink {
  platform: 'facebook' | 'instagram' | 'threads' | 'tiktok';
  url: string;
}
```

## CSS Additions Needed

Add to `src/styles/global.css`:

```css
@layer components {
  /* Hero image card */
  .hero-image-card {
    position: relative;
    border-radius: var(--radius-xl);
    overflow: hidden;
    box-shadow: var(--shadow-2xl);
  }

  /* Priority card styles */
  .priority-card {
    border-radius: var(--radius-xl);
    padding: var(--space-l);
    position: relative;
    overflow: hidden;
    min-height: 320px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
  }

  /* Section container */
  .section-container {
    max-width: var(--container-xl);
    margin: 0 auto;
    padding-inline: var(--space-m);
  }

  /* Endorsement card */
  .endorsement-card {
    background: var(--color-white);
    padding: var(--space-m);
    border-radius: var(--radius-lg);
    display: flex;
    align-items: center;
    gap: var(--space-s);
  }

  /* Action card glassmorphism */
  .action-card-glass {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border-radius: var(--radius-xl);
    padding: var(--space-xl);
  }
}
```

## Responsive Breakpoints

Design uses these breakpoints (already in theme):
- `max-lg` (< 1024px): Hero stacks, grid collapses
- `max-md` (< 768px): Nav hides, mobile menu shows, footer stacks

## External Dependencies

None required. Current stack is sufficient:
- Astro 6.0.0-beta.1
- Tailwind CSS 4.1.18
- @tailwindcss/vite 4.1.18

## Testing Checklist

- [ ] All sections render on desktop (1200px+)
- [ ] Responsive at 1024px (tablet)
- [ ] Responsive at 768px (mobile)
- [ ] Mobile menu opens/closes
- [ ] All external links work (ActBlue, voter info)
- [ ] Anchor links scroll smoothly
- [ ] Images load (when added)
- [ ] Fonts load correctly
- [ ] Color contrast passes WCAG AA
- [ ] Keyboard navigation works
- [ ] Screen reader announces content correctly

## Implementation Order (Recommended)

1. Layout.astro - Add Google Fonts
2. Logo.astro - Create reusable logo
3. Header.astro - Complete with logo, mobile menu
4. Hero.astro - Rename and complete
5. Priorities.astro + PriorityCard.astro
6. About.astro + CampaignBadge.astro
7. Endorsements.astro + EndorsementCard.astro
8. Action.astro + ActionCard.astro
9. VoteCTA.astro
10. Newsletter.astro
11. Footer.astro + SocialIcon.astro
12. index.astro - Wire up all sections
13. Mobile menu JS
14. Favicon + image placeholders
15. Final QA pass

## Files to Modify

| File | Action |
|------|--------|
| `src/layouts/Layout.astro` | Add font preconnect/links |
| `src/components/Header.astro` | Complete with logo + mobile |
| `src/components/Welcome.astro` | Rename to Hero.astro, complete |
| `src/pages/index.astro` | Compose all sections |
| `src/styles/global.css` | Add new component classes |

## Files to Create

| File | Purpose |
|------|---------|
| `src/components/Logo.astro` | SVG logo component |
| `src/components/Hero.astro` | (rename from Welcome) |
| `src/components/Priorities.astro` | Priorities section |
| `src/components/PriorityCard.astro` | Individual priority |
| `src/components/About.astro` | About section |
| `src/components/CampaignBadge.astro` | Circular badge |
| `src/components/Endorsements.astro` | Endorsements section |
| `src/components/EndorsementCard.astro` | Individual endorsement |
| `src/components/Action.astro` | Get involved section |
| `src/components/ActionCard.astro` | Individual action card |
| `src/components/VoteCTA.astro` | Vote date section |
| `src/components/Newsletter.astro` | Newsletter + social |
| `src/components/Footer.astro` | Footer |
| `src/components/SocialIcon.astro` | Reusable social icon |
| `public/favicon.svg` | Site favicon |

## Content Notes

All text content is provided in the original HTML design. Key content:

- **Campaign slogan**: "Strong Schools, Strong Community"
- **Election date**: May 12, 2026
- **Donation URL**: `https://secure.actblue.com/donate/emilyforelkhorn`
- **Email**: `hello@emilyforelkhorn.com`
- **Voter info**: Douglas County, NE election sites

## Out of Scope

- Backend newsletter signup (form only, no submission handling)
- CMS integration
- Blog/news page
- Analytics (can be added later)
- Contact form backend
- Volunteer signup backend
