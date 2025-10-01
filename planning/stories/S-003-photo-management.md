# S-003: Photo Management & Carousel

**Story Type**: User Story
**Priority**: High
**Estimate**: 1 day
**Sprint**: Sprint 1
**Status**: 📋 PENDING

---

## User Story

**As a** property seller (admin user)
**I want to** upload, organize, and manage property photos
**So that** buyers can view high-quality images of my properties

---

## Acceptance Criteria

- [ ] When I log into Django admin, I can upload photos for each property
- [ ] When I upload photos, I can upload multiple images at once
- [ ] When I manage photos, I can designate one as the primary photo
- [ ] When I manage photos, I can set the display order for carousel
- [ ] When I manage photos, I can add optional captions
- [ ] When I save photos, they are associated with the correct property
- [ ] When I view properties on frontend, photos display in the correct order
- [ ] When no primary photo is set, the first photo (by order) is used
- [ ] When I delete a photo, it's removed from storage and database

---

## Definition of Done

- [ ] Photo model created with all fields
- [ ] Django admin configured for photo management
- [ ] Multiple file upload capability in admin
- [ ] Primary photo designation (radio select or checkbox with validation)
- [ ] Display order field (integer, sortable in admin)
- [ ] Caption field (optional text)
- [ ] Photo-Property relationship (ForeignKey)
- [ ] Image upload to media directory
- [ ] Image file deletion when model deleted (signal)
- [ ] Inline photo editing on Property admin page
- [ ] Frontend displays photos in order
- [ ] Primary photo logic in views/templates
- [ ] Tests written and passing (>95% coverage)
- [ ] Documentation updated

---

## Technical Notes

**Photo Model Fields:**
```python
class Photo(models.Model):
    property = ForeignKey(Property, on_delete=CASCADE, related_name='photos')
    image = ImageField(upload_to='property_photos/%Y/%m/')
    caption = CharField(max_length=255, blank=True)
    is_primary = BooleanField(default=False)
    display_order = IntegerField(default=0)
    uploaded_at = DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['display_order', 'uploaded_at']
```

**Admin Configuration:**

1. **Inline Photo Admin:**
   - TabularInline or StackedInline on Property admin
   - Show: image thumbnail, caption, is_primary, display_order
   - Allow: add, edit, delete

2. **Multiple Upload:**
   - Option A: Use django-admin-multiple-file-upload
   - Option B: Custom admin action
   - Option C: Accept multiple in single field (formfield_for_dbfield)
   - Recommendation: Option C or manual multiple adds

3. **Primary Photo Logic:**
   - Only one photo per property can be primary
   - Admin validation or save() override to enforce
   - If primary set, unset others for same property

4. **Display Order:**
   - Admin list_editable for easy reordering
   - Or drag-and-drop (requires admin plugin)
   - Default to 0, then incremental

**Frontend Photo Usage:**

1. **Homepage Cards:**
   - Primary photo as large image
   - Next 3 photos as thumbnails
   - Query: `property.photos.filter(is_primary=True).first()` or `property.photos.first()`

2. **Property Detail:**
   - Primary photo as hero header
   - All photos in carousel (ordered)
   - Query: `property.photos.all()`

3. **Lightbox:**
   - All photos available for navigation
   - Preserve order from display_order

**Image Processing:**
- Use Pillow for thumbnails (optional optimization)
- Consider image size limits (max 5MB per photo)
- Store originals, generate thumbnails on upload
- Formats: JPEG, PNG, WebP

**Storage:**
- Media root: `MEDIA_ROOT = BASE_DIR / 'media'`
- Upload path: `media/property_photos/YYYY/MM/filename.jpg`
- Serve in development: `MEDIA_URL = '/media/'`
- Serve in production: Nginx static file serving

**Performance Considerations:**
- Lazy load images on homepage (load visible first)
- Optimize image sizes (resize on upload)
- Use WebP format if browser supports
- CDN for production (future enhancement)

---

## Dependencies

- Property model created (T-002)
- Django admin setup (T-001)
- Media configuration in settings (T-001)
- Pillow installed (requirements.txt)

---

## Tasks

This story is implemented through:
- T-002: Photo Model Implementation
- T-007: Admin Configuration (Photo inline)

---

## Notes

- Unlimited photos per property (no artificial limit)
- Primary photo defaults to first if none designated
- Photos cascade delete when property deleted
- Consider watermarking (future enhancement)
- Consider EXIF data extraction (future enhancement)
- Alt text for accessibility (use caption or auto-generate)
