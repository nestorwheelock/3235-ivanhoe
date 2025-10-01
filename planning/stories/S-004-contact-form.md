# S-004: Contact Form System

**Story Type**: User Story
**Priority**: High
**Estimate**: 1 day
**Sprint**: Sprint 1
**Status**: 📋 PENDING

---

## User Story

**As a** potential property buyer
**I want to** submit my contact information and interest details
**So that** the seller can reach out to me about properties I'm interested in

---

## Acceptance Criteria

- [ ] When I'm on the homepage, I can select which property I'm interested in (3233 / 3235 / Both)
- [ ] When I'm on a property detail page, that property is auto-selected
- [ ] When I'm on a property detail page, I can check "Also interested in [other property]"
- [ ] When I fill out the form, all required fields are validated
- [ ] When I submit the form, I see a confirmation message
- [ ] When I submit the form, my information is saved to the database
- [ ] When I make an offer, the offer amount field appears
- [ ] When I don't make an offer, the offer amount field is hidden
- [ ] When I submit invalid data, I see helpful error messages
- [ ] When form submission fails, my data is preserved (not lost)

---

## Definition of Done

- [ ] Contact model created with all fields
- [ ] ContactForm created with validation
- [ ] Form appears on homepage (manual property selection)
- [ ] Form appears on property detail pages (auto-selection)
- [ ] Property selection logic works correctly
- [ ] "Also interested in" checkbox on property pages
- [ ] "Making offer" checkbox with conditional offer amount field
- [ ] Client-side validation (JavaScript)
- [ ] Server-side validation (Django form clean methods)
- [ ] CSRF protection enabled
- [ ] Success message after submission
- [ ] Thank you page or message display
- [ ] Form data saves to Contact model
- [ ] Email format validation
- [ ] Phone number format validation (lenient)
- [ ] Tests written and passing (>95% coverage)
- [ ] Documentation updated

---

## Technical Notes

**Contact Model Fields:**
```python
class Contact(models.Model):
    # Property Interest
    primary_property = ForeignKey(Property, on_delete=SET_NULL, null=True, related_name='primary_contacts')
    also_interested_in = ForeignKey(Property, on_delete=SET_NULL, null=True, blank=True, related_name='secondary_contacts')
    interested_in_both = BooleanField(default=False)

    # Contact Information (Required)
    name = CharField(max_length=200)
    email = EmailField()
    phone = CharField(max_length=20)

    # Buyer Details (Optional)
    company = CharField(max_length=200, blank=True)
    buyer_type = CharField(choices=[...], blank=True)
    timeline = CharField(choices=[...], blank=True)
    financing_status = CharField(choices=[...], blank=True)

    # Offer Information
    making_offer = BooleanField(default=False)
    offer_amount = DecimalField(max_digits=12, decimal_places=2, null=True, blank=True)

    # Additional
    message = TextField(blank=True)
    submitted_at = DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['-submitted_at']
```

**Form Logic:**

1. **Homepage Form:**
   - Property selection dropdown (required):
     - "Select property..." (blank option)
     - "3233 Ivanhoe - Commercial"
     - "3235 Ivanhoe - Live-Work"
     - "Both Properties"
   - If "Both" selected: set interested_in_both=True
   - All other fields shown

2. **Property Detail Page Form:**
   - Hidden field: primary_property (auto-set to current property)
   - Checkbox: "Also interested in [other property name]"
   - If checked: set also_interested_in to other property
   - All other fields shown

3. **Offer Amount Logic:**
   - Checkbox: "Would you like to make an offer?"
   - If checked: show offer amount field (number input)
   - If unchecked: hide offer amount field
   - JavaScript toggles visibility

**Form Fields:**

**Required:**
- Name (max 200 chars)
- Email (valid email format)
- Phone (max 20 chars, lenient validation)

**Optional Dropdowns:**
- Company (text input)
- Buyer Type: Investor | Owner-Occupant | Developer | Other
- Timeline: Immediate | 3-6 Months | 6-12 Months | Exploring
- Financing Status: Cash | Financing Pre-approved | Seeking Financing | Unsure

**Optional:**
- Making Offer (checkbox)
- Offer Amount (dollar input, visible if making_offer=True)
- Message (textarea, max 1000 chars)

**Validation Rules:**

1. **Name:** Required, 2-200 chars
2. **Email:** Required, valid format
3. **Phone:** Required, 10-20 chars (allow various formats)
4. **Offer Amount:** Required if making_offer=True, must be > 0
5. **Property Selection:** Required on homepage
6. **Message:** Max 1000 chars

**Form Rendering:**
- Use Django forms (not raw HTML)
- Include {{ form.as_p }} or custom rendering
- Add Bootstrap/CSS classes for styling
- JavaScript for conditional fields

**Success Handling:**
- Option A: Redirect to thank-you page (/thank-you/)
- Option B: Show success message on same page
- Option C: AJAX submission with modal
- Recommendation: Option A (simple, clear)

**Error Handling:**
- Display form errors above form
- Highlight fields with errors
- Preserve submitted data
- Clear error messages

**Security:**
- CSRF token in all forms
- SQL injection prevention (Django ORM handles)
- XSS prevention (template escaping)
- Rate limiting (future enhancement)

---

## Dependencies

- Contact model created (T-002)
- Property model created (T-002)
- Homepage template created (S-001)
- Property detail template created (S-002)
- Thank you page template (T-005)

---

## Tasks

This story is implemented through:
- T-003: Contact Model & Form
- T-005: Contact Forms on Homepage
- T-005: Contact Forms on Property Pages
- T-005: Thank You Page

---

## Notes

- No email notifications in v1.0 (check admin manually)
- Consider CAPTCHA for spam prevention (v2.0)
- Export contacts to CSV from admin
- Phone format flexible: (123) 456-7890, 123-456-7890, 1234567890 all accepted
- Offer amount stored as Decimal for precision
- Property selection on homepage: use dropdown, not radio (cleaner)
- "Also interested" only shows if there's another property to be interested in
