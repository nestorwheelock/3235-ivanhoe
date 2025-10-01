# Wireframes - 3235ivanhoe.com

Visual layout specifications for the property listing platform.

---

## Overview

These ASCII wireframes show the exact layout and structure of all pages. They illustrate:
- Component placement and hierarchy
- Content organization
- User interaction flows
- Responsive behavior (desktop vs mobile)
- Form field layouts
- Photo carousel mechanics

---

## Wireframe Files

### 1. [Homepage - Desktop](01-homepage-desktop.txt)

**Shows:**
- Hero header with both properties image
- Editable intro text section
- Stacked property cards (3233 and 3235)
- Large primary photo + 3 clickable thumbnails per card
- Property information display
- Contact form with property selection dropdown
- Footer with legal links

**Key Features:**
- Property cards stack vertically (not side-by-side)
- Thumbnails change main photo when clicked
- "View Full Details" buttons link to property pages
- Contact form at bottom with manual property selection

---

### 2. [Property Detail Page - Desktop](02-property-detail-desktop.txt)

**Shows:**
- Property-specific hero header
- Full photo carousel with all property photos
- Thumbnail navigation strip (scrollable if many photos)
- Click main image → opens lightbox/fullscreen
- Complete property specifications (grid layout)
- Full property description with formatting
- Contact form with auto-selection + "also interested in" checkbox
- Footer

**Key Features:**
- Carousel displays ALL property photos (unlimited)
- Active thumbnail highlighted
- Lightbox overlay with navigation arrows
- Keyboard support (arrows, ESC)
- Property auto-selected in contact form
- Checkbox to also express interest in other property

---

### 3. [Mobile Views](03-mobile-views.txt)

**Shows:**
- Homepage mobile layout (< 768px)
- Property detail mobile layout
- Mobile lightbox design
- Touch interactions
- Responsive breakpoints

**Key Features:**
- Single column stacked layout
- Touch-friendly buttons (44px+ height)
- Swipeable carousel
- Horizontal scroll thumbnails
- Reduced hero height (250px vs 500px)
- Full-width form fields
- Mobile lightbox with swipe support

---

## Design Patterns

### Color Scheme
*(To be defined in BUILD phase)*
- Primary: Professional blue or neutral
- Secondary: Accent color for CTAs
- Background: White/light gray
- Text: Dark gray/black
- Status badges: Color-coded (green=available, yellow=coming soon, red=sold)

### Typography
- Headers: Large, bold, readable
- Body: 16px base (desktop), 14-16px (mobile)
- Buttons: 16px+, bold
- Forms: 14-16px labels, 16px inputs

### Spacing
- Desktop: Generous padding (40-60px sections)
- Mobile: Tighter but readable (20-30px)
- Card padding: 20-30px
- Form fields: 10-15px margin between

### Interactive Elements
- Buttons: Clear hover states
- Thumbnails: Border/highlight when active
- Links: Underline on hover
- Form fields: Focus states clearly visible
- Carousel arrows: Visible but not obtrusive

---

## User Flows

### Homepage → Property Detail
```
Homepage
  ↓
Click "View Full Details" on property card
  ↓
Property Detail Page
  ↓
View photos in carousel
  ↓
Click main image → Lightbox opens
  ↓
Navigate photos with arrows/keyboard
  ↓
Close lightbox (ESC or X)
  ↓
Scroll to contact form
  ↓
Fill form (auto-selected property)
  ↓
Submit → Thank you page
```

### Homepage Contact Flow
```
Homepage
  ↓
Scroll to contact form
  ↓
Select property from dropdown
  ↓
Fill form fields
  ↓
Optionally check "make an offer"
  ↓
Offer amount field appears
  ↓
Submit → Thank you page
```

---

## Responsive Breakpoints

### Mobile (< 768px)
- Single column layout
- Stacked property cards
- Full-width elements
- Reduced hero height (250px)
- Touch-optimized controls
- Swipeable carousels

### Tablet (768px - 1024px)
- Still single column for simplicity
- Larger touch targets
- More padding/spacing
- Same layout as mobile, just roomier

### Desktop (> 1024px)
- Full width layout (max-width: 1200px)
- Taller hero (500px)
- Side-by-side form fields (2 columns)
- Hover states for interactive elements
- Larger photos in carousel

---

## Component Details

### Property Card (Homepage)
```
┌─────────────────────────────┐
│ Large Primary Photo         │  ← Clickable (goes to detail)
├─────────────────────────────┤
│ [Thumb1] [Thumb2] [Thumb3] │  ← Clickable (changes main)
├─────────────────────────────┤
│ Title + Status Badge        │
│ Address                     │
│ Type, Sq Ft, Year           │
│ Price                       │
│ Description Preview         │
│ [View Full Details →]       │  ← CTA button
└─────────────────────────────┘
```

### Photo Carousel (Detail Page)
```
┌─────────────────────────────┐
│ ← [Main Large Image] →      │  ← Click for lightbox
├─────────────────────────────┤
│ [1] [2] [3] [4] [5] [6]... │  ← Thumbnails
└─────────────────────────────┘
```

### Contact Form Fields

**Homepage version:**
- Property Selection (dropdown) - Required
- Name, Email, Phone - Required
- Company - Optional
- Buyer Type, Timeline, Financing - Optional dropdowns
- Making Offer (checkbox) - Optional
- Offer Amount - Conditional (shows if checkbox checked)
- Message - Optional

**Property Page version:**
- Property auto-selected (hidden field)
- "Also interested in [other property]" (checkbox)
- All other fields same as homepage

---

## Notes for Development

1. **Photo Handling:**
   - Primary photo shows first in carousel
   - If no primary set, use first photo by display_order
   - Handle properties with 0 photos (placeholder)
   - Handle properties with < 4 photos (repeat or show available)

2. **Form Validation:**
   - Client-side: instant feedback on required fields
   - Server-side: validate all data before save
   - Offer amount required if "making offer" checked
   - Email format validation
   - Phone format flexible (allow various formats)

3. **Carousel Behavior:**
   - Thumbnail click changes main image (fade transition)
   - Arrow keys navigate (desktop)
   - Swipe gestures navigate (mobile)
   - Lightbox prevents body scroll when open
   - Lightbox auto-fits large images

4. **Responsive Images:**
   - Serve appropriately sized images per device
   - Lazy load images below the fold
   - Use srcset for retina displays
   - Optimize file sizes (compression)

5. **Accessibility:**
   - Alt text on all images
   - Keyboard navigable carousel
   - ARIA labels on form fields
   - Focus states clearly visible
   - Color contrast meets WCAG AA

---

## Questions or Changes?

If any layout needs adjustment during BUILD phase, update these wireframes first to maintain alignment between SPEC and implementation.

---

**Created:** 2025-10-01
**Status:** SPEC Phase - Approved for BUILD
