# ProFinders (profinders.shmor.ai)

A Shmor marketplace (shared rules in `../../CLAUDE.md`): its own brand, design and SEO. Helps businesses find and compare HR consultants, employment and business lawyers, accountants, bookkeepers, social media marketers, web designers, business consultants and IT support, then refers them to quote platforms (Bark or similar) for a referral fee. Repo: `Shmorai/shmor-profinders` (private until launch). Status: **building**, with no referral links yet.

## Where things are
| What | Where |
|---|---|
| Card on shmor.ai, search keywords, **referral links**, offers, SEO defaults | `shmor-site.json` |
| Pages (1 JSON file = 1 Google landing page) | `content/` (`home.json` = homepage) |
| Design: header, logo, footer | `templates/layout.html` |
| Design: styles | `assets/site.css` (kit defaults at the top; **ProFinders theme** block at the bottom) |
| Generated output (don't edit by hand) | `index.html`, `*/index.html`, `sitemap.xml`, `robots.txt`, `404.html`, `search/`, `search-index.json` |

## Pages (12)
Home · services (directory hub) · hr-consultants · employment-lawyers · business-lawyers · accountants · bookkeepers · social-media-marketing · web-designers · business-consultants · it-support · how-it-works.
Each service page has these sections: what they help with, when to hire, questions to ask, what affects the cost, plus FAQs (FAQ rich results), a "Free / Compare / No obligation" highlights row, and related services.
Top menu (`navOrder`): All services, HR consultants, Employment lawyers, Accountants, Social media, How it works.

## Design
"Trusted professional": white pages, deep navy #0F2440 / #173459 hero and bands, emerald #0F9B77 actions (dark #0B7D60, soft #E6F6F1), mint #2DD4A7 highlights on navy, Inter font. Logo = mint magnifier in a navy rounded square + "Pro**Finders**" (Finders in emerald, or mint on dark) + "by Shmor.ai" pill. Every hero is a photo with a navy overlay. On home the service cards are moved up under the benefits row with CSS `order` (HTML order unchanged). Bullet lists use emerald check marks. Bottom CTA is a navy band and the footer is dark navy. Mobile checked at 375px (no horizontal scroll).
**Photos:** free-licence Unsplash images (not Unsplash+), credited, in each page's `"image"`.

## Search routing on shmor.ai
`keywords` in `shmor-site.json`. The master's router does **substring** matching on `" " + query + " "`, so short words must be padded to match only whole words: `" hr "`, `" vat "`, `" cpa "`, `" seo "`, `" llc "`. Never add a bare 2–3 letter keyword ("hr" would match "christmas"). Tested: "need help with company hr", "employment lawyer", "it support for small business" and "vat return help" go to ProFinders; "plumber near me" and "contractor quote" stay with Shmor Pros (tradespeople).

## Commands (run here)
```bash
node tools/build.js           # rebuild after ANY change to content/, templates/ or shmor-site.json
node tools/build.js --check   # validate only
node tools/serve.js 4002      # preview http://localhost:4002  (preview config "profinders")
```
After changing `shmor-site.json`, also run `node tools/sync-network.js` in `../../shmor-ai`.

## Referral links (the money)
None yet. The user plans to refer leads to **Bark** (or similar quote marketplaces) for a referral fee. They need to join that platform's partner/affiliate programme and get a tracking link first. Then:
1. Add to `shmor-site.json` → `"links"`, e.g. `"bark": { "merchant": "Bark", "network": "direct", "url": "<exact link from the user>", "blurb": "Get free quotes from local professionals" }`. Paste the URL exactly as the user gives it.
2. Add `"partners": ["bark"]` to every service page (the block's heading is already set per page: "Get free quotes from HR consultants" etc.), and optionally `"fallback": "bark"` in the manifest so shmor.ai searches earn before launch.
3. `node tools/build.js`, sync the master, commit both repos.

## History & decisions
- 2026-10-13: the user asked for a "professional finders" site, keyword-rich for businesses searching for HR, legal, social media and other suppliers, that looks like a professional service marketplace and refers leads on to Bark or similar for referral fees. It was first drafted as a standalone static site in `Claude Sessions Local/pro-finders-site/` (same content, own build script). The user then asked to add it to the Shmor network like TravelDeals, so it was rebuilt here on the site kit and the draft folder was superseded.
- Kept separate from planned **Shmor Pros** (tradespeople: plumbers, cleaners). ProFinders covers business/professional services. They could be merged later if the user prefers.
- The user mentioned a separate **.com domain** and design they'd provide. It's served at `profinders.shmor.ai` for now. A .com could later point at the same repo via its `CNAME` (one custom domain per GitHub Pages repo).
- Honesty rules: footer and FAQs say ProFinders is an introduction service that doesn't employ or vet professionals and may earn referral fees. Content gives general guidance only (not legal/tax advice). No invented reviews, ratings, prices or "X pros" counts.
- Chat setup: this site has its own pinned chat titled "ProFinders".

## Ideas / next
Bark (or other) referral link · the user's .com domain + design · more service pages (recruitment agencies, payroll providers, copywriters, SEO agencies, video production, translation) · a blog answering what people search for (e.g. "how to handle a disciplinary", "do I need an accountant for a limited company") · US/UK location pages only if they carry genuinely unique content.
