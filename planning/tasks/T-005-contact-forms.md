# T-005: Contact Forms (Both Locations)

**Task Type**: Implementation
**Priority**: High
**Estimate**: 3 hours
**Sprint**: Sprint 1
**Status**: 📋 PENDING
**User Story**: S-004

---

## Description

Create contact form with comprehensive fields, validation, and logic for both homepage (manual selection) and property pages (auto-selection).

## Deliverables

- [ ] ContactForm class with all fields
- [ ] Form validation (client & server-side)
- [ ] Homepage form with property dropdown
- [ ] Property page form with auto-select + checkbox
- [ ] "Making offer" conditional field logic (JavaScript)
- [ ] Form submission view
- [ ] Thank you page template
- [ ] CSRF protection
- [ ] Success/error message handling
- [ ] Form preservation on errors
- [ ] Email/phone validation
- [ ] Form tests

## Acceptance Criteria

- Form displays on homepage with dropdown
- Form displays on property pages with auto-select
- "Also interested in" checkbox works on property pages
- Offer amount field shows/hides based on checkbox
- Required fields validated
- Email format validated
- Form submits and saves to database
- Thank you page displays after submission
- Errors display with helpful messages
- Form data preserved on validation errors
- All tests pass

## Technical Notes

See S-004-contact-form.md for detailed specifications.

**Form Fields:**
- Required: name, email, phone
- Property selection (homepage) or auto-select (property page)
- Optional: company, buyer_type, timeline, financing_status
- Conditional: offer_amount (if making_offer=True)
- Optional: message (textarea)

**Homepage Form Logic:**
```python
# forms.py
class ContactForm(forms.ModelForm):
    class Meta:
        model = Contact
        fields = ['primary_property', 'name', 'email', 'phone', 'company',
                  'buyer_type', 'timeline', 'financing_status',
                  'making_offer', 'offer_amount', 'message']

    def clean(self):
        # Validate offer_amount required if making_offer
        if self.cleaned_data.get('making_offer') and not self.cleaned_data.get('offer_amount'):
            raise ValidationError("Offer amount is required when making an offer")
```

**Property Page Form:**
- Hidden field: primary_property (auto-set)
- Visible checkbox: also_interested_in (if other property exists)

**JavaScript for Conditional Fields:**
```javascript
// Show/hide offer amount based on checkbox
document.querySelector('#id_making_offer').addEventListener('change', function() {
    const offerField = document.querySelector('#offer-amount-field');
    offerField.style.display = this.checked ? 'block' : 'none';
});
```

**Thank You Page:**
- Simple confirmation message
- "Thank you for your interest!"
- Link back to homepage
- No contact details displayed (form only)

---

## Testing

- [ ] Test form validation (required fields)
- [ ] Test email format validation
- [ ] Test offer amount conditional validation
- [ ] Test form submission success
- [ ] Test form error handling
- [ ] Test homepage form property selection
- [ ] Test property page auto-selection
- [ ] Test "also interested" checkbox
- [ ] Test CSRF protection

---

## Dependencies

- Models created (T-002)
- Homepage template (T-003)
- Property detail template (T-004)

---

**Closes #TBD**
