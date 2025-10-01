# T-003: Homepage Template & Property Cards

**Task Type**: Implementation
**Priority**: High
**Estimate**: 3 hours
**Sprint**: Sprint 1
**Status**: 📋 PENDING
**User Story**: S-001

---

## Description

Create the homepage template with hero header, editable intro section, stacked property cards with photos, and footer.

## Deliverables

- [ ] base.html template with shared layout
- [ ] home.html template extending base
- [ ] Hero header section with static image
- [ ] Editable intro section (SiteContent)
- [ ] Property card component with large photo + 3 thumbnails
- [ ] Thumbnail click JavaScript for photo swap
- [ ] Footer with copyright and legal links
- [ ] CSS for responsive layout
- [ ] Mobile-friendly design
- [ ] Homepage view function
- [ ] URL routing for homepage
- [ ] Template tests

## Acceptance Criteria

- Homepage loads successfully at /
- Hero header displays correctly
- Intro text renders from SiteContent
- Property cards display for all properties
- Thumbnails clickable and change main photo
- Responsive layout works on mobile/tablet/desktop
- Footer appears with correct links
- All tests pass

## Technical Notes

See S-001-homepage-design.md for detailed specifications.

**Key Features:**
- Hero header: 400-600px desktop, 200-300px mobile
- Property cards stacked vertically
- Large primary photo + 3 thumbnails
- JavaScript photo swap with fade effect
- Price display logic (show/call/hide)
- "View Details" button to property page

**JavaScript:**
```javascript
// Thumbnail click handler
document.querySelectorAll('.property-thumbnail').forEach(thumb => {
    thumb.addEventListener('click', function() {
        const mainImg = this.closest('.property-card').querySelector('.property-main-img');
        mainImg.src = this.dataset.fullUrl;
        // Add active class to clicked thumbnail
        this.closest('.thumbnails').querySelectorAll('.property-thumbnail').forEach(t => t.classList.remove('active'));
        this.classList.add('active');
    });
});
```

---

## Testing

- [ ] Test homepage view renders
- [ ] Test property cards display
- [ ] Test SiteContent integration
- [ ] Test with 0, 1, 2+ properties
- [ ] Test with properties with < 4 photos
- [ ] Test responsive breakpoints

---

## Dependencies

- Models created (T-002)
- Static files configured (T-001)
- SiteContent populated with homepage_intro

---

**Closes #TBD**
