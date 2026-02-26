# Blueprint Header Assembly - AI Instructions

**Purpose:** Definitive guide for AI tools to correctly implement Blueprint headers across all breakpoints.  
**Design System:** Blueprint Component Library (ConstructConnect)  
**Last Updated:** February 2026

---

## ⚠️ CRITICAL: What VS Code/AI Tools Get WRONG

**Common Mistakes AI Makes:**

| Mistake | Correct Behavior |
|---------|-----------------|
| Using Font Awesome for hamburger menu | Use 3 div bars with CSS transform animation |
| Icon swapping hamburger ↔ X | Animate with CSS transforms (rotate, opacity) |
| Wrong header heights | Desktop: 60px, Tablet/Mobile: 44px (EXACT) |
| Showing hamburger on desktop | Hamburger ONLY appears at ≤1024px |
| Showing full logo on mobile | Mobile shows icon-only logo |
| Showing username on mobile | Mobile shows avatar initials only, no name |
| Using wrong breakpoints | Use Blueprint breakpoints, NOT Bootstrap/Tailwind defaults |
| Wrong button sizes | Desktop: 60×60px, Tablet/Mobile: 44×44px |

---

## Blueprint Breakpoints (MANDATORY)

**DO NOT use Bootstrap, Tailwind, or other framework defaults.**

| Breakpoint | Width | Header Size | Logo | Menu Toggle | Username |
|------------|-------|-------------|------|-------------|----------|
| Mobile | 375px | Small (44px) | Icon only | ✅ Show | Initials only |
| Tablet Small | 600px | Small (44px) | Icon only | ✅ Show | Initials only |
| Tablet Portrait | 768px | Medium (44px) | Small logo | ✅ Show | Initials + Name |
| Tablet Landscape | 1024px | Medium (44px) | Small logo | ✅ Show | Initials + Name |
| Desktop | 1440px+ | Large (60px) | Full logo | ❌ Hide | Initials + Name |

**Key Breakpoint Rules:**
- `≤1024px`: Show hamburger menu, use 44px header height
- `>1024px`: Hide hamburger menu, use 60px header height
- `≤600px`: Icon-only logo, hide username text
- `>600px to ≤1024px`: Small logo with text
- `>1024px`: Full logo with text

---

## Header Component Hierarchy

```
AppHeader (organism)
├── Left Side Content
│   ├── Menu Toggle Button (molecule) ← ONLY shows ≤1024px
│   └── Header Logo and Title (molecule)
│       └── Header Logo Button (molecule)
│           └── CC Logo (atom)
│
└── Right Side Content
    ├── AppTools Container
    │   ├── Header Button - History (molecule)
    │   ├── Header Button - Notifications (molecule)
    │   └── Header Button - Chat (molecule)
    │
    └── Username Button (molecule)
        ├── Initials Avatar (atom)
        └── Name Text (conditional)
```

---

## Header Sizes - EXACT SPECIFICATIONS

### Large (Desktop - >1024px)

| Property | Value |
|----------|-------|
| Height | 60px |
| Min-width | 1140px |
| Background | #01304a |
| Padding | 0px (content fills) |
| Logo | Full logo (239.587px wide) |
| Logo left padding | 20px |
| Button size | 60×60px |
| Button icon size | 24px |
| Menu toggle | ❌ HIDDEN |

### Medium (Tablet - 768px to 1024px)

| Property | Value |
|----------|-------|
| Height | 44px |
| Min-width | 768px |
| Background | #01304a |
| Logo | Small logo (190.964px wide) |
| Logo padding | 10px |
| Button size | 44×44px |
| Button icon size | 18px |
| Menu toggle | ✅ VISIBLE |

### Small (Mobile - <768px)

| Property | Value |
|----------|-------|
| Height | 44px |
| Min-width | 360px |
| Background | #01304a |
| Logo | Icon only (27.601px wide) |
| Logo padding | 10px |
| Button size | 44×44px |
| Button icon size | 18px |
| Menu toggle | ✅ VISIBLE |
| Username text | ❌ HIDDEN (show initials only) |

---

## Menu Toggle Button - CRITICAL IMPLEMENTATION

### ⚠️ DO NOT USE FONT AWESOME ICONS

The hamburger menu is built with **three div elements** that animate using **CSS transforms**.  
**DO NOT** use `fa-bars` or `fa-times` icons and swap them.

### Menu Icon Structure (Default - Hamburger)

```html
<button class="menu-toggle-button" aria-label="Toggle navigation menu" aria-expanded="false">
  <div class="menu-icon">
    <span class="menu-icon__bar menu-icon__bar--top"></span>
    <span class="menu-icon__bar menu-icon__bar--middle"></span>
    <span class="menu-icon__bar menu-icon__bar--bottom"></span>
  </div>
</button>
```

### Menu Icon CSS

```css
.menu-toggle-button {
  width: 44px;
  height: 44px;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #01304a;
  border: none;
  border-right: 1px solid #185a7d;
  cursor: pointer;
}

.menu-icon {
  width: 16px;
  height: 16px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.menu-icon__bar {
  display: block;
  width: 16px;
  height: 3px;
  background-color: white;
  border-radius: 4px;
  transition: transform 0.3s ease, opacity 0.3s ease;
  transform-origin: center;
}

/* Bar positions in default (hamburger) state */
.menu-icon__bar--top {
  transform: translateY(0) rotate(0);
}

.menu-icon__bar--middle {
  opacity: 1;
}

.menu-icon__bar--bottom {
  transform: translateY(0) rotate(0);
}
```

### Menu Icon Animation (Open - X State)

```css
/* When menu is open, add .menu-open class to button or icon container */

.menu-open .menu-icon__bar--top {
  transform: translateY(6px) rotate(45deg);
}

.menu-open .menu-icon__bar--middle {
  opacity: 0;
}

.menu-open .menu-icon__bar--bottom {
  transform: translateY(-6px) rotate(-45deg);
}
```

### Animation Explanation

| Bar | Default State | Open State | Animation |
|-----|--------------|------------|-----------|
| Top | Position: top, Rotation: 0° | Move down 6px, Rotate +45° | translateY(6px) rotate(45deg) |
| Middle | Opacity: 1 | Opacity: 0 | Fades out |
| Bottom | Position: bottom, Rotation: 0° | Move up 6px, Rotate -45° | translateY(-6px) rotate(-45deg) |

### JavaScript Toggle

```javascript
const menuToggle = document.querySelector('.menu-toggle-button');
const menuIcon = menuToggle.querySelector('.menu-icon');

menuToggle.addEventListener('click', () => {
  const isOpen = menuIcon.classList.toggle('menu-open');
  menuToggle.setAttribute('aria-expanded', isOpen);
  // Also toggle navigation drawer visibility
});
```

---

## Header Logo Button

### Figma Properties

| Property | Options | Default |
|----------|---------|---------|
| `property1` | `Icon`, `SmallLogo`, `LargeLogo` | `LargeLogo` |
| `state` | `Default` | `Default` |

**Note:** No hover or active states. Focus state only (for keyboard navigation).

### Logo Sizes

| Variant | Width | Height | Usage |
|---------|-------|--------|-------|
| `Icon` | 27.601px | 24px | Mobile (<768px) |
| `SmallLogo` | 190.964px | ~19px | Tablet (768-1024px) |
| `LargeLogo` | 239.587px | ~24px | Desktop (>1024px) |

### Implementation

```html
<!-- Desktop: Full logo -->
<a href="/" class="header-logo-button" aria-label="ConstructConnect Home">
  <img src="/assets/cc-logo-white-full.svg" alt="ConstructConnect" width="240" height="24">
</a>

<!-- Tablet: Small logo -->
<a href="/" class="header-logo-button" aria-label="ConstructConnect Home">
  <img src="/assets/cc-logo-white-small.svg" alt="ConstructConnect" width="191" height="19">
</a>

<!-- Mobile: Icon only -->
<a href="/" class="header-logo-button" aria-label="ConstructConnect Home">
  <img src="/assets/cc-logo-icon-white.svg" alt="ConstructConnect" width="28" height="24">
</a>
```

---

## Header Buttons

### Figma Properties

| Property | Options | Default |
|----------|---------|---------|
| `size` | `Large`, `Medium` | `Large` |
| `state` | `Default`, `Hover`, `Active` | `Default` |
| `showNotification` | Boolean | `false` |

### Button Sizes

| Size | Container | Icon Container | Icon Size | Usage |
|------|-----------|----------------|-----------|-------|
| Large | 60×60px | 44×44px | 24px | Desktop |
| Medium | 44×44px | 32×32px | 18px | Tablet/Mobile |

### Button Styling

```css
.header-button {
  display: flex;
  align-items: center;
  justify-content: center;
  background: #01304a;
  border: none;
  border-radius: 30px;
  cursor: pointer;
  position: relative;
}

/* Large (Desktop) */
.header-button--large {
  width: 60px;
  height: 60px;
  padding: 10px;
}

.header-button--large .icon-container {
  width: 44px;
  height: 44px;
  border-radius: 30px;
  font-size: 24px;
}

/* Medium (Tablet/Mobile) */
.header-button--medium {
  width: 44px;
  height: 44px;
  padding: 10px;
}

.header-button--medium .icon-container {
  width: 32px;
  height: 32px;
  border-radius: 30px;
  font-size: 18px;
}

/* Icon styling */
.header-button i {
  color: white;
  font-family: 'Font Awesome 6 Pro';
  font-weight: 400; /* Regular weight */
}

/* Notification indicator */
.header-button__notification {
  position: absolute;
  width: 11px;
  height: 11px;
  background: #ed7800; /* Orange indicator */
  border-radius: 50%;
}

.header-button--large .header-button__notification {
  top: 7px;
  right: 7px;
}

.header-button--medium .header-button__notification {
  top: 5px;
  right: 5px;
}
```

---

## Username Button

### Figma Properties

| Property | Options | Default |
|----------|---------|---------|
| `showName` | Boolean | `true` |

### Structure

```html
<button class="username-button" aria-label="User menu">
  <div class="username-button__container">
    <!-- Always visible -->
    <div class="username-button__avatar">
      <span class="username-button__initials">CD</span>
    </div>
    <!-- Conditionally visible based on breakpoint -->
    <span class="username-button__name">Christopher Donaldson</span>
  </div>
</button>
```

### Styling

```css
.username-button {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  background: #01304a;
  border: none;
  padding: 2px 10px;
  cursor: pointer;
}

.username-button__container {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 2px;
}

.username-button__avatar {
  width: 34px;
  height: 34px;
  background: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
}

.username-button__initials {
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 18px;
  color: #4a4f55;
  line-height: 16px;
}

.username-button__name {
  font-family: 'Montserrat', sans-serif;
  font-weight: 500;
  font-size: 18px;
  color: white;
  line-height: 16px;
  white-space: nowrap;
}

/* Hide name on mobile */
@media (max-width: 600px) {
  .username-button__name {
    display: none;
  }
}
```

---

## Complete Header Assembly

### HTML Structure

```html
<header class="app-header" role="banner">
  <!-- Skip navigation link (accessibility) -->
  <a href="#main-content" class="skip-link">Skip to main content</a>
  
  <!-- Left side content -->
  <div class="app-header__left">
    <!-- Menu toggle - ONLY visible ≤1024px -->
    <button class="menu-toggle-button" aria-label="Toggle navigation" aria-expanded="false">
      <div class="menu-icon">
        <span class="menu-icon__bar menu-icon__bar--top"></span>
        <span class="menu-icon__bar menu-icon__bar--middle"></span>
        <span class="menu-icon__bar menu-icon__bar--bottom"></span>
      </div>
    </button>
    
    <!-- Logo -->
    <a href="/" class="header-logo-button" aria-label="ConstructConnect Home">
      <img class="header-logo header-logo--full" src="/assets/cc-logo-white-full.svg" alt="ConstructConnect">
      <img class="header-logo header-logo--small" src="/assets/cc-logo-white-small.svg" alt="ConstructConnect">
      <img class="header-logo header-logo--icon" src="/assets/cc-logo-icon-white.svg" alt="ConstructConnect">
    </a>
  </div>
  
  <!-- Right side content -->
  <div class="app-header__right">
    <!-- App tools -->
    <nav class="app-header__tools" aria-label="Header actions">
      <button class="header-button" aria-label="History">
        <div class="icon-container">
          <i class="fa-regular fa-clock-rotate-left"></i>
        </div>
      </button>
      
      <button class="header-button" aria-label="Notifications">
        <div class="icon-container">
          <i class="fa-regular fa-bell"></i>
        </div>
        <span class="header-button__notification" aria-label="New notifications"></span>
      </button>
      
      <button class="header-button" aria-label="Chat">
        <div class="icon-container">
          <i class="fa-regular fa-message"></i>
        </div>
      </button>
    </nav>
    
    <!-- Username -->
    <button class="username-button" aria-label="User menu" aria-haspopup="true">
      <div class="username-button__container">
        <div class="username-button__avatar">
          <span class="username-button__initials">CD</span>
        </div>
        <span class="username-button__name">Christopher Donaldson</span>
      </div>
    </button>
  </div>
</header>
```

### Complete Responsive CSS

```css
/* ============================================
   APP HEADER - BASE STYLES (Desktop First)
   ============================================ */

.app-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #01304a;
  width: 100%;
  min-width: 1140px;
  height: 60px;
  padding: 0;
}

.app-header__left {
  display: flex;
  align-items: center;
  flex: 1;
}

.app-header__right {
  display: flex;
  align-items: center;
  padding: 0 10px;
}

.app-header__tools {
  display: flex;
  align-items: center;
}

/* Skip link (accessibility) */
.skip-link {
  position: absolute;
  left: -9999px;
  top: 0;
  z-index: 1000;
  padding: 8px 16px;
  background: white;
  color: #01304a;
}

.skip-link:focus {
  left: 10px;
  top: 10px;
}

/* ============================================
   MENU TOGGLE - Hidden on Desktop
   ============================================ */

.menu-toggle-button {
  display: none; /* Hidden on desktop */
  width: 44px;
  height: 44px;
  padding: 10px;
  align-items: center;
  justify-content: center;
  background: #01304a;
  border: none;
  border-right: 1px solid #185a7d;
  cursor: pointer;
}

.menu-icon {
  width: 16px;
  height: 15px;
  position: relative;
}

.menu-icon__bar {
  position: absolute;
  left: 0;
  width: 16px;
  height: 3px;
  background-color: white;
  border-radius: 4px;
  transition: transform 0.3s ease, opacity 0.3s ease;
  transform-origin: center;
}

.menu-icon__bar--top { top: 0; }
.menu-icon__bar--middle { top: 6px; }
.menu-icon__bar--bottom { top: 12px; }

/* Menu open state (X) */
.menu-icon.menu-open .menu-icon__bar--top {
  transform: translateY(6px) rotate(45deg);
}

.menu-icon.menu-open .menu-icon__bar--middle {
  opacity: 0;
}

.menu-icon.menu-open .menu-icon__bar--bottom {
  transform: translateY(-6px) rotate(-45deg);
}

/* ============================================
   LOGO - Desktop (Full Logo)
   ============================================ */

.header-logo-button {
  display: flex;
  align-items: center;
  padding: 10px 10px 10px 20px;
  height: 60px;
}

.header-logo--full { display: block; width: 240px; }
.header-logo--small { display: none; }
.header-logo--icon { display: none; }

/* ============================================
   HEADER BUTTONS - Desktop (Large)
   ============================================ */

.header-button {
  width: 60px;
  height: 60px;
  padding: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #01304a;
  border: none;
  border-radius: 30px;
  cursor: pointer;
  position: relative;
}

.header-button .icon-container {
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #01304a;
  border-radius: 30px;
}

.header-button i {
  font-size: 24px;
  color: white;
}

.header-button__notification {
  position: absolute;
  top: 7px;
  right: 7px;
  width: 11px;
  height: 11px;
  background: #ed7800;
  border-radius: 50%;
}

/* ============================================
   USERNAME BUTTON
   ============================================ */

.username-button {
  display: flex;
  align-items: center;
  background: #01304a;
  border: none;
  padding: 2px 10px;
  cursor: pointer;
  height: 100%;
}

.username-button__container {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 2px;
}

.username-button__avatar {
  width: 34px;
  height: 34px;
  background: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.username-button__initials {
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  font-size: 18px;
  color: #4a4f55;
  line-height: 16px;
}

.username-button__name {
  font-family: 'Montserrat', sans-serif;
  font-weight: 500;
  font-size: 18px;
  color: white;
  line-height: 16px;
  white-space: nowrap;
}

/* ============================================
   TABLET LANDSCAPE (≤1024px)
   - Show hamburger menu
   - Use medium (44px) header
   - Use small logo
   - Use medium buttons
   ============================================ */

@media (max-width: 1024px) {
  .app-header {
    height: 44px;
    min-width: 768px;
  }
  
  .menu-toggle-button {
    display: flex;
  }
  
  .header-logo-button {
    padding: 10px;
    height: 44px;
  }
  
  .header-logo--full { display: none; }
  .header-logo--small { display: block; width: 191px; }
  .header-logo--icon { display: none; }
  
  .header-button {
    width: 44px;
    height: 44px;
  }
  
  .header-button .icon-container {
    width: 32px;
    height: 32px;
  }
  
  .header-button i {
    font-size: 18px;
  }
  
  .header-button__notification {
    top: 5px;
    right: 5px;
  }
}

/* ============================================
   TABLET PORTRAIT (≤768px)
   - Same as tablet landscape
   ============================================ */

@media (max-width: 768px) {
  .app-header {
    min-width: 600px;
  }
}

/* ============================================
   MOBILE (≤600px)
   - Icon-only logo
   - Hide username text
   ============================================ */

@media (max-width: 600px) {
  .app-header {
    min-width: 360px;
  }
  
  .header-logo--full { display: none; }
  .header-logo--small { display: none; }
  .header-logo--icon { display: block; width: 28px; }
  
  .username-button__name {
    display: none;
  }
}
```

---

## Color Reference

| Element | Token | Hex Value |
|---------|-------|-----------|
| Header background | `--navigation/appbar/background` | #01304a |
| Menu button border | `--navigation/appbar/menubutton/border` | #185a7d |
| Icon/text color | `--navigation/appbar/button/text` | #ffffff |
| Menu icon fill | `--navigation/appbar/menu-icon/fill` | #ffffff |
| Avatar background | `--navigation/appbar/profile/avatar-background` | #ffffff |
| Avatar text | `--navigation/appbar/profile/text-avatar` | #4a4f55 |
| Username text | `--navigation/appbar/profile/text-name` | #ffffff |
| Notification indicator | (indicator dot) | #ed7800 |

---

## Typography Reference

| Element | Font | Weight | Size | Line Height |
|---------|------|--------|------|-------------|
| Title text | Montserrat | 500 (Medium) | 20px | 16px |
| Username | Montserrat | 500 (Medium) | 18px | 16px |
| Initials | Montserrat | 700 (Bold) | 18px | 16px |
| Button icons (large) | Font Awesome 6 Pro | 400 | 24px | 24px |
| Button icons (medium) | Font Awesome 6 Pro | 400 | 18px | 18px |

---

## Accessibility Checklist

| Requirement | Implementation |
|-------------|----------------|
| Skip navigation link | First focusable element, links to `#main-content` |
| Landmark role | `<header role="banner">` |
| Button labels | All buttons have `aria-label` |
| Menu toggle state | `aria-expanded` toggles with menu state |
| User menu popup | `aria-haspopup="true"` on username button |
| Focus visibility | 2px outline on focus for all interactive elements |
| Keyboard navigation | Tab order: Skip link → Logo → Menu toggle → Buttons → Username |

---

## Common Mistakes to Avoid

### ❌ WRONG: Font Awesome icon swap for hamburger
```html
<!-- DON'T DO THIS -->
<button class="menu-toggle">
  <i class="fa-bars"></i>
</button>
<script>
  // Swapping icons on click - WRONG!
  icon.classList.toggle('fa-bars');
  icon.classList.toggle('fa-times');
</script>
```

### ✅ CORRECT: CSS transform animation
```html
<button class="menu-toggle-button">
  <div class="menu-icon">
    <span class="menu-icon__bar menu-icon__bar--top"></span>
    <span class="menu-icon__bar menu-icon__bar--middle"></span>
    <span class="menu-icon__bar menu-icon__bar--bottom"></span>
  </div>
</button>
```

### ❌ WRONG: Using Bootstrap/Tailwind breakpoints
```css
/* DON'T DO THIS */
@media (max-width: 992px) { /* Bootstrap lg */ }
@media (max-width: 768px) { /* Bootstrap md */ }
```

### ✅ CORRECT: Using Blueprint breakpoints
```css
@media (max-width: 1024px) { /* Tablet landscape */ }
@media (max-width: 768px) { /* Tablet portrait */ }
@media (max-width: 600px) { /* Mobile */ }
```

### ❌ WRONG: Same header height at all breakpoints
```css
/* DON'T DO THIS */
.app-header { height: 64px; }
```

### ✅ CORRECT: Different heights per breakpoint
```css
.app-header { height: 60px; } /* Desktop */
@media (max-width: 1024px) { .app-header { height: 44px; } }
```

### ❌ WRONG: Showing hamburger on desktop
```css
/* DON'T DO THIS */
.menu-toggle-button { display: flex; }
```

### ✅ CORRECT: Hamburger only on tablet/mobile
```css
.menu-toggle-button { display: none; }
@media (max-width: 1024px) { .menu-toggle-button { display: flex; } }
```

---

## Figma Component References

| Component | Node ID | Figma Link |
|-----------|---------|------------|
| Header Organism | 628-1830 | [Link](https://www.figma.com/design/gPXcCBbv2now1CwDx5TUGG/Blueprint-Component-Library?node-id=628-1830) |
| Menu Icon Atom | 12816-2509 | [Link](https://www.figma.com/design/gPXcCBbv2now1CwDx5TUGG/Blueprint-Component-Library?node-id=12816-2509) |
| Menu Toggle Button | 628-1773 | [Link](https://www.figma.com/design/gPXcCBbv2now1CwDx5TUGG/Blueprint-Component-Library?node-id=628-1773) |
| Header Logo Button | 13379-23701 | [Link](https://www.figma.com/design/gPXcCBbv2now1CwDx5TUGG/Blueprint-Component-Library?node-id=13379-23701) |
| Header Logo and Title | 12894-2877 | [Link](https://www.figma.com/design/gPXcCBbv2now1CwDx5TUGG/Blueprint-Component-Library?node-id=12894-2877) |
| Header Button | 626-1783 | [Link](https://www.figma.com/design/gPXcCBbv2now1CwDx5TUGG/Blueprint-Component-Library?node-id=626-1783) |
| Username Button | 620-1907 | [Link](https://www.figma.com/design/gPXcCBbv2now1CwDx5TUGG/Blueprint-Component-Library?node-id=620-1907) |

---

**Document maintained by:** Blueprint Design System Team  
**For questions:** Reference this document when implementing headers. If AI tools deviate from these specifications, provide this document as context.
