# T-006: Legal Pages (Privacy Policy & Terms of Use)

**Task Type**: Implementation
**Priority**: Medium
**Estimate**: 2 hours
**Sprint**: Sprint 1
**Status**: 📋 PENDING
**User Story**: Foundation

---

## Description

Create Privacy Policy and Terms of Use pages with legally appropriate content, disclaimers, and non-binding offer language.

## Deliverables

- [ ] privacy.html template
- [ ] terms.html template
- [ ] Privacy Policy content
- [ ] Terms of Use content
- [ ] View functions for legal pages
- [ ] URL routing for legal pages
- [ ] Footer links to legal pages
- [ ] Simple static page styling
- [ ] Template tests

## Acceptance Criteria

- Privacy page loads at /privacy/
- Terms page loads at /terms/
- Content is clear and appropriate
- Offer disclaimers prominently displayed
- Footer links work on all pages
- Responsive design matches site
- All tests pass

## Technical Notes

**Privacy Policy Content:**
```
Privacy Policy

Last Updated: [Date]

At 3235 Ivanhoe, we collect information you voluntarily provide through our
contact forms. This may include your name, email address, phone number,
company information, and any messages you send.

Data Collection and Use:
- We use your information solely to respond to your property inquiries
- Your information is stored securely in our database
- We do not sell or share your information with third parties
- You have no expectation of privacy for information submitted online

Information Storage:
- Contact information is retained indefinitely for business purposes
- You may request removal by contacting us

Contact Us:
If you have questions about this policy, please use our contact form.

Disclaimer:
By using this website, you acknowledge that electronic communications and
data storage carry inherent risks. We make no guarantees regarding data
security or privacy.
```

**Terms of Use Content:**
```
Terms of Use

Last Updated: [Date]

By accessing and using 3235ivanhoe.com, you agree to these terms.

No Rights Granted:
Use of this website does not grant you any rights, licenses, or interests
in any property, content, or information displayed.

Property Listings:
All property information is provided "as-is" without warranties. Property
availability, pricing, and details may change without notice.

Offer Submissions:
IMPORTANT: Any offers or expressions of interest submitted through this
website are SPECULATIVE ONLY and are NOT BINDING. Submission of an offer
amount or interest does NOT constitute an offer to purchase or a binding
agreement. All property transactions require formal written purchase
agreements executed by authorized parties.

No acceptance of any offer is created or implied by your submission. The
seller reserves the right to reject any and all offers, inquiries, or
expressions of interest without explanation.

Contact Form:
Information submitted through contact forms is for communication purposes
only. Submission does not create any business relationship, agency, or
contractual obligation.

Limitation of Liability:
To the fullest extent permitted by law, 3235 Ivanhoe and its owners,
agents, and representatives shall not be liable for any direct, indirect,
incidental, or consequential damages arising from use of this website.

Changes to Terms:
We reserve the right to modify these terms at any time. Continued use
constitutes acceptance of modified terms.

Governing Law:
These terms are governed by the laws of [State/Jurisdiction].

Contact:
For questions regarding these terms, use our contact form.
```

**View Functions:**
```python
def privacy_policy(request):
    return render(request, 'property/privacy.html')

def terms_of_use(request):
    return render(request, 'property/terms.html')
```

**URL Patterns:**
```python
urlpatterns = [
    path('privacy/', views.privacy_policy, name='privacy'),
    path('terms/', views.terms_of_use, name='terms'),
]
```

---

## Testing

- [ ] Test privacy page renders
- [ ] Test terms page renders
- [ ] Test footer links navigate correctly
- [ ] Test responsive layout

---

## Dependencies

- Base template (T-003)
- Footer implementation (T-003)

---

**Closes #TBD**
