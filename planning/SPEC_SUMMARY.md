# SPEC SUMMARY - 3235ivanhoe.com v1.0.0

Quick reference for project specification.

---

## Project Overview

**Name:** 3235-ivanhoe
**Type:** Property Listing Platform
**Domain:** 3235ivanhoe.com
**Purpose:** Market two Ivanhoe properties (3233 commercial, 3235 live-work) and capture buyer leads
**Timeline:** 1-2 days (14-17 hours estimated)

---

## Core Features

1. **Homepage**
   - Hero image (both properties)
   - Editable intro text (HTML in admin)
   - Stacked property cards (large photo + 3 thumbnails)
   - Contact form

2. **Property Detail Pages**
   - Individual hero headers
   - Full photo carousel with lightbox
   - Complete property details
   - Contact form with auto-selection

3. **Contact Management**
   - Comprehensive buyer qualification fields
   - Property selection/auto-selection logic
   - Offer capture with amount field
   - CSV export from admin

4. **Photo Management**
   - Unlimited photos per property
   - Primary photo designation
   - Display order control
   - Carousel + lightbox display

5. **Admin Backend**
   - Django admin for all management
   - Property creation/editing
   - Photo upload (inline)
   - Contact viewing/export
   - Editable site content (HTML textarea)

6. **Legal Pages**
   - Privacy Policy
   - Terms of Use with offer disclaimers

---

## Technology Stack

**Backend:** Django 4.2+, Python 3.8+
**Database:** SQLite
**Deployment:** Docker + Nginx + Let's Encrypt SSL
**Frontend:** Vanilla JavaScript, responsive CSS
**Hosting:** Vultr VPS (Linux)

---

## User Stories

- **S-001:** Homepage with Hero & Property Cards
- **S-002:** Individual Property Detail Pages
- **S-003:** Photo Management & Carousel
- **S-004:** Contact Form System
- **S-005:** Admin Management & Editable Content

---

## Tasks

- **T-001:** Django + Docker Setup (2 hours)
- **T-002:** Data Models (3 hours)
- **T-003:** Homepage Template (3 hours)
- **T-004:** Property Detail Pages (4 hours)
- **T-005:** Contact Forms (3 hours)
- **T-006:** Legal Pages (2 hours)
- **T-007:** Admin Configuration (2 hours)
- **T-008:** Docker Deployment (3 hours)

**Total:** 22 hours estimated

---

## Data Models

### Property
- Title, slug, address
- Property type, status
- Details: sq ft, year built, lot size, zoning
- Price display logic
- Featured, display_order

### Photo
- Property FK
- Image file
- Caption, is_primary, display_order

### Contact
- Property FKs (primary + also_interested_in)
- Contact info: name, email, phone, company
- Buyer details: type, timeline, financing
- Offer: making_offer, offer_amount
- Message, submitted_at

### SiteContent
- Unique key
- HTML content
- Last_updated

---

## Key Design Decisions

1. **Single-page vs Multi-page:** Multi-page (homepage + individual property pages)
2. **Photo Limit:** Unlimited per property
3. **Contact Strategy:** Form only, no public contact info
4. **Database:** SQLite (sufficient for low traffic)
5. **Deployment:** Docker for portability and ease
6. **SSL:** Let's Encrypt (free, auto-renew)
7. **Editable Content:** HTML textarea (simple, powerful)
8. **Admin:** Django admin (no custom backend needed)

---

## Success Criteria

**Functional:**
- ✅ Homepage displays both properties with photos
- ✅ Property pages show full details and carousel
- ✅ Contact forms work and save to database
- ✅ Admin can manage properties, photos, contacts
- ✅ Legal pages accessible
- ✅ SSL certificate active

**Quality:**
- ✅ >95% test coverage
- ✅ Responsive design (mobile/tablet/desktop)
- ✅ Performance: page load <2s
- ✅ Security: CSRF, SQL injection prevention

**Deployment:**
- ✅ Live at https://3235ivanhoe.com
- ✅ Docker containers running
- ✅ SSL auto-renewal configured

---

## Out of Scope (v1.0)

- ❌ Email notifications
- ❌ CAPTCHA/spam protection
- ❌ Search/filtering
- ❌ Map integration
- ❌ User authentication
- ❌ Analytics dashboard
- ❌ CRM integration

---

## Approval Gates

**🚦 CLIENT APPROVAL GATE #1 (SPEC Phase)**
- Review all planning documents
- Approve scope, features, approach
- Sign-off to proceed to BUILD

**🚦 CLIENT APPROVAL GATE #2 (Acceptance Test)**
- Test complete application
- Verify all user stories pass
- Sign-off to deploy to production

---

## Contact Form Fields

**Required:**
- Name, Email, Phone

**Property Selection:**
- Homepage: Dropdown (3233 / 3235 / Both)
- Property page: Auto-select + "Also interested in" checkbox

**Optional:**
- Company
- Buyer Type: Investor / Owner-Occupant / Developer / Other
- Timeline: Immediate / 3-6 months / 6-12 months / Exploring
- Financing: Cash / Pre-approved / Seeking / Unsure
- Making Offer: Checkbox
- Offer Amount: Dollar field (conditional)
- Message: Textarea

---

## Deployment Summary

1. **Setup VPS:** Vultr Ubuntu 22.04
2. **Install:** Docker + Docker Compose
3. **Configure DNS:** Point 3235ivanhoe.com to VPS IP
4. **Deploy:** `docker-compose up -d`
5. **SSL:** Run certbot for Let's Encrypt
6. **Create Admin:** `createsuperuser`
7. **Load Data:** Add properties via admin
8. **Go Live:** Site accessible at https://3235ivanhoe.com

---

## Next Steps

1. ✅ SPEC documents created
2. ⏳ **CLIENT APPROVAL GATE #1** - Review and approve
3. ⏳ BUILD Phase (23-step TDD cycle)
4. ⏳ VALIDATION Phase (internal QA)
5. ⏳ ACCEPTANCE TEST Phase (client testing)
6. ⏳ **CLIENT APPROVAL GATE #2** - Sign-off
7. ⏳ SHIP Phase (deploy to production)

---

**For complete details, see:** `planning/PROJECT_CHARTER.md`

**Created:** 2025-10-01
**Status:** 📋 PENDING CLIENT APPROVAL
