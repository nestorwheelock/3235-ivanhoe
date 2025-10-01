# T-002: Data Models (Property/Photo/Contact/SiteContent)

**Task Type**: Implementation
**Priority**: High
**Estimate**: 3 hours
**Sprint**: Sprint 1
**Status**: 📋 PENDING
**User Story**: S-001, S-002, S-003, S-004, S-005

---

## Description

Create all Django models for Property, Photo, Contact, and SiteContent with proper relationships, fields, and validation.

## Deliverables

- [ ] Property model with all fields
- [ ] Photo model with ForeignKey to Property
- [ ] Contact model with property relationships
- [ ] SiteContent model for editable sections
- [ ] Model Meta classes (ordering, verbose names)
- [ ] Model __str__ methods
- [ ] Model save() overrides where needed
- [ ] Migrations generated and applied
- [ ] Model tests (creation, validation, relationships)
- [ ] Test coverage >95%

## Acceptance Criteria

- All models create successfully
- Relationships work correctly (ForeignKey, related_name)
- Migrations apply without errors
- Models appear in Django admin
- Tests pass for all models
- Field validations work correctly

## Technical Notes

All model implementations are detailed in PROJECT_CHARTER.md. Key points:

**Property Model:**
- Slug field for SEO-friendly URLs
- Price display choices (show/call/hide)
- Featured and display_order fields
- Optional fields with blank=True

**Photo Model:**
- ForeignKey to Property with CASCADE
- ImageField with upload_to path
- is_primary enforcement (only one per property)
- display_order for carousel ordering
- Default ordering: ['display_order', 'uploaded_at']

**Contact Model:**
- primary_property and also_interested_in ForeignKeys
- interested_in_both boolean flag
- Choice fields for buyer_type, timeline, financing_status
- offer_amount conditional on making_offer
- Default ordering: ['-submitted_at']

**SiteContent Model:**
- Unique key field
- TextField for HTML content
- Auto-updating last_updated timestamp

**Key Methods to Implement:**
```python
# Property
def get_absolute_url(self):
    return reverse('property:detail', args=[self.slug])

def get_primary_photo(self):
    return self.photos.filter(is_primary=True).first() or self.photos.first()

# Photo
def save(self, *args, **kwargs):
    # If setting as primary, unset others
    if self.is_primary:
        Photo.objects.filter(property=self.property, is_primary=True).update(is_primary=False)
    super().save(*args, **kwargs)
```

---

## Testing

- [ ] Test Property creation with all fields
- [ ] Test Property.get_primary_photo()
- [ ] Test Photo upload and storage
- [ ] Test Photo primary flag enforcement
- [ ] Test Contact creation with relationships
- [ ] Test SiteContent key uniqueness
- [ ] Test model __str__ methods
- [ ] Test CASCADE deletion behavior

---

## Dependencies

- Django project setup (T-001)
- Pillow installed for ImageField

---

**Closes #TBD** (GitHub issue to be created)
