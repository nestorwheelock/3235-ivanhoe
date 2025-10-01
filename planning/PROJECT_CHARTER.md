# PROJECT CHARTER - 3235ivanhoe.com v1.0.0

**Project Name:** 3235-ivanhoe
**Version:** 1.0.0
**Project Type:** Property Listing Platform
**Domain:** 3235ivanhoe.com
**Start Date:** 2025-10-01
**Target Completion:** 2025-10-02

---

## WHAT - Project Description

**3235ivanhoe.com** is a property listing platform for marketing and capturing buyer interest in two Ivanhoe properties: a commercial building at 3233 and a live/work building at 3235. The platform provides property information with photo galleries, collects buyer contact information through forms, and offers admin tools for property and contact management.

### Core Functionality

- **Property Display:** Homepage with stacked property cards, individual detail pages
- **Photo Galleries:** Unlimited photos per property with carousel and lightbox viewing
- **Contact Forms:** Capture buyer interest on homepage and property pages
- **Editable Content:** Admin-managed intro text and site content using HTML
- **Contact Management:** Django admin backend for viewing and exporting leads
- **Legal Pages:** Privacy Policy and Terms of Use with offer disclaimers
- **Docker Deployment:** Containerized deployment to Vultr with Let's Encrypt SSL

### Key Features

1. **Homepage Design:**
   - Hero header image (both properties)
   - Editable intro section (HTML formatting)
   - Stacked property cards with large photo + 3 thumbnails
   - Contact form at bottom

2. **Property Detail Pages:**
   - Individual hero header (property-specific)
   - Full photo carousel with lightbox
   - Complete property details
   - Contact form with auto-selection

3. **Contact Form System:**
   - Homepage: Select property (3233 / 3235 / Both)
   - Property page: Auto-select + "Also interested in" checkbox
   - Comprehensive fields: Name, Email, Phone, Company, Buyer Type, Timeline, Financing, Offer Amount, Message

4. **Photo Management:**
   - Unlimited photos per property
   - Primary photo designation
   - Display order control
   - Multiple upload capability
   - Carousel with thumbnail navigation
   - Lightbox for fullscreen viewing

5. **Admin Backend:**
   - Django admin for all management
   - Property creation and editing
   - Photo upload and organization
   - Contact viewing and CSV export
   - Editable site content (HTML textarea)

6. **Legal Compliance:**
   - Privacy Policy page
   - Terms of Use page
   - Offer disclaimer language
   - No acceptance of submissions

---

## WHY - Business Need

### Problem Statement

Selling commercial properties requires:
- Professional online presence to showcase properties
- Qualified lead capture with detailed buyer information
- Efficient contact management and follow-up
- Clear legal disclaimers for speculative offers

### Solution

A custom Django property platform that provides:
- Beautiful property presentation with unlimited photos
- Comprehensive buyer qualification through detailed forms
- Centralized contact database with export capability
- Built-in legal protections

### Value Proposition

- **Professional Presentation:** High-quality photo galleries and property details
- **Lead Qualification:** Capture buyer type, timeline, financing status, and offer amounts
- **Efficiency:** All contacts in one database, easy export to CSV
- **Legal Protection:** Clear disclaimers that offers are speculative, not binding
- **Flexibility:** Multi-property platform, easy to add more properties
- **Control:** Self-hosted, no third-party dependencies

### Use Cases

1. **Property Marketing:** Showcase properties to potential buyers
2. **Lead Capture:** Collect qualified buyer information
3. **Lead Management:** View, search, filter, and export contacts
4. **Communication:** Reach out to interested parties when listed
5. **Legal Compliance:** Display required privacy and terms information

---

## HOW - Technical Approach

### Architecture

**Django Application Structure:**
```
3235-ivanhoe/
├── property/                    # Main Django app
│   ├── models.py               # Property, Photo, Contact, SiteContent
│   ├── views.py                # Homepage, detail, contact, legal pages
│   ├── forms.py                # Contact form with validation
│   ├── admin.py                # Admin configuration
│   ├── urls.py                 # URL routing
│   ├── templates/
│   │   └── property/
│   │       ├── base.html
│   │       ├── home.html
│   │       ├── property_detail.html
│   │       ├── privacy.html
│   │       └── terms.html
│   ├── static/
│   │   └── property/
│   │       ├── css/
│   │       │   └── style.css
│   │       ├── js/
│   │       │   └── carousel.js
│   │       └── images/
│   └── tests/
│       ├── test_models.py
│       ├── test_views.py
│       ├── test_forms.py
│       └── test_admin.py
├── ivanhoe/                    # Django project settings
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── docker/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   └── nginx/
│       └── nginx.conf
├── manage.py
├── requirements.txt
└── README.md
```

### Data Models

**Property Model:**
```python
class Property(models.Model):
    title = CharField(max_length=200)
    address = CharField(max_length=255)
    property_type = CharField(choices=[
        ('commercial', 'Commercial'),
        ('residential', 'Residential'),
        ('live_work', 'Live-Work'),
        ('mixed_use', 'Mixed Use')
    ])
    status = CharField(choices=[
        ('coming_soon', 'Coming Soon'),
        ('available', 'Available'),
        ('under_contract', 'Under Contract'),
        ('sold', 'Sold')
    ])

    # Property Details
    square_footage = IntegerField(null=True, blank=True)
    year_built = IntegerField(null=True, blank=True)
    lot_size = CharField(max_length=100, blank=True)
    zoning = CharField(max_length=100, blank=True)
    current_use = CharField(max_length=255, blank=True)
    description = TextField(blank=True)

    # Pricing
    price_display = CharField(choices=[
        ('show', 'Show Price'),
        ('call', 'Call for Pricing'),
        ('hide', 'Hide Price')
    ])
    price = DecimalField(max_digits=12, decimal_places=2, null=True, blank=True)

    # Metadata
    featured = BooleanField(default=False)
    display_order = IntegerField(default=0)
    created_at = DateTimeField(auto_now_add=True)
    updated_at = DateTimeField(auto_now=True)
```

**Photo Model:**
```python
class Photo(models.Model):
    property = ForeignKey(Property, on_delete=CASCADE, related_name='photos')
    image = ImageField(upload_to='property_photos/')
    caption = CharField(max_length=255, blank=True)
    is_primary = BooleanField(default=False)
    display_order = IntegerField(default=0)
    uploaded_at = DateTimeField(auto_now_add=True)
```

**Contact Model:**
```python
class Contact(models.Model):
    # Property Interest
    primary_property = ForeignKey(Property, on_delete=SET_NULL, null=True, related_name='primary_contacts')
    also_interested_in = ForeignKey(Property, on_delete=SET_NULL, null=True, blank=True, related_name='secondary_contacts')
    interested_in_both = BooleanField(default=False)

    # Contact Information
    name = CharField(max_length=200)
    email = EmailField()
    phone = CharField(max_length=20)
    company = CharField(max_length=200, blank=True)

    # Buyer Details
    buyer_type = CharField(choices=[
        ('investor', 'Investor'),
        ('owner_occupant', 'Owner-Occupant'),
        ('developer', 'Developer'),
        ('other', 'Other')
    ], blank=True)
    timeline = CharField(choices=[
        ('immediate', 'Immediate'),
        ('3_6_months', '3-6 Months'),
        ('6_12_months', '6-12 Months'),
        ('exploring', 'Exploring')
    ], blank=True)
    financing_status = CharField(choices=[
        ('cash', 'Cash'),
        ('pre_approved', 'Financing Pre-approved'),
        ('seeking', 'Seeking Financing'),
        ('unsure', 'Unsure')
    ], blank=True)

    # Offer Information
    making_offer = BooleanField(default=False)
    offer_amount = DecimalField(max_digits=12, decimal_places=2, null=True, blank=True)

    # Additional
    message = TextField(blank=True)
    submitted_at = DateTimeField(auto_now_add=True)
```

**SiteContent Model:**
```python
class SiteContent(models.Model):
    key = CharField(max_length=100, unique=True)
    content = TextField()
    last_updated = DateTimeField(auto_now=True)

    # Keys: 'homepage_intro', 'footer_text', etc.
```

### Frontend Pages

**1. Homepage (home.html)**
- Hero header image (both properties)
- Editable intro section (from SiteContent)
- Stacked property cards:
  - Large primary photo
  - 3 thumbnail previews (clickable)
  - Property info (address, type, status, price)
  - "View Details" button
- Contact form (select property)
- Footer with legal links

**2. Property Detail (property_detail.html)**
- Hero header image (individual property)
- Full photo carousel:
  - Main large image
  - Thumbnail strip below
  - Click for lightbox/fullscreen
  - Navigation arrows
- Complete property details
- Contact form (auto-selected property + "also interested" checkbox)
- Footer with legal links

**3. Privacy Policy (privacy.html)**
- Standard privacy policy language
- "No expectation of privacy online" disclaimer
- Data collection and use explanation

**4. Terms of Use (terms.html)**
- No rights granted to users
- "All offers are speculative only"
- "Submissions are not binding offers"
- "No acceptance constituted by submission"
- Seller reserves all rights
- Liability disclaimers

### Photo Carousel Features

**Carousel Functionality:**
- Display all property photos in order
- Primary photo shows first
- Thumbnail navigation strip
- Click thumbnail to change main image
- Click main image for lightbox
- Lightbox with left/right arrows
- Close button (X) and ESC key support
- Responsive design (mobile-friendly)

**Technology:**
- Vanilla JavaScript (no dependencies)
- CSS Grid/Flexbox for layout
- Touch-friendly on mobile
- Lazy loading for performance

### Contact Form Logic

**Homepage Form:**
- Property selection dropdown (required): 3233 / 3235 / Both
- Standard contact fields
- No auto-selection

**Property Detail Page Form:**
- Auto-select current property (hidden field)
- Checkbox: "Also interested in [other property]"
- Same contact fields
- Pre-filled property context

### Technology Stack

**Backend:**
- Django 4.2+
- Python 3.8+
- SQLite database (simple, sufficient)
- Pillow (image processing)

**Frontend:**
- Vanilla JavaScript (carousel, lightbox, form validation)
- CSS Grid/Flexbox (responsive layout)
- No frontend framework needed

**Deployment:**
- Docker + Docker Compose
- Nginx (reverse proxy, SSL termination)
- Let's Encrypt (free SSL certificates)
- Gunicorn (WSGI server)
- Vultr VPS (Linux host)

**Development:**
- pytest + pytest-django (testing)
- pytest-cov (coverage >95%)
- black (code formatting)
- flake8 (linting)

---

## SUCCESS CRITERIA

### Functional Requirements

- [ ] **Homepage Display:** Shows both properties with photos and info
- [ ] **Property Cards:** Large photo + 3 thumbnails, clickable
- [ ] **Property Detail Pages:** Individual pages with full carousels
- [ ] **Photo Carousel:** All photos navigable with lightbox
- [ ] **Contact Forms:** Work on both homepage and property pages
- [ ] **Property Selection:** Auto-select on property pages, manual on homepage
- [ ] **Editable Content:** Admin can edit intro text with HTML
- [ ] **Contact Management:** Admin can view, search, filter, export contacts
- [ ] **Photo Upload:** Admin can upload multiple photos, set primary, order
- [ ] **Legal Pages:** Privacy Policy and Terms of Use accessible
- [ ] **Footer:** Copyright and legal links on all pages

### Quality Requirements

- [ ] **Test Coverage:** >95% code coverage
- [ ] **All Tests Pass:** 100% pass rate
- [ ] **Responsive Design:** Works on mobile, tablet, desktop
- [ ] **Browser Compatibility:** Chrome, Firefox, Safari, Edge
- [ ] **Performance:** Page load < 2 seconds, image optimization
- [ ] **Security:** CSRF protection, SQL injection prevention, XSS protection
- [ ] **SSL:** HTTPS enabled with valid certificate

### Usability Requirements

- [ ] **Intuitive Navigation:** Clear path from home to detail to contact
- [ ] **Mobile-Friendly:** Touch-friendly carousel and forms
- [ ] **Fast Load Times:** Optimized images, minimal JavaScript
- [ ] **Clear CTAs:** Obvious "View Details" and "Submit" buttons
- [ ] **Form Validation:** Client and server-side validation with helpful errors

### Deployment Requirements

- [ ] **Docker Deployment:** Runs in containers on Vultr
- [ ] **Domain Configuration:** 3235ivanhoe.com points to server
- [ ] **SSL Certificate:** Let's Encrypt auto-renewal configured
- [ ] **Database Backup:** SQLite file backed up regularly
- [ ] **Media Storage:** Uploaded photos persistent in volume

### Acceptance Criteria

**The project is successful when:**
1. Homepage displays both properties with working carousels
2. Property detail pages show all photos in fullscreen-capable carousel
3. Contact forms submit and save to database
4. Admin can create properties, upload photos, view contacts
5. Legal pages are accessible and contain required disclaimers
6. Site is live at 3235ivanhoe.com with valid SSL
7. All tests pass with >95% coverage
8. Client approves for production use

---

## RISKS

### Technical Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Photo upload/storage issues | Low | Medium | Use Django's ImageField, test with large files, implement size limits |
| Carousel performance with many photos | Low | Medium | Implement lazy loading, optimize images, limit initial load |
| Docker deployment complexity | Medium | High | Use docker-compose, document setup, test locally first |
| SSL certificate renewal | Low | Medium | Use Let's Encrypt with auto-renewal, document manual renewal |
| Database file corruption | Low | High | Regular SQLite backups, consider PostgreSQL for production |
| Form spam/abuse | Medium | Low | Add basic rate limiting, consider CAPTCHA later |

### Process Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Scope creep (feature requests) | Medium | Medium | Lock scope after approval, defer extras to v2.0 |
| Deployment delays | Low | Medium | Test deployment early, document steps |
| Property data entry incomplete | Low | Low | Create seed data script, document required fields |

### Assumptions

1. **Hosting:** Vultr VPS with Docker support is available
2. **Domain:** 3235ivanhoe.com DNS can be configured
3. **Photos:** Property photos will be provided (initially launch without)
4. **Traffic:** Low to moderate traffic (< 1000 visitors/day)
5. **Properties:** Initially 2 properties, platform supports unlimited
6. **Legal:** Privacy/Terms language is acceptable (review with attorney if needed)

---

## SCOPE

### IN SCOPE (v1.0.0)

**Core Features:**
- ✅ Homepage with hero image and stacked property cards
- ✅ Individual property detail pages
- ✅ Photo carousel with lightbox on detail pages
- ✅ Thumbnail previews (3 photos) on property cards
- ✅ Contact forms (homepage and property pages)
- ✅ Property selection logic (manual/auto-select)
- ✅ Editable intro text (HTML in admin)
- ✅ Django admin for property/photo/contact management
- ✅ Contact CSV export
- ✅ Privacy Policy page
- ✅ Terms of Use page
- ✅ Footer with legal links
- ✅ Docker deployment configuration
- ✅ Nginx reverse proxy
- ✅ Let's Encrypt SSL
- ✅ Responsive design (mobile/tablet/desktop)
- ✅ SQLite database
- ✅ Unlimited photos per property
- ✅ Photo ordering and primary designation
- ✅ Comprehensive contact fields (buyer type, timeline, offer, etc.)

### OUT OF SCOPE (Future Versions)

**Deferred to v2.0.0+:**
- ❌ User authentication (public-only site for v1.0)
- ❌ Email notifications on form submission
- ❌ CAPTCHA or anti-spam measures
- ❌ Advanced search/filtering
- ❌ Map integration
- ❌ Virtual tours or 3D walkthroughs
- ❌ Mortgage calculator
- ❌ Property comparison tool
- ❌ Social media integration
- ❌ Analytics dashboard
- ❌ Multi-language support
- ❌ Blog or news section
- ❌ Appointment scheduling
- ❌ Document upload for buyers
- ❌ CRM integration
- ❌ Email campaign tools

---

## DELIVERABLES

### Code Deliverables

1. **Django Application**
   - Property, Photo, Contact, SiteContent models
   - Homepage, property detail, legal pages views
   - Contact form with validation
   - Django admin configuration
   - URL routing

2. **Templates**
   - base.html (shared layout)
   - home.html (homepage with cards)
   - property_detail.html (individual property)
   - privacy.html (Privacy Policy)
   - terms.html (Terms of Use)

3. **Static Assets**
   - CSS (responsive design, carousel styling)
   - JavaScript (carousel, lightbox, form validation)
   - Placeholder images (until real photos provided)

4. **Docker Configuration**
   - Dockerfile (Django app)
   - docker-compose.yml (orchestration)
   - nginx.conf (reverse proxy + SSL)
   - Environment variable configuration

5. **Tests**
   - >95% code coverage
   - Model tests
   - View tests
   - Form tests
   - Admin tests

### Documentation Deliverables

1. **README.md**
   - Project overview
   - Local development setup
   - Docker deployment instructions
   - Environment variables
   - Adding properties guide
   - Photo upload guide

2. **Deployment Guide**
   - Vultr VPS setup
   - Docker installation
   - Domain configuration
   - SSL certificate setup
   - Database backup procedures

3. **Admin Guide**
   - Creating properties
   - Uploading photos
   - Managing contacts
   - Exporting to CSV
   - Editing site content

4. **Legal Content**
   - Privacy Policy text
   - Terms of Use text
   - Disclaimer language

---

## TIMELINE

**Target:** 1-2 days

### Phase Breakdown

- **SPEC Phase:** 2 hours ✅ (Complete)
- **CLIENT APPROVAL GATE #1:** 30 minutes (Review and approve SPEC)
- **BUILD Phase:** 8-10 hours (TDD implementation)
  - Setup: 1 hour
  - Models & Admin: 2 hours
  - Templates: 3 hours
  - Forms & Logic: 2 hours
  - Docker Config: 2 hours
- **VALIDATION Phase:** 1 hour (Internal testing)
- **ACCEPTANCE TEST Phase:** 1 hour (Client testing)
- **CLIENT APPROVAL GATE #2:** 30 minutes (Sign-off)
- **SHIP Phase:** 1-2 hours (Deploy to Vultr)

**Total Estimated Time:** 14-17 hours

---

## DEPENDENCIES

### Technical Dependencies

- Django >= 4.2, < 5.0
- Python >= 3.8
- Pillow >= 10.0 (image processing)
- gunicorn >= 21.0 (WSGI server)

### Development Dependencies

- pytest >= 7.0
- pytest-django >= 4.5
- pytest-cov >= 4.0
- black >= 23.0 (formatting)
- flake8 >= 6.0 (linting)

### Infrastructure Dependencies

- Docker >= 24.0
- Docker Compose >= 2.0
- Vultr VPS (or similar Linux host)
- Domain DNS access (3235ivanhoe.com)

### External Dependencies

- None (self-contained application)

---

## STAKEHOLDERS

**Product Owner / Client:** Nestor Wheelock
**Developer:** Claude Code (AI-Native Development)
**End Users:** Potential property buyers viewing 3233 and 3235 Ivanhoe

---

## APPROVAL

**Created By:** Claude Code
**Date:** 2025-10-01
**Status:** 📋 PENDING CLIENT APPROVAL

This charter will be reviewed as part of SPEC approval (GitHub Issue #1).

---

**Next Steps:**
1. ✅ Create user stories (S-001 through S-005)
2. ✅ Create task breakdown (T-001 through T-008)
3. ✅ Create SPEC_SUMMARY.md
4. ✅ Create initial README.md (SPEC approval guide)
5. ✅ Initialize git repository
6. ✅ Create GitHub repository
7. ✅ Create GitHub Issue #1 for SPEC approval
8. ⏳ Get client approval to begin BUILD phase

---

**End of Project Charter**
