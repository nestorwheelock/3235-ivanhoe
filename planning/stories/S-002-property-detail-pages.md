# S-002: Individual Property Detail Pages

**Story Type**: User Story
**Priority**: High
**Estimate**: 1 day
**Sprint**: Sprint 1
**Status**: 📋 PENDING

---

## User Story

**As a** potential property buyer
**I want to** view complete details and all photos for a specific property
**So that** I can make an informed decision about my interest and contact the seller

---

## Acceptance Criteria

- [ ] When I click "View Details" on homepage, I navigate to individual property page
- [ ] When I view property detail page, I see a hero header image of that specific property
- [ ] When I scroll down, I see a full photo carousel with all property photos
- [ ] When I click a thumbnail in the carousel, the main image changes
- [ ] When I click the main carousel image, it opens in fullscreen lightbox
- [ ] When in lightbox, I can navigate with left/right arrows or keyboard
- [ ] When in lightbox, I can close with X button or ESC key
- [ ] When I view property details, I see all information: sq ft, year built, zoning, etc.
- [ ] When I scroll to bottom, I see a contact form auto-selected to this property
- [ ] When I view the footer, I see copyright and legal links

---

## Definition of Done

- [ ] Property detail template created
- [ ] Hero header displays property-specific image
- [ ] Full photo carousel implemented with all photos
- [ ] Thumbnail navigation strip below main image
- [ ] Main image click opens lightbox
- [ ] Lightbox navigation (arrows, keyboard, close)
- [ ] All property fields displayed (with null handling)
- [ ] Contact form auto-selects current property (S-004)
- [ ] Footer matches homepage footer
- [ ] Responsive design (mobile, tablet, desktop)
- [ ] URL structure: /property/<id>/ or /property/<slug>/
- [ ] Tests written and passing (>95% coverage)
- [ ] Documentation updated

---

## Technical Notes

**Property Detail Page Sections:**

1. **Hero Header**
   - Property-specific header image
   - Property title overlay
   - Height: 400-600px desktop, 200-300px mobile

2. **Photo Carousel**
   - Main large image display
   - Thumbnail strip below (all photos)
   - Navigation:
     - Click thumbnail → change main image
     - Click main image → open lightbox
     - Left/right arrows (optional for main carousel)

3. **Property Details**
   - Title and address
   - Property type and status badges
   - Grid layout for specs:
     - Square Footage
     - Year Built
     - Lot Size
     - Zoning
     - Current Use
   - Price display (based on price_display setting)
   - Description (formatted text)

4. **Contact Form**
   - Auto-selected to current property
   - "Also interested in [other property]" checkbox
   - Full form implementation in S-004

5. **Footer**
   - Same as homepage

**Lightbox Features:**
- Fullscreen overlay (dark background)
- Large centered image
- Left/right navigation arrows
- Close button (top-right X)
- Keyboard support:
  - Left/Right arrows: navigate
  - ESC: close
- Click outside image: close
- Touch swipe support (mobile)

**URL Design:**
- Option A: `/property/<id>/` (simple, using pk)
- Option B: `/property/<slug>/` (SEO-friendly)
- Recommendation: Use slug for better URLs

**CSS Requirements:**
- Carousel container with proper aspect ratio
- Thumbnail grid/flex layout
- Lightbox z-index above all content
- Smooth transitions for image changes
- Touch-friendly controls on mobile

**JavaScript Requirements:**
- Carousel image switching
- Lightbox open/close
- Lightbox navigation
- Keyboard event handlers
- Touch event handlers (mobile swipe)

---

## Dependencies

- Property model with slug field (T-002)
- Photo model with ordering (T-002)
- Homepage created for navigation (S-001)
- Base template (T-003)

---

## Tasks

This story is implemented through:
- T-004: Property Detail Pages Template
- T-005: Contact Forms (for auto-select logic)

---

## Notes

- Primary photo becomes hero header image
- All photos appear in carousel (ordered by display_order)
- Handle properties with 0 photos (show placeholder)
- Handle properties with 1 photo (no carousel, just display)
- Lightbox should prevent body scroll when open
- Carousel thumbnails should indicate active image
- Consider lazy loading for images (performance)
