# S-005: Admin Management & Editable Content

**Story Type**: User Story
**Priority**: High
**Estimate**: 1 day
**Sprint**: Sprint 1
**Status**: 📋 PENDING

---

## User Story

**As a** property seller (admin user)
**I want to** manage properties, photos, contacts, and site content through Django admin
**So that** I can maintain the website without needing developer assistance

---

## Acceptance Criteria

- [ ] When I log into Django admin, I see all models (Property, Photo, Contact, SiteContent)
- [ ] When I create a property, I can fill in all fields with HTML textarea for description
- [ ] When I edit a property, I can manage photos inline (add/edit/delete)
- [ ] When I view contacts, I can search by name, email, property
- [ ] When I view contacts, I can filter by property, buyer type, timeline, date
- [ ] When I view contacts, I can export selected contacts to CSV
- [ ] When I edit site content, I can use HTML formatting in textarea
- [ ] When I edit site content, I see a monospace font (code editor feel)
- [ ] When I save editable content, changes appear immediately on frontend
- [ ] When I upload photos, I can set primary flag and display order

---

## Definition of Done

- [ ] Django admin registered for all models
- [ ] Property admin with custom configuration
- [ ] Photo inline on Property admin
- [ ] Contact admin with search, filters, list display
- [ ] CSV export action for contacts
- [ ] SiteContent admin with HTML textarea
- [ ] Textarea uses monospace font (CSS)
- [ ] List views show relevant columns
- [ ] Admin actions work correctly
- [ ] Read-only fields where appropriate (e.g., submitted_at)
- [ ] Help text on complex fields
- [ ] Ordering and pagination configured
- [ ] Tests written and passing (>95% coverage)
- [ ] Documentation: Admin User Guide

---

## Technical Notes

**Property Admin Configuration:**

```python
class PhotoInline(admin.TabularInline):
    model = Photo
    extra = 3
    fields = ['image', 'caption', 'is_primary', 'display_order']

@admin.register(Property)
class PropertyAdmin(admin.ModelAdmin):
    list_display = ['title', 'address', 'property_type', 'status', 'featured']
    list_filter = ['property_type', 'status', 'featured']
    search_fields = ['title', 'address', 'description']
    prepopulated_fields = {'slug': ('title',)}
    inlines = [PhotoInline]

    fieldsets = (
        ('Basic Information', {
            'fields': ('title', 'slug', 'address', 'property_type', 'status')
        }),
        ('Property Details', {
            'fields': ('square_footage', 'year_built', 'lot_size', 'zoning', 'current_use', 'description')
        }),
        ('Pricing', {
            'fields': ('price_display', 'price')
        }),
        ('Display Options', {
            'fields': ('featured', 'display_order')
        }),
    )
```

**Contact Admin Configuration:**

```python
@admin.register(Contact)
class ContactAdmin(admin.ModelAdmin):
    list_display = ['name', 'email', 'phone', 'primary_property', 'buyer_type', 'submitted_at']
    list_filter = ['primary_property', 'buyer_type', 'timeline', 'financing_status', 'making_offer', 'submitted_at']
    search_fields = ['name', 'email', 'phone', 'company', 'message']
    readonly_fields = ['submitted_at']
    date_hierarchy = 'submitted_at'
    actions = ['export_to_csv']

    fieldsets = (
        ('Property Interest', {
            'fields': ('primary_property', 'also_interested_in', 'interested_in_both')
        }),
        ('Contact Information', {
            'fields': ('name', 'email', 'phone', 'company')
        }),
        ('Buyer Details', {
            'fields': ('buyer_type', 'timeline', 'financing_status')
        }),
        ('Offer Information', {
            'fields': ('making_offer', 'offer_amount')
        }),
        ('Additional', {
            'fields': ('message', 'submitted_at')
        }),
    )

    def export_to_csv(self, request, queryset):
        # CSV export implementation
        pass
```

**SiteContent Admin Configuration:**

```python
@admin.register(SiteContent)
class SiteContentAdmin(admin.ModelAdmin):
    list_display = ['key', 'last_updated']
    search_fields = ['key', 'content']
    readonly_fields = ['last_updated']

    # Custom CSS for monospace textarea
    class Media:
        css = {
            'all': ('admin/css/sitecontent.css',)
        }
```

**CSS for SiteContent textarea:**
```css
/* static/admin/css/sitecontent.css */
.field-content textarea {
    font-family: 'Courier New', Courier, monospace;
    font-size: 14px;
    line-height: 1.5;
}
```

**CSV Export Action:**

```python
def export_to_csv(self, request, queryset):
    import csv
    from django.http import HttpResponse

    response = HttpResponse(content_type='text/csv')
    response['Content-Disposition'] = 'attachment; filename="contacts.csv"'

    writer = csv.writer(response)
    writer.writerow(['Name', 'Email', 'Phone', 'Property', 'Buyer Type', 'Timeline',
                     'Financing', 'Offer', 'Offer Amount', 'Message', 'Date'])

    for contact in queryset:
        writer.writerow([
            contact.name,
            contact.email,
            contact.phone,
            contact.primary_property.title if contact.primary_property else '',
            contact.get_buyer_type_display(),
            contact.get_timeline_display(),
            contact.get_financing_status_display(),
            'Yes' if contact.making_offer else 'No',
            contact.offer_amount if contact.offer_amount else '',
            contact.message,
            contact.submitted_at.strftime('%Y-%m-%d %H:%M')
        ])

    return response

export_to_csv.short_description = "Export selected to CSV"
```

**SiteContent Keys:**

Pre-populate these keys:
1. `homepage_intro` - Intro text on homepage
2. `footer_text` - Additional footer text (optional)
3. `about_text` - About section (future use)

**Editable Content Workflow:**

1. Admin navigates to Site Content
2. Selects key (e.g., homepage_intro)
3. Edits content using HTML:
   ```html
   <p>Welcome to <strong>3235 Ivanhoe</strong>!</p>
   <p>Two exceptional properties available...</p>
   ```
4. Saves
5. Frontend renders HTML (using |safe filter with caution)

**Security for Editable Content:**
- Only admin users can edit
- Sanitize HTML (use bleach library)
- Whitelist safe tags: p, strong, em, h2, h3, ul, li, a
- Strip dangerous tags: script, style, iframe

**Admin Dashboard Customization:**

- Custom admin site title: "3235 Ivanhoe Admin"
- Custom admin site header: "3235 Ivanhoe Property Management"
- Group models logically
- Add admin index dashboard (optional)

---

## Dependencies

- All models created (T-002)
- Django admin installed (default)
- CSV module (Python stdlib)
- bleach library for HTML sanitization

---

## Tasks

This story is implemented through:
- T-007: Admin Configuration
- T-002: SiteContent Model Creation

---

## Notes

- Superuser creation: `python manage.py createsuperuser`
- Admin URL: `/admin/`
- CSV export includes all fields
- HTML sanitization prevents XSS attacks
- Consider rich text editor (CKEditor/TinyMCE) in v2.0
- Admin permissions can be granular (future: staff users with limited access)
- Photo thumbnails in admin for easy identification
