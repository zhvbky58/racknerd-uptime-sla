# Is RackNerd reliable? Real uptime, the 99.999% SLA fine print, and why the datacenter you pick matters more than the plan (full price table)

RackNerd's real uptime on a healthy node lands somewhere in the 99.9%–99.99% band, and almost every outage behind that number is a single-hypervisor event rather than a whole datacenter going dark. That makes it a reasonable home for personal sites, DNS servers, VPNs, dev boxes and small production workloads, and a poor fit for anything where one node dying at 3 a.m. costs real money, unless you're running two of them.

Below you'll find every current plan and price, what the 99.999% SLA actually pays out when it's breached (less than most people assume), where the failures cluster, and a rule for choosing a location instead of defaulting to Los Angeles.

> **Quick pick:** most readers end up on the **1 GB KVM VPS Special at $21.99/year**, roughly $1.83/month. Same KVM stack, same 1 Gbps port and RAID-10 SSD as the monthly plans, but 3 TB of monthly transfer instead of 1 TB. 👉 [check the current annual specials and prices](https://bit.ly/RacKnerd)

## All RackNerd VPS plans and prices

Two product lines exist, and the price gap between them is large enough that it decides the purchase on its own. The annual specials are discounted heavily for paying a year up front; the monthly plans cost more per unit of RAM but let you walk away after 30 days.

Prices below are the ones listed on RackNerd's specials and KVM VPS pages at the time of writing. The cart is the source of truth, so confirm the number on the checkout page before you pay.

### Annual VPS specials (billed yearly)

| Plan | Specs | Bandwidth | Price | Order |
| --- | --- | --- | --- | --- |
| 1 GB Special | 1 vCore · 1 GB RAM · 20 GB SSD | 3 TB/mo @ 1 Gbps | $21.99/year | [1 GB annual](https://bit.ly/RacKnerd) |
| 2 GB Special | 2 vCores · 2 GB RAM · 35 GB SSD | 5 TB/mo @ 1 Gbps | $35.99/year | [2 GB annual](https://bit.ly/RacKnerd) |
| 4 GB Special | 3 vCores · 4 GB RAM · 60 GB SSD | 7 TB/mo @ 1 Gbps | $59.99/year | [4 GB annual](https://bit.ly/RacKnerd) |
| 6 GB Special | 6 vCores · 6 GB RAM · 100 GB SSD | 12 TB/mo @ 1 Gbps | $89.99/year | [6 GB annual](https://bit.ly/RacKnerd) |
| 8 GB Special | 7 vCores · 8 GB RAM · 150 GB SSD | 20 TB/mo @ 1 Gbps | $119.99/year | [8 GB annual](https://bit.ly/RacKnerd) |

### Standard KVM VPS (billed monthly)

| Plan | Specs | Bandwidth | Price | Order |
| --- | --- | --- | --- | --- |
| 512 MB | 1 vCore · 512 MB RAM · 30 GB SSD | 500 GB/mo @ 1 Gbps | $26.99/year | [512 MB plan](https://bit.ly/RacKnerd) |
| 1 GB | 2 vCores · 1 GB RAM · 50 GB SSD | 1 TB/mo @ 1 Gbps | $17.99/month | [1 GB monthly](https://bit.ly/RacKnerd) |
| 2 GB | 3 vCores · 2 GB RAM · 75 GB SSD | 2 TB/mo @ 1 Gbps | $20.59/month | [2 GB monthly](https://bit.ly/RacKnerd) |
| 4 GB | 4 vCores · 4 GB RAM · 130 GB SSD | 3 TB/mo @ 1 Gbps | $24.59/month | [4 GB monthly](https://bit.ly/RacKnerd) |
| 6 GB | 5 vCores · 6 GB RAM · 170 GB SSD | 4 TB/mo @ 1 Gbps | $27.59/month | [6 GB monthly](https://bit.ly/RacKnerd) |
| 8 GB | 6 vCores · 8 GB RAM · 220 GB SSD | 5 TB/mo @ 1 Gbps | $36.59/month | [8 GB monthly](https://bit.ly/RacKnerd) |
| 12 GB | 7 vCores · 12 GB RAM · 300 GB SSD | 6 TB/mo @ 1 Gbps | $55.99/month | [12 GB monthly](https://bit.ly/RacKnerd) |

The arithmetic on the 4 GB tier is worth doing once. The annual special is $59.99 for a year. The monthly 4 GB plan is $24.59/month, or about $295 over the same twelve months. Same virtualization, same disk type, roughly five times the cost.

Bare metal starts at $139/month for an Intel Xeon E3-1230 v2 with 16 GB RAM and a 480 GB SSD, and runs up to a 160 TB storage server at $479/month. Each dedicated box includes full root, IPMI/KVM console access and remote reboot, which matters for the reason in the next section.

Upgrades are possible but not instant: adding RAM to a VPS requires a reboot, so plan on about a minute of downtime.

## What the 99.999% SLA actually pays out

RackNerd publishes 99.999% for both network transit and power, measured monthly. On a 30-day month that allows about 26 seconds of downtime. It is a real, written commitment with a credit schedule behind it, not a marketing line.

The credit schedule is where expectations usually diverge from reality:

- Every additional hour of downtime beyond the 99.999% floor earns **2% of that month's fee**.
- The payout is capped at **50% of the month's fee** in total.
- Claims run in one-hour increments and must be filed **within 30 days** of the incident.
- Compensation is **account credit**, never a cash refund.
- Planned and emergency maintenance is excluded, capped at **180 minutes per month**, and announced on the status page in advance.

Run that against the plan most people buy. On the $21.99 annual special, one month of service is worth about $1.83, so the maximum possible credit for a full month of misery is roughly 92 cents. The SLA is not a financial safety net at this price point. It's a formalized apology. Budget-node buyers should treat redundancy as their own job, which is what the rest of this article is about.

If you'd rather see the numbers on the actual order page, 👉 [compare the VPS specials and monthly plans side by side](https://bit.ly/RacKnerd).

## Real uptime: what the numbers mean and what they don't

The honest answer is a range, because every published figure is measuring something different.

Third-party monitoring and multi-month user reports cluster between **99.9% and 99.99%**, with a meaningful share of users hitting something close to 100% across several consecutive months. A long-running Reddit thread on production use includes people reporting two to three years on DNS boxes without an incident, and a three-year-old Black Friday box still running. The same thread has one user who lost a VPS for a full day to a datacenter problem, which they called acceptable given it was one day out of a year.

The variance has a cause. Smaller plans in the 1 GB–4 GB range sit on hosts that are less likely to be hammered by a single noisy neighbor; 8 GB and larger instances occasionally show more fluctuation, which points at busier host nodes rather than worse hardware. Disk benchmarks tell the same story: one user measured about 125 MB/s writes on a "pure SSD RAID-10" node, while others report multi-GB/s figures on different nodes at different times of day. Same product, different neighbors.

One caution about the numbers you'll see quoted elsewhere: a 30-day average of 99.993% works out to roughly three minutes of downtime, not thirty seconds. The 26-second figure applies to 99.999%. If a review converts one into the other, it has the math wrong, and you should discount the rest of its measurements accordingly.

## Outages are node-level, which is why your datacenter matters more than the SLA

Open RackNerd's status page and read the incident titles. They look like this: "Chicago Datacenter OUTAGE - CHI180KVM, CHI184KVM, CHI178KVM," or "Seattle Datacenter OUTAGE - SEA113KVM," or "Dallas Datacenter OUTAGE - 7950XDAL301."

Each one names specific nodes, meaning individual physical hypervisors, not an entire city. RackNerd states on that same status page that around 99% of incidents affect a single hypervisor out of the whole location's infrastructure, leaving most customers in that city untouched.

Two conclusions follow. First, the headline SLA tells you almost nothing about your personal risk, because your risk is concentrated in one box on one host. Second, location selection is a real decision with real consequences:

- **Los Angeles** carries the most traffic and the most node churn, largely because it's the Asia-facing POP. Incidents happen there, but they get fixed quickly.
- **Chicago** has seen a cluster of node-level events recently, close enough together to suggest a rack or switch issue rather than random hardware failure. Equipment was swapped during the cleanup.
- **Seattle and Dallas** are quieter by comparison.
- **European locations** (Amsterdam, London, Strasbourg, Dublin) have been steady, though stock is thinner and you may have to wait for a slot.
- **Singapore** carries shared hosting; VPS inventory appears intermittently.

The rule that comes out of this: pick a location near your users that isn't the busiest POP in the network. If you need Asia latency, Los Angeles delivers it, but Seattle or San Jose typically gets you about 95% of the latency benefit with fewer noisy neighbors and less node churn. For US-central traffic, Dallas or Chicago; Amsterdam or London for Europe; Strasbourg if you specifically want continental Europe. Don't pick the newest location just because it's new. The mature facilities have the longest track record and the most battle-tested transit mix.

## Support, refunds, and the backup gap

Support runs 24/7 through tickets and live chat, and RackNerd advertises an average first response under 10 minutes. In practice, simple requests (reboot, rDNS, OS reinstall, billing questions) get handled cleanly and fast. Network routing anomalies and host-node performance complaints take longer and often need follow-up. During a multi-node incident, the first reply is usually a template acknowledging the investigation, and a human answer can take over an hour. That's a fair trade on a $21.99 annual box. On a $139/month dedicated server, know it going in.

There is one useful thing support can do that isn't advertised: if a node is persistently flaky, ask to be migrated to a different host in the same datacenter. Users report this happening quickly once requested.

**Refunds:** RackNerd's own knowledge base states plainly that there is no money-back guarantee and no refunds under the terms of service. That's the answer, and it's the reason to start with a monthly plan or the cheapest annual special rather than an 8 GB year-up-front commitment. 👉 [start with a small monthly plan before committing to a year](https://bit.ly/RacKnerd)

**Backups:** shared hosting includes free daily backups through JetBackup inside cPanel, retained 30 days. VPS and dedicated servers do not. There is no managed backup option by default, and RackNerd's terms explicitly disclaim responsibility for data integrity. RAID-10 protects against a single drive failing; it does nothing about a bad `rm -rf` or a corrupted database. VPS users typically script file-level backups to S3-compatible storage and, for full images, boot SystemRescue and pull a disk image manually.

Other details worth knowing before checkout: IPv6 is free on request, up to 100 addresses, in Los Angeles and the France location, but you have to open a ticket after ordering. EU customers may see VAT added. And if you let an invoice lapse, expect a late fee rather than a grace period.

On renewals, RackNerd has stated on Reddit that when a promotion ends, pricing for existing customers who bought it stays locked, and that the company has not raised prices on active customers. Treat that as a company statement rather than a contract term, but it's the most direct answer available to the question everyone asks before buying an annual special.

## Is RackNerd reliable enough for production?

It depends on what "production" means to you.

Good fits: a personal blog or static site, a DNS resolver, a WireGuard or Tailscale exit node, a CI runner, a staging environment, a self-hosted app for a handful of users, a small SaaS with one operator who can tolerate an hour of downtime occasionally. For these, $21.99/year for a working KVM box is close to unbeatable, and the node-level failure mode is survivable because nothing critical depends on that specific machine.

Poor fits: a single-database e-commerce store, anything with an SLA you've promised to someone else, real-time workloads where an unexpected reboot breaks state, or a workload running on 8 GB or more where a busy host node will actually show up in your latency graphs. If you're in this group, two small VPS instances in different datacenters joined by WireGuard beats one big instance every time, and bare metal at $139/month gives you a machine you can reboot yourself instead of waiting on a hypervisor.

Performance expectations deserve a caveat. These are unmanaged KVM VPS instances: what you get out of them depends heavily on your OS and software stack. Reviews on the same page swing from 1.5 years of "rock solid" to complaints that a WooCommerce store couldn't load five products. RackNerd's response to the latter is that the customer controls the stack, and that Nginx or OpenLiteSpeed plus a CDN plus tuned queries is the difference. Both things are true at once, and neither is a hardware defect.

## FAQ

**Does RackNerd really have an SLA?** Yes. 99.999% for network transit and power, monthly measurement, with a credit schedule of 2% of the monthly fee per extra hour of downtime, capped at 50%, claimed within 30 days, paid as account credit.

**Will my renewal price jump?** RackNerd says promotional pricing is locked for customers who bought it and that it hasn't raised prices on active customers. Confirm on your own invoice before renewing, since this is a public statement rather than a clause in the terms.

**Which datacenter is most reliable?** The mature US facilities (Los Angeles, San Jose, Dallas, Chicago, New York) have the longest records and the most redundant transit. Among them, Los Angeles is the busiest and Chicago has had a recent run of node-level events. Seattle, San Jose and Dallas tend to be quieter.

**Can I get a refund if I don't like it?** No. RackNerd's knowledge base says there is no money-back guarantee and no refunds under the terms of service.

**Do I get backups?** On shared hosting, yes: daily JetBackup, 30-day retention. On VPS and dedicated servers, no. Back up off-box yourself.

**Can I get IPv6?** Up to 100 addresses free on request in Los Angeles and France. Open a ticket after ordering.

## How to decide

Match the plan to the job and stop overbuying:

- **One small site, a resolver, a VPN endpoint, a dev box** → 1 GB annual special at $21.99/year. Nothing else in the lineup gets close on cost per month.
- **WordPress with a handful of plugins, or a Docker stack with a small database** → 2 GB or 4 GB annual special. The 4 GB at $59.99/year is the value sweet spot if you're committing to a year anyway.
- **Multiple services, a database plus app server, or 4–8 users** → 4 GB monthly to test, then switch to the annual special once you know the workload fits.
- **Anything where an outage has a dollar figure attached** → two 2 GB instances in different datacenters with failover you control, or a $139/month dedicated box.
- **Europe or Asia latency requirements** → Amsterdam or London for Europe, Los Angeles or San Jose for Asia, and skip the newest location in favor of the proven one.

Two things to check on the order page before you pay: the billing term on the plan you selected (the annual specials and the monthly plans sit on the same page and look similar), and whether your preferred datacenter has stock, since European locations in particular run thin. If IPv6 matters, add the ticket request right after checkout rather than a month later.

👉 [pick the RackNerd plan that matches your workload](https://bit.ly/RacKnerd)
