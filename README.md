# DMIT Hong Kong speed test: ~15ms latency to mainland on CN2 GIA, T1 plans from $36.9/year

A few weeks back, a friend pinged me at 1am with a single screenshot. It was a ping test from his Shanghai apartment to a Hong Kong server. The number on screen: 14ms. "That's the floor, right?" he asked, half bragging. I told him no, that's actually real, and yes, he was on DMIT's HKG.Pro node.

That little screenshot kicked off my own little deep dive into the **DMIT Hong Kong speed test** rabbit hole. I'd heard the name bounced around VPS forums for years, but never bothered to actually benchmark it myself. So I spent a couple weekends pulling latency numbers, digging through the routing specs, and figuring out which of the three Hong Kong product lines actually does what it claims. Here's the honest version of what I found.

## Why people run a DMIT Hong Kong speed test in the first place

If you've ever hosted anything for users in mainland China, you already know the pain. US West Coast servers sit at 150–200ms on a good day, and during the evening rush (think 8pm to midnight, Beijing time), they balloon to 300ms+ with packet loss that makes SSH feel like dial-up. Hong Kong is geographically the closest major internet hub to mainland China that lives outside the Great Firewall, which is why everyone who's serious about China-facing latency ends up here eventually.

But here's the catch most spec sheets conveniently skip: two servers in the same Hong Kong rack, with identical CPU and RAM, can have wildly different real-world performance to a Shanghai user. It all comes down to **routing**. The cheap international transit route and the premium CN2 GIA route are not the same animal. That's the whole reason a "DMIT Hong Kong speed test" is a thing people search for — they want to know which tier actually delivers the low-latency promise, not just the marketing brochure version.

DMIT runs three product lines out of their Hong Kong data center (Equinix HK2 in Kwai Chung, for those who care about the building):

- **HKG.Pro** — Premium tier. China Telecom via CN2 GIA, China Unicom via AS9929, China Mobile via CMI. All three carriers, optimized.
- **HKG.EB** — Eyeball tier. CMI direct for Mobile users; Telecom and Unicom route through NTT in Japan outbound, return direct. The middle ground.
- **HKG.T1** — Tier 1 international routing via RETN. No China-specific optimization, but you still get Hong Kong's geographic proximity.

Each tier exists for a real reason, and the price gap between them is enormous. Let's actually look at the numbers.

## The actual speed test data — what ~15ms really means

DMIT publishes a reference measurement on their Hong Kong location page: **~15ms average latency to China Mainland, with packet loss below 0.1%**, for the Premium (CN2 GIA) network. They footnote it as a Hong Kong-to-Shenzhen reference, and yes, your real number will swing based on access network, route, and time of day. Fair disclaimer.

But here's the thing — that 15ms figure lines up with what independent testers have been reporting. The commonly cited test IP for HKG.Pro is `103.117.100.2`, and you can ping or MTR it from your origin before you ever spend a dollar. From major Chinese cities (Shanghai, Guangzhou, Beijing), users typically see **20–50ms on HKG.Pro during peak evening hours**, versus **150–200ms from Los Angeles-based CN2 GIA nodes**. That's the geographic advantage, and it's not subtle.

The Tier 1 test IP `154.12.176.2` tells a different story. Same Hong Kong building, same AMD EPYC hardware, but on RETN international routing instead of CN2 GIA. Latency to mainland China is meaningfully higher, and packet loss creeps up during evening congestion windows. You're paying for Hong Kong proximity without paying for the China-optimized pipes.

HKG.EB sits in between. China Mobile users get genuinely good CMI direct routes both directions. Telecom and Unicom users eat an NTT hop through Japan on the outbound, which adds a few ms but keeps the price manageable. If your audience skews Mobile-heavy, EB is the value play.

## DMIT Hong Kong plans: side-by-side comparison

Here's the full plan breakdown I pulled together from the official pricing pages. Prices are before promo codes — and yes, the promo codes matter, so keep reading.

**HKG.Pro — Premium CN2 GIA + AS9929 + CMI (monthly billing)**

| Plan | vCPU | RAM | Storage | Traffic | Bandwidth | Price | Purchase |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HKG.AS3.Pro.STARTER | 1 Core | 2 GB | 40 GB SSD | 1000 GB BIDI | 1 Gbps | $79.90/mo | [ Get HKG.Pro STARTER](https://www.dmit.io/aff.php?aff=13832&pid=155) |
| HKG.AS3.Pro.MINI | 2 Cores | 2 GB | 60 GB SSD | 1500 GB BIDI | 1 Gbps | $119.90/mo | [ Get HKG.Pro MINI](https://www.dmit.io/aff.php?aff=13832&pid=156) |
| HKG.AS3.Pro.MICRO | 4 Cores | 4 GB | 80 GB SSD | 2000 GB BIDI | 1 Gbps | $159.90/mo | [ Get HKG.Pro MICRO](https://www.dmit.io/aff.php?aff=13832&pid=157) |

> Apply `202510_HKG_TYO_PRO_20OFF_RECURRING` at checkout for **20% off recurring** on quarterly or annual billing. That brings the STARTER down to roughly $63.92/month if you commit to a quarter or longer.

**HKG.EB — Eyeball (CMI + NTT paths, monthly billing)**

| Plan | vCPU | RAM | Storage | Traffic | Bandwidth | Price | Purchase |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HKG.EB.STARTER | 1 Core | 1 GB | 20 GB SSD | 1000 GB | 1 Gbps | ~$39.90/mo | [ Get HKG.EB STARTER](https://bit.ly/DMIt) |
| HKG.EB.MINI | 2 Cores | 2 GB | 40 GB SSD | 2000 GB | 1 Gbps | ~$59.90/mo | [ Get HKG.EB MINI](https://bit.ly/DMIt) |
| HKG.EB.MICRO | 2 Cores | 4 GB | 60 GB SSD | 4000 GB | 10 Gbps | ~$99.90/mo | [ Get HKG.EB MICRO](https://bit.ly/DMIt) |

> EB inventory moves fast and occasionally sells out during promo windows. If the plan you want is in stock, grab it.

**HKG.T1 — Tier 1 international routing (annual billing)**

| Plan | vCPU | RAM | Storage | Traffic | Bandwidth | Annual Price | Purchase |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HKG.T1.WEE | 1 Core | 0.5 GB | 10 GB SSD | 500 GB | 10 Gbps | $36.9/yr | [ Get HKG.T1 WEE](https://www.dmit.io/aff.php?aff=13832&pid=152) |
| HKG.T1.STARTER | 1 Core | 1 GB | 20 GB SSD | 1000 GB | 10 Gbps | $67.9/yr | [ Get HKG.T1 STARTER](https://www.dmit.io/aff.php?aff=13832&pid=153) |
| HKG.T1.MINI | 2 Cores | 2 GB | 40 GB SSD | 2000 GB | 10 Gbps | $99.9/yr | [ Get HKG.T1 MINI](https://www.dmit.io/aff.php?aff=13832&pid=154) |

> Two codes worth knowing on T1: `HKG-T1-ANNUALLY-45OFF-RECUR` knocks **45% off recurring** on annual STARTERv2 or higher tiers **and** upgrades your specs (more vCPU, double the disk, 50% more RAM, better I/O). And `202510_HKG_TYO_T1_30OFF_RECURRING` gives **30% off** on quarterly or longer billing, excluding the WEE tier.

## The promo code that's actually a different product

I want to call out the `HKG-T1-ANNUALLY-45OFF-RECUR` deal specifically, because it's not a typical discount. It's 45% off the annual price **plus** a spec bump — more vCPU, double the storage, 50% more memory, and better I/O performance. Functionally, you're buying a different, better product at a lower price.

After the discount, the WEE tier effectively drops to around $20/year for a 10Gbps port Hong Kong VPS. That's not a typo. It's the cheapest legitimate Hong Kong VPS I've come across that runs on real AMD EPYC hardware with native IPv4 + IPv6 /64. If you've been curious whether Hong Kong hosting even works for your use case, this is the sandbox ticket.

One honest heads-up: the upgraded T1 v2 plans are currently in alpha testing on a distributed storage architecture, and DMIT explicitly notes there's **no SLA guarantee on data loss or temporary IO interruptions** during this phase. Back up your data. If that risk bothers you, the stable T1 series is still available at standard pricing without the upgrade.

## How to actually run your own DMIT Hong Kong speed test

Before you buy anything, do the sensible thing — test from your own location. DMIT publishes test IPs so you can benchmark before spending a cent.

- **HKG.Pro Premium test IP:** `103.117.100.2`
- **HKG.T1 Tier 1 test IP:** `154.12.176.2`

From a shell in your origin region, run:

bash
ping -c 20 103.117.100.2
mtr -r -c 100 103.117.100.2


The `mtr` output is the more useful one — it shows you hop-by-hop latency and packet loss, so you can see exactly where the route enters China Telecom's CN2 GIA backbone (AS4809 / AS23764). If you see the path sticking to CN2 GIA end-to-end with no NTT or RETN detours, that's the Premium tier doing what you're paying for.

For benchmarking throughput, most DMIT users run `iperf3` against a public iperf server in their target region after provisioning. The 1Gbps and 10Gbps port numbers DMIT lists are peak VirtIO interface rates — real-world throughput depends on the international path between you and the Hong Kong node.

## What happens when you hit the monthly traffic cap

This is one of the things I genuinely appreciate about DMIT: they don't cut you off. When you exhaust your monthly transfer quota, they **throttle your bandwidth** rather than killing the server or hitting you with overage fees. The T1 WEE throttles to around 50Mbps; most other plans settle at 100Mbps. Your service stays alive, your SSH session stays open, you just don't get full port speed until the next monthly reset.

For most use cases — websites, APIs, dev environments — that's perfectly fine. If you're running something that genuinely needs sustained high throughput past the quota, look at the unmetered options on the LAX side, or size up to a higher T1 / EB / Pro tier from the start.

## Which tier should you actually buy?

Let me make this concrete, because the comparison tables alone won't tell you which one fits your situation.

**If your users are mostly in mainland China and latency actually matters** — websites, live streaming, game servers, cross-border SaaS — go **HKG.Pro**. The CN2 GIA + AS9929 + CMI trifecta is the whole point. Use `202510_HKG_TYO_PRO_20OFF_RECURRING` on quarterly or annual billing to knock 20% off recurring. Yes, $79.90/month for the STARTER is real money. The peak-hour performance gap between Pro and T1 is equally real, and you'll feel it the moment your Chinese users hit your site at 9pm.

**If your audience is mixed China + international, or skews Mobile-heavy** — go **HKG.EB**. China Mobile users get a genuinely good experience on CMI direct both ways. The NTT hop on Telecom/Unicom outbound adds a little latency but keeps the price around half of Pro. Good middle ground for blogs, SaaS backends, and download mirrors with moderate China traffic.

**If your users aren't primarily in mainland China** — go **HKG.T1**. You still get Hong Kong's geographic benefit (relevant for Southeast Asia, Taiwan, Japan routes), and the `HKG-T1-ANNUALLY-45OFF-RECUR` deal makes the annual pricing genuinely competitive. The WEE at $36.9/year is the entry point worth trying first if you just want to kick the tires on Hong Kong hosting.

## A few things worth knowing before you commit

**IP replacement policy:** If your IP gets blocked by the Great Firewall, DMIT lets you request a free replacement once every 15 days. After that it's $5 per change. Clear, documented, more than most providers offer.

**Refund window:** 3-day money-back guarantee (up to 30 GB data transfer used), plus a 30-day prorated refund option. Enough runway to run real benchmarks on the routes you care about before committing long-term.

**Hardware across the board:** AMD EPYC processors (AN5 = EPYC 9005 "Turin" with DDR5, AN4 = EPYC 9004 "Genoa", AS3 = EPYC 7003 "Milan"), all-NVMe SSD storage, KVM virtualization, native IPv4 + IPv6 /64 per instance. No overselling — DMIT is explicit about this, which is why peak-hour latency doesn't degrade the way it does on oversold budget hosts.

**Payment options:** Credit card, PayPal, Alipay, WeChat Pay, and crypto. The Alipay and WeChat options are genuinely useful if you're in China and don't want to fight international card friction.

**SSH defaults:** Key-based authentication out of the box, which is more secure but catches some users off guard. If you want password login, you'll need to flip it manually in the control panel.

## The bottom line on the DMIT Hong Kong speed test

After spending real time with the numbers, the short version is this: the ~15ms-to-mainland claim on HKG.Pro is not marketing fluff. It's a reference measurement to Shenzhen, and from major Chinese cities you'll typically see 20–50ms during peak evening hours, which is a meaningful, perceptible improvement over anything US-hosted. The Tier 1 test IP tells you what you give up when you don't pay for CN2 GIA — and the price gap between the tiers reflects that honestly.

If you're serving mainland China users and latency matters, HKG.Pro is the tier that earns its price tag. If you just want a Hong Kong presence without paying for China-optimized routing, T1 with the annual 45% off code is one of the better Hong Kong VPS values currently on the market.

Either way, run your own speed test against `103.117.100.2` and `154.12.176.2` from your actual user locations before you buy. The numbers I've shared here are reference points — your routes, your times of day, your mileage will vary. That's the whole reason the test IPs exist.

👉 [Browse all DMIT Hong Kong plans and check current availability](https://bit.ly/DMIt)

👉 [Grab the HKG.T1 WEE at $36.9/year with the 45% off annual code](https://www.dmit.io/aff.php?aff=13832&pid=152)

👉 [Step up to HKG.Pro STARTER for CN2 GIA premium routing](https://www.dmit.io/aff.php?aff=13832&pid=155)

*Pricing, promo codes, and plan availability reflect information available as of early 2026 and may change. High-demand Hong Kong plans sell out during promotional windows — verify current specs and stock on the official site before purchasing.*
