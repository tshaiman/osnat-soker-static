# Domain Registration — osnat-soker.com

## Current State (June 2026)

- Domain `osnat-soker.com` is **registered at Network Solutions** (Wix resells through them)
- Wix manages the DNS (nameservers: `ns12.wixdns.net`, `ns13.wixdns.net`)
- The new Astro site is **live on Cloudflare Pages** but not yet connected to the domain
- A transfer auth code was issued by Wix: valid 7 days from issue date (already expired — request a new one when needed)

---

## Why We Can't Transfer Directly to Cloudflare

Cloudflare blocks direct transfers from Wix-managed domains due to an **ICANN rule**: any domain that has been transferred must wait **60 days** before it can be transferred again. Since the domain is at Network Solutions (via Wix), transferring it out triggers that 60-day lock — meaning you'd have to:

1. Transfer Wix → another registrar (e.g., Namecheap) — day 0
2. Wait 60 days
3. Transfer that registrar → Cloudflare — day 60+

This is not worth it right now.

---

## The Plan: Now → 60 Days

### Do This Now: Point DNS to Cloudflare (keep domain at Wix)

This makes the new Astro site live at `osnat-soker.com` **today**, without transferring anything.

**In Cloudflare:**
1. Go to [dash.cloudflare.com](https://dash.cloudflare.com)
2. Add site → type `osnat-soker.com` → Free plan
3. Continue through DNS scan → "Continue to activation"
4. Note your assigned nameservers:
   - `alexandra.ns.cloudflare.com`
   - `guss.ns.cloudflare.com`

**In Wix (manage.wix.com → Domains → osnat-soker.com → Nameservers):**
1. Replace `ns12.wixdns.net` and `ns13.wixdns.net`
2. Add `alexandra.ns.cloudflare.com` and `guss.ns.cloudflare.com`
3. Save

DNS propagation: up to 24 hours, usually under 2 hours.

**Then in Cloudflare Pages:**
- Go to your Pages project → Custom Domains → add `osnat-soker.com` and `www.osnat-soker.com`
- Cloudflare will auto-create the correct DNS records

---

## The DNS Records That Need Changing (Wix side)

Cloudflare scanned and found these **old Wix records** that will be replaced once nameservers switch:

| Type  | Name         | Current Value         | Action         |
|-------|--------------|-----------------------|----------------|
| A     | osnat-soker.com | 185.230.63.171     | Delete / replace with Cloudflare Pages CNAME |
| A     | osnat-soker.com | 185.230.63.107     | Delete |
| A     | osnat-soker.com | 185.230.63.186     | Delete |
| CNAME | www          | cdn1.wixdns.net       | Delete / replace with Cloudflare Pages CNAME |

Once nameservers are pointed to Cloudflare, **Cloudflare manages all DNS** — these Wix records become irrelevant. Cloudflare Pages will add its own records automatically when you connect the custom domain.

---

## After 60 Days: Optional Transfer to Cloudflare Registrar

Once 60 days have passed since Wix released the domain, you can optionally transfer the registration itself to Cloudflare Registrar (~$10/year vs ~$15/year at Wix). This saves ~$5/year and consolidates everything in one place.

**Steps at that point:**
1. Request a new auth/EPP code from Wix (the previous one expired)
2. Go to Cloudflare → Domain Registration → Transfer Domains
3. Enter `osnat-soker.com` + the auth code
4. Pay ~$10 (covers one year renewal)
5. Confirm via email — transfer completes in 5–7 days

> Note: DNS will be unaffected since Cloudflare already manages it.

---

## Cost Summary

| Phase | Where | Cost |
|-------|-------|------|
| Now | Domain stays at Wix/Network Solutions | ~$15/year |
| After 60 days (optional) | Transfer to Cloudflare Registrar | ~$10/year |
| Cloudflare Pages hosting | Free tier | $0 |
