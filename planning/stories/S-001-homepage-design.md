# S-001: Homepage with Hero & Property Cards

**Story Type**: User Story
**Priority**: High
**Estimate**: 1 day
**Sprint**: Sprint 1
**Status**: 📋 PENDING

---

## User Story

**As a** potential property buyer
**I want to** view both properties on the homepage with photos and key details
**So that** I can quickly assess my interest and navigate to full property information

---

## Acceptance Criteria

- [ ] When I visit the homepage, I see a hero header image showing both properties
- [ ] When I scroll down, I see an editable intro section with formatted text
- [ ] When I view property cards, I see them stacked vertically (not side-by-side)
- [ ] When I see a property card, it displays a large primary photo
- [ ] When I see a property card, I see 3 thumbnail previews below the main photo
- [ ] When I click a thumbnail, the main photo changes to that image
- [ ] When I view a property card, I see key info: address, type, status, price
- [ ] When I click "View Details", I navigate to the individual property page
- [ ] When I scroll to bottom, I see a contact form
- [ ] When I view the footer, I see copyright and links to Privacy Policy and Terms

---

## Definition of Done

- [ ] Homepage template created with all sections
- [ ] Hero header image displays correctly
- [ ] Editable intro section pulls from SiteContent model
- [ ] Property cards render for all properties (ordered by display_order)
- [ ] Large photo + 3 thumbnails display per card
- [ ] Thumbnail click changes main photo (JavaScript)
- [ ] Property info displays: address, type, status, price logic
- [ ] "View Details" buttons link to property detail pages
- [ ] Contact form section present (form logic in S-004)
- [ ] Footer with copyright and legal links
- [ ] Responsive design (works on mobile, tablet, desktop)
- [ ] Tests written and passing (>95% coverage)
- [ ] Documentation updated

---

## Technical Notes

**Homepage Sections:**
1. **Hero Header**
   - Full-width header image (both properties)
   - Site title/logo overlay
   - Height: 400-600px on desktop, 200-300px on mobile

2. **Intro Section**
   - Editable content from SiteContent model (key='homepage_intro')
   - Renders HTML safely
   - Styled text container

3. **Property Cards (Stacked)**
   - Loop through Property.objects.all().order_by('display_order')
   - For each property:
     - Large primary photo (or first photo if no primary set)
     - 3 thumbnails (photos 2-4, or repeat if fewer)
     - Property details
     - "View Details" button

4. **Contact Form Section**
   - Form implementation in S-004
   - Property selection dropdown

5. **Footer**
   - Copyright © 2025 3235 Ivanhoe
   - Privacy Policy | Terms of Use links

**CSS Requirements:**
- Responsive grid layout
- Mobile-first design
- Card styling with shadows/borders
- Image aspect ratio maintenance
- Touch-friendly clickable areas

**JavaScript Requirements:**
- Thumbnail click handler
- Photo swap animation (fade or slide)
- Smooth scroll to contact form (optional)

---

## Dependencies

- Property model created (T-002)
- Photo model created (T-002)
- SiteContent model created (T-002)
- Base template created (T-003)

---

## Tasks

This story is implemented through:
- T-003: Homepage Template & Property Cards
- T-004: Property Detail Pages (for "View Details" links)
- T-006: Contact Forms (for bottom form section)

---

## Notes

- Primary photo shows first, thumbnails show next 3 photos
- If property has < 4 photos, repeat or show blank placeholders
- Price display logic: show price / "Call for Pricing" / hide based on property.price_display
- Hero header image is static (uploaded to static/images/)
- Intro section is dynamic (editable in admin via SiteContent)
