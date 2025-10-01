# T-007: Admin Configuration

**Task Type**: Implementation
**Priority**: High
**Estimate**: 2 hours
**Sprint**: Sprint 1
**Status**: 📋 PENDING
**User Story**: S-005

---

## Description

Configure Django admin for all models with custom configurations, CSV export, search/filter capabilities, and enhanced usability.

## Deliverables

- [ ] PropertyAdmin with inline photos
- [ ] PhotoInline configuration
- [ ] ContactAdmin with search/filter/export
- [ ] SiteContentAdmin with monospace textarea
- [ ] CSV export action for contacts
- [ ] Custom admin CSS for SiteContent
- [ ] List displays optimized
- [ ] Search fields configured
- [ ] Filters configured
- [ ] Fieldsets organized logically
- [ ] Help text on complex fields
- [ ] Admin site customization (title, header)
- [ ] Admin tests

## Acceptance Criteria

- All models registered in admin
- Property admin shows inline photos
- Contact admin has search and filters
- CSV export works for contacts
- SiteContent textarea is monospace
- Admin interface is user-friendly
- All tests pass

## Technical Notes

See S-005-admin-backend.md for complete admin configurations.

**Key Features:**

**Property Admin:**
- Photo inline (tabular, 3 extra)
- Fieldsets for organization
- Prepopulated slug from title
- List display with key fields
- Filters for type, status, featured

**Contact Admin:**
- List display: name, email, phone, property, buyer_type, date
- Search: name, email, phone, company, message
- Filters: property, buyer_type, timeline, financing, making_offer, date
- Date hierarchy: submitted_at
- CSV export action
- Readonly: submitted_at

**SiteContent Admin:**
- Monospace textarea (CSS)
- List display: key, last_updated
- Search: key, content
- Readonly: last_updated

**CSV Export Implementation:**
```python
def export_to_csv(self, request, queryset):
    import csv
    from django.http import HttpResponse

    response = HttpResponse(content_type='text/csv')
    response['Content-Disposition'] = 'attachment; filename="contacts.csv"'

    writer = csv.writer(response)
    writer.writerow(['Name', 'Email', 'Phone', 'Property', 'Buyer Type',
                     'Timeline', 'Financing', 'Offer', 'Offer Amount',
                     'Message', 'Date'])

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

**Custom Admin CSS:**
```css
/* static/admin/css/sitecontent.css */
.field-content textarea {
    font-family: 'Courier New', Courier, monospace;
    font-size: 14px;
    line-height: 1.5;
}
```

**Admin Site Customization:**
```python
# admin.py
admin.site.site_header = "3235 Ivanhoe Property Management"
admin.site.site_title = "3235 Ivanhoe Admin"
admin.site.index_title = "Welcome to 3235 Ivanhoe Admin"
```

---

## Testing

- [ ] Test all models appear in admin
- [ ] Test property creation with photos
- [ ] Test photo inline add/edit/delete
- [ ] Test contact search
- [ ] Test contact filters
- [ ] Test CSV export
- [ ] Test SiteContent editing
- [ ] Test admin customizations

---

## Dependencies

- Models created (T-002)
- Django admin installed (default)

---

**Closes #TBD**
