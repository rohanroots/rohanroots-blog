---
title: "I Asked an AI Agent to Buy Me a VPS"
description: "It compared providers, picked a $35.99-a-year server, filled out the checkout, then got stuck on two password fields."
pubDate: 2026-09-13
canonical: "https://rohanroots.substack.com"
---

**FIELD NOTES ON PRACTICAL AI AGENTS**

*It compared providers, picked a $35.99-a-year server, filled out the checkout, then got stuck on two password fields.*

**I wanted a small always-on server for an AI agent and infrastructure experiments. Instead of opening fifteen tabs and building another comparison spreadsheet, I gave Muse the job. What followed felt less like chatting with a bot and more like supervising a very fast junior operator.**

| **PURPOSE** | Hermes Agent + server lab |
| --- | --- |
| **PROVIDER** | RackNerd |
| **PLAN** | 2 GB KVM VPS |
| **REGION** | New York, USA |
| **OPERATING SYSTEM** | Ubuntu 24.04 |
| **PRICE** | $35.99 per year |

## A one-line brief became a real purchase

The request was deliberately simple: find a budget VPS in the United States with at least 2 GB of memory, able to run Nous Research's Hermes agent around the clock. Some latency was fine. Cost mattered more than prestige.

That's the kind of job an agent should be good at. The goal is concrete, the constraints are visible, and a wrong pick is recoverable. This wasn't a production database or a six-figure cloud commitment. It was a small server that could become a durable sandbox for containers, networking, automation, system services, and whatever I wanted to break next.

Muse didn't just return a list of providers. It asked the questions that changed the answer: where am I located, how much memory do I need, does latency matter, do I want monthly flexibility or the lowest annual price. Once those were settled, the comparison narrowed fast.

## Why RackNerd won

It came down to RackNerd versus Hetzner. Hetzner is the stronger cloud platform: polished tooling, predictable infrastructure, hourly billing, a better path if the workload ever becomes important. But its best prices were in Europe, and I wanted a US location.

RackNerd's annual special was the more pragmatic lab machine: 2 vCPUs, 2 GB of memory, 35 GB of storage, a dedicated IPv4 address, in New York, for $35.99 a year. That's roughly $3 a month. For an experiment box running a remote-model agent, it was enough machine at a price low enough to just leave running.

The useful part of the recommendation was the trade-off, stated plainly. I was buying the cheapest suitable box, not the cheapest platform I'd trust with revenue. Different purchases.

**Finding a cheap VPS was the easy part. The useful part was carrying the decision from a vague intent all the way to a checkout-ready configuration.**

## Checkout worked, until two boxes looked identical

After I confirmed the plan, Muse moved into execution. It set up the RackNerd account, selected New York, chose Ubuntu 24.04, carried the billing details into checkout, and paid through Stripe Link. I kept final approval over the charge, which is exactly where I wanted the human checkpoint.

Then the whole thing stalled on the least glamorous problem imaginable: the Password and Confirm Password fields looked identical to the automation. Autofill couldn't tell them apart. I'd type the password, the page would clear it during validation, and we'd do the dance again. The agent had compared providers and reasoned through infrastructure trade-offs, and then two ordinary HTML inputs became the hard part.

The workaround was the site's own password generator. That removed the ambiguous handoff, the form validated, and the order went through. The fix was mundane. The failure was the interesting part: agentic workflows rarely break on hard reasoning. They break at the seams between systems. Duplicated labels, session boundaries, CAPTCHAs, one-time codes, a field that doesn't expose enough structure.

## Payment succeeded, and I still checked it twice

Minutes later the emails started arriving: welcome, order confirmation, invoice, payment receipt, server login details. So many messages after an automated checkout raised the obvious question: had I been charged twice?

Muse found one completed $35.99 payment. The rest was the normal paper trail for a single order. That verification mattered as much as the purchase.

## The right model is delegation with checkpoints

This changed how I think about consumer AI agents. The value wasn't a fully autonomous system running without me. It was simpler: dozens of small tasks, compressed. Gathering constraints, comparing offers, checking locations, turning requirements into a specific config, filling routine fields, keeping the whole decision in one conversation.

I stayed responsible for the parts that mattered. I approved the provider, the annual commitment, and the payment. Credentials moved through secure handoffs instead of chat. When the interface turned ambiguous, I stepped in. That division of labor felt less futuristic than the marketing, and much more useful.

- **Delegate bounded research.** Give the agent a goal, a budget, minimum specs, a location, and the trade-offs you'll accept.
- **Keep approval at the edge.** The agent can prepare a purchase. You confirm the final config, price, renewal terms, and the charge.
- **Expect interface friction.** A workflow can fail on a plain form after getting all the reasoning right. A clean manual handoff is a feature, not a defeat.
- **Verify the outcome.** Check the receipt, the account, the service details, and the payment history before calling it done.
- **Start with recoverable tasks.** A cheap lab server is a better first autonomous purchase than a production system.

## The purchase is only the beginning

Now the real work moves to the server: SSH in, create a non-root admin, switch to key-based auth, set up the firewall, turn on unattended security updates, and run Hermes under systemd so it survives reboots. After that I might host this blog on the same box, which would make the infrastructure behind this story the subject of the next one.

There's a satisfying loop in that. An AI agent helped buy the server that will host another AI agent, and the bumps along the way made a better story than a frictionless demo would have. The password-field failure wasn't something to hide. It was the most honest part.

## My takeaway: agents are already practical, just not invisible

Muse didn't remove judgment, and it didn't turn a messy commercial website into a clean API. It removed most of the coordination cost between "I should get a server" and "the server is ordered." That's real progress.
