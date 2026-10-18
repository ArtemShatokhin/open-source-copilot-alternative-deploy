# Leaving Microsoft Copilot for Kortix: the open-source route's real cost

Is Microsoft Copilot free? Copilot Chat is: it comes with eligible Microsoft 365 plans at no additional cost; the full product is licensed per person, per month. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. Side by side, the shape differs before the numbers do. Microsoft charges per seat on an annual clock; the open-source route charges for a machine you point at the internet, plus one afternoon to stand it up.

## What Microsoft's pricing page documents

Microsoft's [plans and pricing page](https://www.microsoft.com/en-us/microsoft-365-copilot/pricing) splits the lineup into individual, business, enterprise, and Copilot Studio tabs (checked October 18, 2026). The business tab prices Copilot two ways. The bundled plans are Microsoft 365 Business Standard with Copilot at $23.50 per person, per month, paid yearly, and Microsoft 365 Business Premium with Copilot at $32.00; month to month, those run $28.20 and $38.40.

The add-on suits teams already on Microsoft 365. Microsoft 365 Copilot Business starts from $18.00 per person, per month, paid yearly, down from a $21.00 list price under a promotion Microsoft dates July 1 through December 31, 2026, first year only, annual commitment required. Month to month: $25.20. The footnotes add two: each licensed person needs a qualifying Microsoft 365 plan, capped at 300, and the one-month free trial converts into a paid subscription. The [Is Microsoft Copilot free?](https://opensourcecopilotalternative.com/is-microsoft-copilot-free) guide prices every Copilot in the lineup.

## What staying costs beyond the price list

The per-seat fee is the visible half; three documented facts make up the rest. The meter counts people: each seat rides on a qualifying Microsoft 365 plan for that person, so the Copilot line multiplies a base you already pay. The clock is annual, and month-to-month freedom costs 20 percent more on every plan priced both ways. The exit has a price: cancel a subscription and the page states all associated data will be deleted, with certain cancellations carrying a fee.

Then there is the part no footnote prices. The agents you build, the connectors you wire, and the grounding Microsoft's Work IQ layer assembles live in Microsoft's tools, and Microsoft's pages document no export into another stack; the [Kortix vs Microsoft Copilot](https://opensourcecopilotalternative.com/kortix-vs-copilot) comparison covers the lock-in dimension by dimension. Copilot's depth inside Microsoft 365 is real; the cost of staying is the fee plus a per-person meter, an annual clock, and work that does not travel.

## What the open-source route costs to stand up

This repository deploys one of the [Microsoft 365 Copilot alternatives](https://opensourcecopilotalternative.com/microsoft-365-copilot-alternatives). Its setup is the shortest cost line here. Kortix's README publishes it as three commands ([Kortix on GitHub](https://github.com/kortix-ai/suna)):

```bash
curl -fsSL https://kortix.com/install | bash
kortix init
kortix ship
```

The first command installs the CLI. The second, `kortix init`, scaffolds a project: `kortix.yaml`, your agents, skills, and runtime config. The third, `kortix ship`, pushes the repo live in Kortix Cloud. The self-hosted path swaps the third command: `kortix self-host start` runs the full stack on your own box as one Docker Compose stack, and the same file runs on a laptop, a VPS, or a cloud VM. Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free.

The afternoon is real, and it has a checklist. This repository's walkthrough lists the prerequisites: a Linux host with Docker, 8 GiB of RAM to start, a domain with two DNS records, ports 80 and 443 open, a sandbox provider key for the agent sessions, and model credentials. Point the DNS, run the commands, connect GitHub in the dashboard, add a key, start a session. The software costs nothing. Open source. Read it, fork it, audit it.

## What it costs to keep running

Two line items, both sized by you. The host bills whatever your provider charges; nothing about Kortix changes that number. Model spend is the second item. Kortix takes three forms of payment. Any model provider with your own keys. Or the ChatGPT plan you already pay for. Or sign in with your OpenCode Console account for OpenCode Zen and Go. Credits pay for the cloud computers sessions run on, per second while a session works, nothing while it stops. On your own keys, the first $1,000 a month of model usage carries no platform fee (kortix.com/pricing, checked October 18, 2026).

Teams that would rather not run a host can stop at the managed cloud. Free is $0 with 200 credits per month and 1 project. Paid plans start at $50 a month for 5,000 credits, and members are free, sharing one credit pool. Microsoft meters by person; the tenth member costs the same as the first.

## The two bills, added up

Staying buys Copilot's depth inside Microsoft 365, and it renews a meter that counts people on an annual clock. Leaving buys an agent layer you own: one afternoon of setup, a host you already know how to run, and model keys you probably hold. Teams still weighing the two incumbents can read [Microsoft Copilot vs Gemini for Work](https://opensourcecopilotalternative.com/microsoft-copilot-vs-gemini-for-work); when the open-source route wins, [Open Source Copilot Alternative](https://opensourcecopilotalternative.com) keeps the pricing, the comparison, and the deployment walkthrough in one place.
