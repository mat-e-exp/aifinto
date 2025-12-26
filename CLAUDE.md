# aifinto - Landing Page & Infrastructure

Portfolio landing page for AI financial tools at https://aifinto.com

---

## What This Is

Simple landing page linking to AI-Pulse and future products. Handles user signup flow.

**Live site**: https://aifinto.com
**Repo**: mat-e-exp/aifinto (GitHub Pages)

---

## Architecture

```mermaid
flowchart TD
    subgraph User["User Flow"]
        U1[User visits aifinto.com]
        U2[Clicks 'Login' for AI-Pulse]
        U3[Enters email for access request]
    end

    subgraph Signup["Signup Flow"]
        F1[Formspree receives email]
        F2[Formspree emails you notification]
        F3[You send branded response from hello@aifinto.com]
    end

    subgraph Email["Email Infrastructure"]
        E1[hello@aifinto.com]
        E2[Cloudflare Email Routing]
        E3[Your personal Gmail]
        E4[Gmail 'Send as' feature]
    end

    subgraph DNS["DNS (Cloudflare)"]
        D1[A records → GitHub Pages]
        D2[MX records → Cloudflare]
        D3[SPF record includes Google + Cloudflare]
    end

    U1 --> U2 --> AI_PULSE[ai-pulse.aifinto.com]
    U1 --> U3 --> F1
    F1 --> F2 --> E3
    E3 --> E4 --> E1
    E1 --> E2 --> E3
```

---

## Signup Flow

**Formspree endpoint**: `https://formspree.io/f/xpqzzyjl`

1. User enters email on aifinto.com
2. JavaScript POSTs to Formspree
3. Formspree sends notification to your email
4. You manually send branded response using Gmail template

**Code location**: `index.html` lines 38-70

---

## Email Infrastructure

### Receiving (hello@aifinto.com → your inbox)

**Cloudflare Email Routing** (configured in Cloudflare dashboard):
- Catch-all or specific address: hello@aifinto.com
- Forwards to: your personal Gmail

### Sending (from hello@aifinto.com)

**Gmail "Send as"** feature:
- SMTP server: smtp.gmail.com:587
- Authentication: Gmail App Password (not regular password)
- Reply-to: hello@aifinto.com

### DNS Records (Cloudflare)

| Type | Name | Value |
|------|------|-------|
| MX | @ | Priority 41, route1.mx.cloudflare.net |
| MX | @ | Priority 1, route2.mx.cloudflare.net |
| MX | @ | Priority 29, route3.mx.cloudflare.net |
| TXT | @ | `v=spf1 include:_spf.google.com include:_spf.mx.cloudflare.net ~all` |

**SPF includes both**:
- `_spf.google.com` - for Gmail "Send as"
- `_spf.mx.cloudflare.net` - for Cloudflare Email Routing

---

## Gmail Template

Branded email template created in Gmail (Settings → Templates):
- Includes AI-Pulse logo (PNG)
- Welcome message for new signups
- Access: Gmail → Compose → More options → Templates

---

## Files

```
aifinto/
├── index.html         # Landing page with signup form
├── style.css          # Styling
├── logo.svg           # Static logo
├── logo-animated.svg  # Animated pulse logo
└── CNAME              # Custom domain config
```

---

## Deployment

**Hosting**: GitHub Pages from main branch
**Domain**: aifinto.com (Cloudflare DNS)
**SSL**: Automatic via GitHub Pages

To deploy changes:
```bash
git add .
git commit -m "Description"
git push
```
GitHub Pages auto-deploys on push.

---

## Related

- **AI-Pulse**: https://ai-pulse.aifinto.com (main product)
- **AI-Pulse repo**: mat-e-exp/ai-pulse (private)
- **Portfolio strategy**: `/Users/mat.edwards/dev/test-claude/PORTFOLIO-STRATEGY-2025-12.md`

---

## Changelog

**2025-12-26**:
- Added Formspree signup form (endpoint: xpqzzyjl)
- Set up Cloudflare Email Routing for hello@aifinto.com
- Configured Gmail "Send as" with App Password
- Updated SPF record to include Google
- Created branded Gmail template with logo
