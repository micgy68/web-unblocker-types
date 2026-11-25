# Web Unblocker: Which of the Four Types You Need, What Free Proxies Won't Do, and ScraperAPI's Plans and Credit Math (Full Table)

"Web unblocker" gets used for two purchases that share almost nothing. One opens a single blocked page inside your browser, costs nothing, and takes about ten seconds. The other is a developer tool that fetches pages at scale from code, and that is the one with a price tag: ScraperAPI's entry plan is **$49/month for 100,000 credits**, and how far those credits stretch depends entirely on which sites you point them at.

If you're a student, a traveller, or someone on an office network trying to reach one site, you want the first thing. Jump to the free-tools section below and ignore the pricing tables. If you're writing a scraper that keeps getting 403s from Cloudflare, Akamai, or Datadome, the rest of this page is for you.

What follows covers the four product categories hiding behind the same phrase, every ScraperAPI tier with its price and limits, the credit arithmetic that decides your real monthly cost, and the limits free proxies admit to only in their FAQ.

> **Fast answer:** most projects that need unblocking start on **Hobby at $49/month** (100,000 credits, 20 concurrent threads, US and EU geotargeting). Move up only when you can name the reason: more than 20 parallel requests (Startup), global IP coverage (Business), or the ability to keep scraping past your monthly cap at a fixed per-credit rate (Scaling and above).
> 👉 [Compare all seven ScraperAPI plans side by side](https://www.scraperapi.com/pricing?fp_ref=coupons)

## The four things people mean by "web unblocker"

**A free web proxy.** You paste a URL into a box, the proxy server fetches the page, and you read it in your browser. No account, no install. Unblock-Web is a representative example, and its FAQ is unusually candid: it only opens domains that have been reviewed and approved in advance rather than acting as an open gateway, and pages that require a login are deliberately unsupported.

**A browser extension.** Same idea, one click from the toolbar. Website Unblocker on the Chrome Web Store lists 6,000 users, version 1.0.0, and a last update of 28 October 2024. Its technical pitch is narrow tunnelling: only requests to the blocked site go through the proxy, so the rest of your browsing keeps its normal speed.

**A self-hosted proxy.** Holy Unblocker LTS is the best-known open-source option. You deploy it yourself through GitHub Codespaces, Railway, or a VPS, run `npm run fresh-install`, put NGINX in front, and set `production` to true in `config.json`. Its author describes it as "a proof of concept for the ideal clientless solution to bypassing censorship." Free software, real maintenance work.

**An unblocking API.** This is what ScraperAPI sells. Your code sends a URL; the service handles IP rotation across residential and datacenter pools, realistic browser headers, CAPTCHA detection with automatic retry, JavaScript rendering via a `render=true` parameter, and country-level geotargeting, then returns HTML or JSON. Nobody sits in front of it. It runs inside a job.

Most people searching this term want category one or two. Category four is a procurement decision, so treat the rest of this article as a buying guide rather than a tutorial.

## ScraperAPI plans and prices

ScraperAPI lists seven tiers, from a $49 hobby plan to custom enterprise contracts. Prices below are monthly USD, with the annual-billing monthly equivalent in brackets; annual billing takes roughly 10% off. Plan lineups and prices do change, so confirm the figure on the page before you enter card details.

| Plan | Credits per month | Concurrent threads | Price | Get started |
| --- | --- | --- | --- | --- |
| Hobby | 100,000 | 20 | $49/mo ($44.10 annual) | [ Choose Hobby](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Startup | 1,000,000 | 50 | $149/mo ($134.10 annual) | [ Choose Startup](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Business | 3,000,000 | 100 | $299/mo ($269.10 annual) | [ Choose Business](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Scaling | 5,000,000 | 200 | $475/mo ($427.50 annual) | [ Choose Scaling](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Professional | 10,500,000 | 300 | $975/mo ($877.50 annual) | [ Choose Professional](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Advanced | 21,500,000 | 500 | $1,975/mo ($1,777.50 annual) | [ Choose Advanced](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Enterprise | 22M+ | 500+ | Custom | [ Request Enterprise pricing](https://www.scraperapi.com/pricing?fp_ref=coupons) |

Credits are not the only difference between tiers. Geotargeting and overage handling change at specific breakpoints, and those two rows decide more purchases than the credit number does.

| Plan | Geotargeting | Analytics history | When credits run out |
| --- | --- | --- | --- |
| Hobby | US and EU only | Last 30 days | Opt-in auto-upgrade to next plan |
| Startup | US and EU only | Last 30 days | Opt-in auto-upgrade |
| Business | Global | Unlimited | Opt-in auto-upgrade |
| Scaling | Global | Unlimited | Auto-upgrade or pay-as-you-go with a monthly cap |
| Professional | Global | Unlimited | Auto-upgrade or pay-as-you-go with a monthly cap |
| Advanced | Global | Unlimited | Pay-as-you-go with a monthly cap |
| Enterprise | Global | Unlimited | Pay-as-you-go with a monthly cap |

Every tier includes custom headers, custom sessions, desktop and mobile user agents, automatic retries, and unlimited bandwidth on a 99.9% uptime commitment.

## Why 100,000 credits is not 100,000 pages

One credit buys one standard page request. Harder targets carry multipliers, and ScraperAPI prices them explicitly: Amazon costs 5 credits, Google and Bing including their subdomains cost 25, LinkedIn costs 30, and any request that has to get past Cloudflare, Datadome, or PerimeterX adds 10 credits per request.

Run those numbers against the Hobby allowance and the picture changes fast.

| Target type | Credits per request | Pages on Hobby (100,000) | Pages on Business (3,000,000) |
| --- | --- | --- | --- |
| Ordinary page | 1 | 100,000 | 3,000,000 |
| Amazon | 5 | 20,000 | 600,000 |
| Google or Bing results | 25 | 4,000 | 120,000 |
| LinkedIn | 30 | 3,333 | 100,000 |
| Cloudflare-protected page | 11 | 9,090 | 272,727 |

A price-comparison project pulling Amazon listings gets one fifth of the pages a blogger pulling ordinary articles gets for the same $49. A rank-tracking job hitting Google burns the allowance 25 times faster than the headline suggests.

Three tools exist to stop this from being guesswork, and none of them appear in most write-ups of the product. The Domain Cost Estimator in the dashboard prices any URL before you scrape it. The `/account/urlcost` endpoint returns the same figure programmatically, accepting your intended request parameters. And every response carries an `sa-credit-cost` header showing what that request actually consumed. You can also set a `max_cost` on a single scrape to cap it.

👉 [Pick the plan that matches your real monthly page volume](https://www.scraperapi.com/pricing?fp_ref=coupons)

## What happens at the limit

Exceeding your plan returns a 429 asking you to bring concurrency back within the plan's limit. The request is turned away rather than queued, and there is no extra charge for going over. That matters for production jobs: a rejected request is a failed job, not a slow one, so a retry wrapper and concurrency monitoring are part of running this reliably.

Credits reset when the subscription renews and do not roll over. You can move up or down a tier from the billing page at any time. Auto-upgrade is opt-in and available on Hobby, Startup, Business, Scaling, and Professional. Pay-as-you-go, which keeps requests flowing at a fixed per-credit rate up to a monthly cap you set, is available on Scaling, Professional, Advanced, and Enterprise. Annual plans can also be renewed early for a fresh batch of credits, which is the closest thing to banking unused allowance.

👉 [Start on Hobby at $49/month and watch your first month's credit usage](https://www.scraperapi.com/pricing?fp_ref=coupons)

## A change worth knowing if you scrape Akamai-protected sites

Since 28 October 2026, ScraperAPI unblocks Akamai-protected sites without any extra parameters. A standard request now handles the bypass. If you were previously setting `premium=true` or `ultra_premium=true` specifically to get through Akamai, retest those URLs without the flags: they are no longer required for Akamai, and optional parameters can affect credit consumption even when they aren't needed.

## What free web proxies will not do

The free category is genuinely useful, and the operators are more honest about its edges than the marketing pages of paid services tend to be. Unblock-Web's own FAQ gives the clearest list.

Pages behind a login are out. "It might require a login (which we intentionally don't support, for your safety)." You cannot use a free proxy to reach a webmail inbox, a bank portal, or a paid course dashboard.

Domain coverage is curated, not open. Unblock-Web only opens domains reviewed and approved in advance, with an Approved Domains page and a request form for additions. The internet you can reach through it is a subset, not the whole thing.

Streaming usually fails. Netflix and similar platforms actively block proxy IPs. One competing free proxy, Website Unblocker, claims support for "virtually any website" including streaming services, alongside 2M+ users, 50+ server locations, and a 99.9% availability figure. Those numbers come from the site's own marketing, with no third-party audit behind them, and they contradict what Unblock-Web says about streaming. Treat the claim as unverified rather than settled.

Speed fluctuates with the clock. Free proxy servers carry heavy traffic, so throughput depends on the time of day and your distance from the server. A paid proxy or VPN outperforms them on consistency.

Never type credentials into one. "Don't enter passwords or banking details through any free proxy, ours included." The same warning appears in the Google Play listing for an Android proxy browser called Website Unblocker, which advises against entering passwords, banking details, or confidential information unless you trust the connection. That app's Data safety section is also internally inconsistent: it states the app may share location, app performance, and device IDs with third parties while separately declaring "No data collected." Do not read that listing as a privacy guarantee.

A proxy is not a VPN. It covers the browser session you opened it in, typically one tab. A VPN routes every app on the device and encrypts all of it. If the goal is protecting a whole machine, a proxy is the wrong tool.

## Free proxy vs extension vs self-hosted vs API vs VPN

|  | Free web proxy | Browser extension | Self-hosted proxy | Unblocking API | VPN |
| --- | --- | --- | --- | --- | --- |
| Setup | Paste a URL | One click | Node.js, NGINX, config edits, hosting | Code and an API key | Install an app |
| Scope | One tab | Only the blocked site's traffic | Whatever you host | Your scripts and jobs | Entire device |
| Login pages | Not supported | Not stated | Not stated | Session support for multi-step flows | Works normally |
| Streaming | Often blocked by the platform | Claimed by some vendors, unverified | Depends on your proxy config | Not its purpose | Usually works |
| Cost | Free | Free | Free software, paid in time and hosting | Per credit, from $49/mo | Paid |
| Suits | One page, one time | Casual single-site access | Privacy-minded tinkerers | Developers at volume | Whole-device privacy |

The self-hosted column deserves a caveat. Holy Unblocker LTS supports YouTube, Reddit, Discord, Instagram, GeForce NOW, and Spotify, plus Tor and Onion routing through a SOCKS5 or Tor proxy URL, with configuration flags for DOM masquerading and source randomization. It is also, by its author's own description, a proof of concept with domain restocks handled through a Discord community rather than a guaranteed hosted service. Budget a weekend, not ten minutes.

## Questions that decide the purchase

**Can a free web unblocker open Netflix or a streaming account?**
Rarely. Streaming platforms block proxy IP ranges aggressively, and the honest free services say so. Vendors promising otherwise are making claims you cannot verify before paying.

**Is a free proxy safe for ordinary browsing?**
For public pages, the operators argue yes: traffic between your device and the proxy is encrypted, and Unblock-Web says it does not log traffic. The line is sensitive input. No passwords, no card numbers, no banking. Keep those off any free proxy, this writer's included.

**Do I need ScraperAPI to read one blocked article?**
No. A free proxy or a VPN handles that. An API is for code that needs to fetch thousands of pages that actively resist being fetched.

**How many pages does $49 actually buy?**
Between 3,333 and 100,000 on Hobby, depending on the target. Ordinary pages cost 1 credit, Amazon 5, Google 25, LinkedIn 30, and bot-protected sites add 10 on top. Price your actual URLs with the Domain Cost Estimator before choosing a tier.

**What if I only need global IPs occasionally?**
Business at $299/month unlocks global geotargeting. Below that, Hobby and Startup cover US and EU only, which is fine for most regional e-commerce and news work and useless for anything else.

## Choosing between them

One page, once, right now: use a free proxy or an extension and spend nothing. Be ready for login walls, unsupported domains, and slow evenings.

A script that needs a few thousand pages a month from ordinary sites: Hobby at $49 covers it comfortably, and you should still price your targets first, because a Cloudflare-protected site triples your effective cost per page.

Anything needing IPs outside the US and EU: Business at $299 is the first tier that offers it.

Bursty workloads that exceed a monthly cap: Scaling at $475 adds pay-as-you-go with a spending cap, which is cheaper than permanently buying the next tier up.

Before you pay, confirm three things on the pricing page: the per-URL credit cost for your actual targets, the concurrency your job needs (Hobby's 20 threads is the real constraint for parallel crawlers), and whether annual billing suits a workload whose credits reset at renewal rather than accumulating.

👉 [Open ScraperAPI's pricing page and match a tier to your volume](https://www.scraperapi.com/pricing?fp_ref=coupons)
