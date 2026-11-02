# Best VPS for n8n: How Much RAM You Actually Need, Real 3-Year Costs, and the Plans Worth Buying

Self-hosting n8n doesn't need a big server — it needs the right one. The official docs are blunt about it: memory matters more than CPU ("memory needs supersede CPU needs" is their actual wording). n8n idles at around 320 MB, but a running workflow can spike to 1–2 GB, and the database holding your credentials, execution history, and workflow definitions needs SSD storage. A single vCPU is genuinely enough for most people.

That combination — modest CPU, real RAM headroom, fast disk — is exactly where cheap VPS providers get tricky. Some oversell CPU cores and give you 1 GB of RAM. Others bury an n8n-friendly price behind a 48-month commitment. This guide walks through what to actually look for, breaks down RackNerd's full current lineup (annual specials from $21.99/year), does the 3-year cost math that most comparison posts skip, and flags the limitations nobody puts in the comparison tables.

> **Quick pick:** for most self-hosters, the 2 GB KVM plan at **$35.99/year** (about $3/month) is the sweet spot — enough RAM for n8n plus a PostgreSQL backend, with 5 TB of monthly traffic. 👉 [Grab the 2 GB KVM plan for $35.99/year](https://my.racknerd.com/aff.php?aff=11397&pid=953). If you want a managed, one-click setup instead and don't mind paying roughly three times as much, Hostinger's KVM 2 tier with its n8n template is the usual alternative.

## What n8n actually needs from a VPS

RAM first, disk second, CPU last. Here's the breakdown from n8n's own documentation and deployment guidance:

- **Memory:** 320 MB idle, 1–2 GB peak per running execution. That means a 1 GB box runs n8n, but a single heavy workflow can squeeze it. 2 GB is the comfortable floor.
- **Storage:** SSD, and this matters more than people realize. n8n's database stores credentials, past executions, and workflow definitions — slow disk is the number one cause of a sluggish editor UI. Plan for 512 MB–4 GB of database growth depending on how much execution history you keep.
- **CPU:** one vCPU handles typical workloads. Extra cores only start to matter when you turn on queue mode with multiple workers.
- **Database:** SQLite is fine for personal use. Once you're running n8n for real work, the official recommendation is PostgreSQL, which needs roughly another 512 MB–1 GB of RAM headroom on the same box.

A practical sizing rule: under ~100 executions per day with small payloads, 2 GB works. Dozens of scheduled workflows with Postgres on the same machine, 4 GB. High webhook volume or AI agent workflows with queue mode, 8 GB.

## RackNerd's full plan lineup and pricing

RackNerd sells KVM VPS with SolusVM, full root access, and RAID-10 SSD storage across roughly 21 datacenters. Every plan includes a 1 Gbps port — it's not gated behind a premium tier — and one IPv4 address. Provisioning is effectively instant after payment, and upgrades between plans take about a minute of reboot downtime with no migration.

The annual specials are where the prices get attention-grabbing. These five plans make up the current specials lineup:

| Plan (RAM / vCPU) | SSD Storage | Monthly Traffic | Price | Order |
| --- | --- | --- | --- | --- |
| 1 GB KVM — 1 vCPU | 20 GB | 3 TB | $21.99/year | [Buy the 1 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=952) |
| 2 GB KVM — 2 vCPU | 35 GB | 5 TB | $35.99/year | [Buy the 2 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=953) |
| 4 GB KVM — 3 vCPU | 60 GB | 7 TB | $59.99/year | [Buy the 4 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=954) |
| 6 GB KVM — 6 vCPU | 100 GB | 12 TB | $89.99/year | [Buy the 6 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=955) |
| 8 GB KVM — 7 vCPU | 150 GB | 20 TB | $119.99/year | [Buy the 8 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=956) |

If you'd rather pay monthly — sensible when you're still testing whether self-hosting suits you — there's a separate line of standard KVM plans billed monthly:

| Plan (RAM / vCPU) | SSD Storage | Monthly Traffic | Price | Order |
| --- | --- | --- | --- | --- |
| 1 GB KVM — 2 vCPU | 50 GB | 1 TB | $17.99/mo | [Order monthly 1 GB](https://my.racknerd.com/aff.php?aff=11397&pid=20) |
| 2 GB KVM — 3 vCPU | 75 GB | 2 TB | $20.59/mo | [Order monthly 2 GB](https://my.racknerd.com/aff.php?aff=11397&pid=21) |
| 4 GB KVM — 4 vCPU | 130 GB | 3 TB | $24.59/mo | [Order monthly 4 GB](https://my.racknerd.com/aff.php?aff=11397&pid=22) |
| 6 GB KVM — 5 vCPU | 170 GB | 4 TB | $27.59/mo | [Order monthly 6 GB](https://my.racknerd.com/aff.php?aff=11397&pid=23) |
| 8 GB KVM — 6 vCPU | 220 GB | 5 TB | $36.59/mo | [Order monthly 8 GB](https://my.racknerd.com/aff.php?aff=11397&pid=24) |
| 12 GB KVM — 7 vCPU | 300 GB | 6 TB | $55.99/mo | [Order monthly 12 GB](https://my.racknerd.com/aff.php?aff=11397&pid=25) |

One thing worth knowing about the pricing itself: no coupon codes are involved for VPS. The specials page *is* the price. (RackNerd does run promo codes, but they apply to dedicated servers, not VPS plans.) There are also smaller 512 MB annual plans and AMD Ryzen NVMe variants on the monthly side — the 512 MB tiers are too tight for n8n once you add a database, so I'd skip them for this use case.

## What n8n costs over three years (the math most comparisons skip)

Sticker prices across providers aren't directly comparable, because billing cycles and renewal policies differ wildly. Here's the same calculation run consistently — first year and three-year total for an n8n-capable plan:

| Provider & plan | Year 1 | 3-year total | Renewal behavior |
| --- | --- | --- | --- |
| RackNerd 2 GB KVM (annual) | $35.99 | ~$108 | Same as first year |
| Hostinger KVM 2 (48-mo promo, $8.99/mo) | ~$107.88 | Renewal roughly doubles | Jumps at renewal |
| Hetzner CX22 ($4.59/mo) | ~$55 | ~$165 | Stable, but 4 GB shared-CPU tier |
| Hetzner CCX13 ($15/mo) | ~$180 | ~$540 | Stable, dedicated cores |
| DigitalOcean Premium AMD ($24/mo) | ~$288 | ~$864 | Stable, pay-as-you-go |

The pattern is hard to miss. Over three years, the RackNerd 2 GB plan costs about what one year of Hostinger KVM 2 costs at its promo rate — and Hostinger's renewal roughly doubles after the initial term. RackNerd's renewal price matches the first year, which is why long-term users sometimes call these "grandfathered" plans: the rate you sign up at is the rate you keep.

The trade-off is what you give up, which I'll cover in the limitations section. But purely on cost-per-GB-of-RAM for a self-hosted automation box, the annual specials are in a different league — the 2 GB plan works out to about $1.50 per GB of RAM per month, and even the 8 GB plan at $119.99/year is under $10/month for specs that would run $24+ monthly at DigitalOcean.

If the annual commitment gives you pause, the monthly KVM plans exist precisely for that: two or three months at $20.59/month tells you whether n8n self-hosting fits your workflow, then you can switch to the annual special and upgrade in place. 👉 [Browse the full specials lineup before deciding](https://bit.ly/RacKnerd).

## How RackNerd compares with the usual n8n hosting picks

No single VPS is right for every n8n deployment, so here's the honest comparison. All figures below are from current published plan pages and recent third-party reviews:

| Provider & plan | vCPU | RAM | Traffic | Price/mo | Where it wins |
| --- | --- | --- | --- | --- | --- |
| RackNerd 2 GB KVM | 2 | 2 GB | 5 TB | ~$3 (billed yearly) | Lowest 3-year cost, renewal never rises |
| Hostinger KVM 2 | 2 | 8 GB | 8 TB | ~$8.99 (promo term) | One-click n8n template, easiest setup |
| Hetzner CX22 | 2 shared | 4 GB | 20 TB | $4.59 | Cheap EU/US capacity, strong disk I/O |
| Hetzner CCX13 | 2 dedicated | 8 GB | 20 TB | ~$15 | Production-grade dedicated cores |
| DigitalOcean Premium AMD | 2 | 4 GB | 4 TB | $24 | Managed Postgres, n8n marketplace droplet, 13+ regions |
| Contabo Cloud VPS S | 4 | 8 GB | 32 TB | ~$6.99 | Most raw specs per dollar |

Reading that table honestly:

- **Hostinger** is the pick if you've never touched a terminal. Its n8n template gets Docker and SSL configured in minutes, no commands required. You pay for that convenience in renewal pricing.
- **DigitalOcean** wins when you expect to outgrow a single box — managed PostgreSQL, snapshots, and object storage mean you can split components without rebuilding anything. It's the most expensive option here by a wide margin.
- **Hetzner** has the best disk and network performance per dollar, especially the dedicated-core CCX line. Support is ticket-based and European-hours-leaning, with no formal uptime SLA, and US datacenters get 1 TB of traffic on some plans rather than the headline 20 TB.
- **Contabo** gives you the most specs, with caveats: provisioning can take hours, support is slow, and its control panel shows its age. Fine for setups where downtime isn't costly.
- **RackNerd** owns the budget-annual niche. Raw per-year cost is unmatched in this set, the renewal-price guarantee is real, and payment options (Alipay, PayPal, cards, crypto) are broader than most. What you don't get: managed anything, a one-click installer, or premium routing.

## Limitations to know before you order

These are the details that live in FAQs and third-party reviews rather than marketing pages, and they're the reason RackNerd isn't automatically the answer for everyone:

1. **Specials plans are non-refundable.** The annual special pricing comes with no refund window. The monthly plans are your risk-free testing ground if you're unsure.
2. **Routing is ordinary international bandwidth, not premium lines.** RackNerd uses Noction IRP for route optimization, but there's no CN2 GIA-style premium transit. If you're serving users in mainland China, expect 160–200ms latency from the West Coast locations and some evening-peak fluctuation. That's usually irrelevant for n8n talking to APIs, but it matters if your workflows serve latency-sensitive users directly.
3. **IP changes have a cost after day one.** If your IP gets blocked within the first 72 hours, RackNerd replaces it free once. After that, each change runs $3 — doable via the panel or a ticket.
4. **Traffic overages throttle instead of billing.** Exceed your monthly transfer and the port slows down rather than your invoice growing. For n8n, whose traffic is usually tiny API calls, 3–5 TB monthly is far beyond what most automation setups consume.
5. **No one-click n8n installer.** You'll SSH in and run Docker commands yourself (template below). It's a 15-minute job if you're comfortable with Linux, a genuine obstacle if you're not — and the honest reason to pick Hostinger instead.

There's one underrated perk worth mentioning: Los Angeles and France locations offer up to 100 free IPv6 addresses on request via a support ticket. If you ever run separate n8n instances per client, that's a quietly useful feature.

## Deploying n8n on a RackNerd VPS

The officially recommended path is Docker, and it works identically on any RackNerd plan since you get full root and can install Debian, Ubuntu, AlmaLinux, Rocky, or your own ISO. The short version:

1. Order a plan, pick your OS (Ubuntu LTS is the safe default), and note the root password from the provisioning email — it arrives within minutes of payment.
2. SSH in, install Docker, then pull the official image: `docker pull docker.n8n.io/n8nio/n8n:latest`
3. Run it with a persistent volume mounted at `/home/node/.n8n` (this holds your database and encryption key — back it up) and expose port 5678.
4. Put Caddy or Traefik in front for automatic HTTPS. n8n behind plain HTTP is functional for testing but not something you want handling webhook credentials long-term.
5. When you outgrow SQLite, add PostgreSQL and point n8n at it with the `DB_TYPE` and `DB_POSTGRESDB_HOST` environment variables. A 2 GB box handles n8n plus Postgres comfortably.

The 4 GB tier is where queue mode enters the picture: main process, Redis, and separate worker containers all fit on one box. Beyond roughly 50 concurrent executions or 20 GB of Postgres data, the better move is splitting components across two smaller machines rather than scaling one box up — memory and disk contention isolated beats a bigger shared box. If that's your trajectory, 👉 [the 4 GB plan at $59.99/year](https://my.racknerd.com/aff.php?aff=11397&pid=954) is the natural starting point, with the 1-minute in-place upgrade as your safety net if you misjudge it.

## FAQ

**Which datacenter should I choose?** During checkout, promo plans default to "Multiple Locations" — you need to actively pick one from the dropdown. For workloads touching Asian APIs or users, Los Angeles DC-03, San Jose, or Seattle are the low-latency picks; Los Angeles DC-02 is available on 2 GB and larger plans. For European targets, Amsterdam or France. Popular locations sell out first, so if a specific one shows unavailable, either wait for restock or pick the next-closest.

**Does the renewal price go up?** Not on the annual specials — renewal matches your first year, and you can generate the renewal invoice early from the client area to lock it in. This is the single biggest cost difference versus promo-priced competitors.

**Can I pay with Alipay or PayPal?** Both, plus WeChat Pay, major credit cards, crypto, and bank wire. Registration from your regular network IP is recommended to avoid triggering a manual verification review.

**Is 1 GB of RAM enough for n8n?** Barely, and only for light use — a few simple scheduled workflows with SQLite, no heavy payloads. The editor UI gets sluggish under load. The extra $14/year for the 2 GB plan is the cheapest reliability you'll ever buy for this stack.

**How reliable is it in practice?** RackNerd has made the Inc. 5000 list, and Trustpilot feedback skews positive, with a recurring theme captured by one mid-2026 review: prices this low paired with support replies this fast. Complaints cluster around evening-peak speed fluctuations on China routes and popular locations going out of stock — real issues, but neither affects a typical n8n deployment much.

## Which plan to actually pick

- **Learning n8n or running a handful of personal automations:** 1 GB at $21.99/year, SQLite, done.
- **Anything you'd be annoyed to lose** — client webhooks, business automations, Postgres: 2 GB at $35.99/year. This is the plan most people should stop scrolling at.
- **Queue mode, AI agent workflows, or n8n sharing a box with other services:** 4 GB at $59.99/year, or 8 GB at $119.99/year if webhook volume is serious.
- **Still deciding:** a monthly 2 GB plan for a couple of months, then migrate to the annual special once you're sold.

And if you've read this far and your conclusion is "I just don't want to manage a server," that's a legitimate answer — a managed host with a one-click n8n template will cost you more per year and be worth it for the hand-holding. For everyone else, 👉 [check RackNerd's current specials and pick your plan](https://bit.ly/RacKnerd) — just double-check the datacenter dropdown before hitting checkout, and remember the annual specials don't come with a refund window.
