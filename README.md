# 3235 Ivanhoe - Property Listing Platform

⚠️ **STATUS**: This project is in SPEC phase. No code has been written yet.
This README guides you through the approval process.

---

## What This Will Be

A professional property listing platform for marketing two Ivanhoe properties (3233 commercial building and 3235 live-work building) with comprehensive buyer lead capture and management.

**Key Features:**
- Beautiful homepage with hero images and property cards
- Individual property pages with photo carousels and lightbox
- Comprehensive contact forms with buyer qualification fields
- Django admin backend for property and contact management
- Legal pages (Privacy Policy, Terms of Use)
- Docker deployment to Vultr with SSL

**Domain:** https://3235ivanhoe.com

---

## SPEC Documents for Review

Please review these planning documents:

### Core Documents
- **[Project Charter](planning/PROJECT_CHARTER.md)** - Complete project specification (WHAT, WHY, HOW, risks, success criteria)
- **[SPEC Summary](planning/SPEC_SUMMARY.md)** - Quick reference guide

### User Stories
- **[S-001: Homepage Design](planning/stories/S-001-homepage-design.md)** - Hero header, property cards, intro text
- **[S-002: Property Detail Pages](planning/stories/S-002-property-detail-pages.md)** - Individual pages with carousels
- **[S-003: Photo Management](planning/stories/S-003-photo-management.md)** - Upload, organize, display photos
- **[S-004: Contact Form System](planning/stories/S-004-contact-form.md)** - Lead capture with qualification fields
- **[S-005: Admin Backend](planning/stories/S-005-admin-backend.md)** - Property and contact management

### Implementation Tasks
- **[T-001: Django + Docker Setup](planning/tasks/T-001-django-docker-setup.md)** - Project foundation
- **[T-002: Data Models](planning/tasks/T-002-data-models.md)** - Property, Photo, Contact, SiteContent
- **[T-003: Homepage Template](planning/tasks/T-003-homepage-template.md)** - Homepage implementation
- **[T-004: Property Detail Pages](planning/tasks/T-004-property-detail-pages.md)** - Individual property pages
- **[T-005: Contact Forms](planning/tasks/T-005-contact-forms.md)** - Form implementation
- **[T-006: Legal Pages](planning/tasks/T-006-legal-pages.md)** - Privacy Policy, Terms of Use
- **[T-007: Admin Configuration](planning/tasks/T-007-admin-configuration.md)** - Django admin setup
- **[T-008: Docker Deployment](planning/tasks/T-008-docker-deployment.md)** - Production deployment config

---

## Key Design Decisions

**Homepage Design:**
- Hero header image showing both properties
- Editable intro text (you can update via admin using HTML)
- Property cards stacked vertically
- Large primary photo + 3 thumbnail previews per card
- Contact form at bottom

**Property Pages:**
- Individual hero headers per property
- Full photo carousel with lightbox
- Unlimited photos per property
- Complete property details (sq ft, year, zoning, etc.)
- Contact form with auto-selection

**Contact Form:**
- Homepage: Manual property selection (3233 / 3235 / Both)
- Property page: Auto-select + "Also interested in" checkbox
- Fields: Name, Email, Phone (required)
- Optional: Company, Buyer Type, Timeline, Financing, Offer Amount, Message

**Technology:**
- Django 4.2+ / Python 3.8+
- SQLite database (simple, sufficient)
- Docker + Nginx + Let's Encrypt SSL
- Vanilla JavaScript (no frameworks)
- Hosted on Vultr VPS

---

## What's IN Scope (v1.0.0)

✅ Homepage with property cards
✅ Individual property detail pages
✅ Photo carousel with lightbox
✅ Comprehensive contact forms
✅ Django admin for management
✅ Contact CSV export
✅ Editable site content (HTML)
✅ Privacy Policy and Terms pages
✅ Docker deployment
✅ SSL certificate
✅ Responsive design (mobile/tablet/desktop)
✅ Unlimited photos per property

---

## What's OUT of Scope (Future)

❌ Email notifications (check admin manually)
❌ CAPTCHA/spam protection
❌ Property search/filtering
❌ Map integration
❌ User authentication
❌ Analytics dashboard
❌ CRM integration
❌ Virtual tours

*These can be added in v2.0 if desired*

---

## Timeline & Effort

**Estimated Timeline:** 1-2 days

**Phase Breakdown:**
- SPEC Phase: 2 hours ✅ (Complete - awaiting your approval)
- CLIENT APPROVAL GATE #1: 30 minutes ⏳ (You are here)
- BUILD Phase: 8-10 hours (TDD implementation)
- VALIDATION Phase: 1 hour (Internal QA)
- ACCEPTANCE TEST Phase: 1 hour (You test it)
- CLIENT APPROVAL GATE #2: 30 minutes (You approve for deployment)
- SHIP Phase: 1-2 hours (Deploy to Vultr)

**Total Estimated:** 14-17 hours

---

## Approval Process

### Step 1: Review All Documents ✋

Read through the planning documents listed above, especially:
1. Project Charter (complete specification)
2. SPEC Summary (quick overview)
3. User Stories (what you'll get)

### Step 2: Ask Questions or Request Changes 💬

If anything is unclear or you want changes:
- Comment on GitHub Issue #1 (will be created after your initial review)
- Or respond directly to this proposal

**Common things to consider:**
- Are the contact form fields correct?
- Do you want different property information displayed?
- Any features missing from your vision?
- Timeline acceptable?

### Step 3: Formal Approval ✅

Once you're satisfied with the SPEC, approve by:
- Responding "APPROVED" or "Looks good, proceed"
- Closing GitHub Issue #1

**After approval:**
- Scope is locked (changes require new approval process)
- Development begins immediately
- This README will be replaced with full documentation

---

## What Happens After Approval

1. **BUILD Phase begins** - Code is written following 23-step TDD cycle
2. **GitHub repo created** - Code pushed regularly
3. **Regular updates** - Progress visible in commit history
4. **Internal testing** - VALIDATION phase ensures quality
5. **You test it** - ACCEPTANCE TEST phase (you try it out)
6. **You approve again** - CLIENT APPROVAL GATE #2
7. **Deployment** - Goes live at https://3235ivanhoe.com
8. **Admin access** - You get credentials to manage properties/contacts

---

## Deployment Details

**Hosting:** Vultr VPS (Linux)
**Domain:** 3235ivanhoe.com (you'll need to point DNS to server IP)
**SSL:** Let's Encrypt (free, auto-renews)
**Database:** SQLite file (backed up regularly)
**Photos:** Unlimited, stored on server

**What you'll need:**
- Vultr account (or I can help set this up)
- DNS access to point 3235ivanhoe.com to server
- Property photos (can add after launch)
- Property details for 3233 and 3235

---

## Legal Pages

**Privacy Policy:**
- Standard online privacy disclaimer
- "No expectation of privacy on the internet"
- Data collection and use explanation

**Terms of Use:**
- No rights granted to site users
- **Important:** "All offers submitted are speculative only and NOT binding"
- "Submissions do not constitute acceptance or agreement"
- Seller reserves right to reject all offers
- Liability disclaimers

*Legal language is included in the build. Review with attorney if needed.*

---

## Questions?

Before formal approval, feel free to ask:

**About features:**
- "Can we add [X] feature?" (May be v2.0)
- "How does [Y] work exactly?"
- "Can we change [Z]?"

**About timeline:**
- "When can this be live?"
- "How long will BUILD phase take?"

**About deployment:**
- "What does hosting cost?"
- "How do I update property info after launch?"

**About technical:**
- "Why SQLite instead of PostgreSQL?"
- "Why Django vs WordPress?"

---

## Ready to Approve?

If you've reviewed the SPEC documents and are satisfied, reply with:

**"APPROVED - Proceed to BUILD phase"**

Then we'll:
1. Create GitHub repository
2. Initialize project structure
3. Begin 23-step TDD development
4. Keep you updated on progress
5. Deliver working site for your testing

---

**Project:** 3235-ivanhoe v1.0.0
**Created:** 2025-10-01
**Status:** 📋 PENDING CLIENT APPROVAL (GATE #1)
**Developer:** Claude Code

---

**Questions? Review planning documents above or ask for clarification.**
