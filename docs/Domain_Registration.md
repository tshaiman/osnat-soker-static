# Domain Registration — osnat-soker.com

## Current State (June 2026)

- Domain `osnat-soker.com` is **registered through Wix** (Wix resells via Network Solutions).
- Wix manages the DNS (nameservers: `ns12.wixdns.net`, `ns13.wixdns.net`).
- The new Astro site is **live on Cloudflare Pages** but not yet connected to the domain.

---

## The Key Problem: Wix Won't Let You Change Nameservers

Cloudflare Pages needs Cloudflare to manage the domain's DNS. That requires pointing the
domain's **nameservers** at Cloudflare. But **Wix does not allow changing nameservers on a
Wix-registered domain** — the option simply doesn't exist in the Wix dashboard. This is
confirmed by both Wix and Cloudflare documentation.

There is no workaround for the bare/apex domain (`osnat-soker.com`) while it stays at Wix:

- A subdomain like `www` could be pointed to Pages with a CNAME from Wix's DNS panel.
- But the **apex** (`osnat-soker.com`) cannot — Cloudflare Pages requires the apex domain to
  be a Cloudflare zone (on Cloudflare nameservers), and Wix blocks that.

**Conclusion:** to use the domain properly on Cloudflare, the domain must be moved off Wix.

---

## Why Not Transfer Straight to Cloudflare Registrar?

Two reasons, stacked:

1. **Cloudflare's entry rule:** Cloudflare Registrar only accepts a domain that is *already*
   running on Cloudflare nameservers. You can't get there from Wix (Wix blocks nameserver
   changes), so Cloudflare can't take it directly.
2. **ICANN 60-day lock:** after any transfer, the domain can't be transferred again for 60
   days.

So the path is: **Wix → an intermediate registrar (Namecheap/Porkbun) → point nameservers at
Cloudflare**. Moving the registration to Cloudflare itself is an optional later step.

---

## Timeline (important)

- **Going live takes about a week** — gated only by the registrar transfer (~5–7 days).
- The **60-day lock is optional/background only**. It applies just to the *later* hop to
  Cloudflare Registrar, which doesn't affect the live site at all.
- There is **no 60-day wait to leave Wix** (the domain is well-aged; it expires Aug 12, 2026).

---

## The Plan — Step by Step

### Step 1 — Transfer the domain away from Wix (do this now)

**In Wix:**
1. Go to **manage.wix.com → Settings → Domains**.
2. Click the **Domain Actions** icon (the `•••` button) next to `osnat-soker.com`.
3. Select **Transfer away from Wix**.
4. Review the info, click **Transfer Domain**.
5. Click **I Still Want to Transfer**.
6. Wix emails a **transfer authorization code (EPP code)** to the domain's registrant contact
   email. Keep this code — it's needed at the new registrar.

**Before/while doing this:**
- Make sure the domain is **unlocked** (Wix may need to do this; contact Wix support if the
  transfer is blocked with a "transfer prohibited" status).
- **Disable Private Registration** if it's on, so you don't miss transfer confirmation emails.
- Note: once transferred away, the Wix site disconnects and Wix no longer manages the domain.
  That's fine — the site now lives on Cloudflare Pages.

### Step 2 — Start the transfer in at Namecheap (the receiving registrar)

1. Create an account at **namecheap.com** (or Porkbun — same idea, cheaper renewals).
2. From the top menu, click **Domains → Transfer**
   (direct link: `https://www.namecheap.com/domains/domain-transfer/`).
3. Type `osnat-soker.com` in the box and click **Transfer**.
4. Namecheap checks the domain is eligible (unlocked, 60+ days old). If it shows as ready,
   **paste the EPP/auth code** from Step 1 into the Auth code field.
5. Click **Add to Cart → View Cart**. The price (~$10–12) includes a 1-year renewal added on
   top of your current expiry.
6. Click **Confirm Order** and check out.
7. Approve any confirmation email Namecheap sends.

**What happens next (no action needed):**
- The domain goes to **"pendingTransfer"** status.
- Wix (the losing registrar) has up to **5–7 days** to release it; it then completes
  automatically. You can speed it up by approving the release from Wix's side if prompted.
- When done, Namecheap emails you and the domain appears under **Domain List**. It's
  auto-renewed for 1 year and re-locked.

> Tip: track progress in Namecheap under **Domain List → Filters → Pending Transfer**.
> Don't change nameservers (Step 4) until the transfer fully completes — nameserver edits are
> locked during a pending transfer.

### Step 3 — Add the domain to Cloudflare

1. Go to **dash.cloudflare.com → Add a site**.
2. Choose **Connect a domain**, enter `osnat-soker.com`, pick the **Free** plan.
3. Let it scan DNS, continue to activation.
4. Copy the **two Cloudflare nameservers** it assigns you
   (e.g. `something.ns.cloudflare.com`). Use the exact pair shown in *your* dashboard.

### Step 4 — Point the nameservers at Cloudflare

1. Log in to the new registrar (Namecheap/Porkbun).
2. Open the domain's **Nameserver** settings.
3. Replace the existing nameservers with the **two Cloudflare nameservers** from Step 3.
4. Save. Propagation is usually under 2 hours (up to 24).

### Step 5 — Connect the site in Cloudflare Pages

1. Cloudflare → **Workers & Pages** → your Pages project → **Custom Domains**.
2. Add `osnat-soker.com` **and** `www.osnat-soker.com`.
3. Cloudflare creates the correct DNS records automatically. **Site is live.**

### Step 6 (optional, after 60 days) — Move registration to Cloudflare

Once 60 days have passed since the Step 2 transfer, you *may* move the registration to
**Cloudflare Registrar** (~$10/year vs ~$15 elsewhere) to consolidate everything.

1. At the intermediate registrar: unlock the domain + get a fresh EPP code.
2. Cloudflare → **Domain Registration → Transfer Domains** → enter the domain + code.
3. Pay (~$10, covers a year) and confirm via email. Completes in 5–7 days.

DNS and the live site are unaffected — Cloudflare already manages DNS by this point. This step
is purely a cost optimization and can be skipped entirely.

---

## Cost Summary

| Phase | Where | Cost |
|-------|-------|------|
| Now → live | Domain at Namecheap/Porkbun, DNS on Cloudflare | ~$10–15/year |
| After 60 days (optional) | Registration moved to Cloudflare Registrar | ~$10/year |
| Cloudflare Pages hosting | Free tier | $0 |

---

## Rollback

Until the transfer in Step 1 completes, nothing has changed and the old Wix setup still serves
the domain. After nameservers are on Cloudflare (Step 4), if anything looks wrong you can point
the nameservers back at the previous values at the registrar level. The old Wix DNS records
were:

| Type  | Name            | Value           |
|-------|-----------------|-----------------|
| A     | osnat-soker.com | 185.230.63.171  |
| A     | osnat-soker.com | 185.230.63.107  |
| A     | osnat-soker.com | 185.230.63.186  |
| CNAME | www             | cdn1.wixdns.net |
