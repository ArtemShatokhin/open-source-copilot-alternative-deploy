# Kortix vs Microsoft Copilot: the open-source AI Operating System, self-hosted

Kortix is the open-source AI Operating System, and it is the system this repository deploys: you run it on hardware you control. Microsoft Copilot is Microsoft's AI assistant for work, and Copilot Studio is Microsoft's separate platform for building custom agents. This comparison covers the four dimensions that decide a rollout: source and ownership, self-hosting, models, and ecosystem lock-in. Every Microsoft fact links to Microsoft's own documentation, checked October 2026.

## What Kortix is

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. The code is open source, so you can read it, fork it, and audit it. Run it on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host is free.

The mechanisms matter more than the label. One isolated sandbox per session, on its own branch. Session work reaches `main` through a change request a person reviews, and merge is default-deny for agents. Connector credentials are brokered server-side and never enter the machine. Per-resource permissions for people and agents, roles, groups, and an audit trail. Approval gates you set, off until you set them. Any model provider with your own keys, or the ChatGPT plan you already pay for, or sign in with your OpenCode Console account for OpenCode Zen and Go.

## What Microsoft Copilot and Copilot Studio are

Copilot is Microsoft's AI assistant for work. Microsoft's own documentation splits it into Copilot Chat, included with eligible Microsoft 365 licenses, and the Microsoft 365 Copilot add-on, which adds automatic grounding in organizational content through Microsoft Graph and Work IQ, with access scoped by each person's permissions ([Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/copilot/microsoft-365-copilot-overview)). Copilot runs as a Microsoft-hosted service you reach through Microsoft's apps and cloud endpoints, at `copilot.cloud.microsoft` and inside Word, Excel, PowerPoint, Outlook, and Teams.

Copilot Studio is the separate Microsoft platform for building and managing custom agents, workflows, and apps. Microsoft describes it as reaching other systems through 1,500+ prebuilt connectors and MCP servers, with application lifecycle management, audit logs that flow into Microsoft Purview, and data residency alignment across major compliance standards ([Microsoft's Copilot Studio page](https://www.microsoft.com/en-us/copilot/products/copilot-studio)). It is sold by consumption: tenant-wide Copilot Credit packs of 25,000 credits at $200 per pack per month, or a pay-as-you-go meter, and a standalone subscription is what unlocks premium connectors and any-channel publishing ([Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-copilot-studio/requirements-licensing-subscriptions)).

Copilot and Copilot Studio are strong inside Microsoft 365, and Microsoft's documentation shows the depth there. The decision comes down to what changes when the agent layer has to run on infrastructure you own.

## The comparison

| Dimension | Kortix | Microsoft Copilot / Copilot Studio |
| --- | --- | --- |
| Open source | Yes | No |
| Self-hosting | Cloud, VPC, or on-prem; self-host is free | Microsoft-hosted service |
| Models | Any provider, your own keys | Models Microsoft selects |
| Configuration | Files in a git repo you own | Managed inside Microsoft's tools |
| Reach | 3,000+ apps, plus MCP, OpenAPI, GraphQL, raw HTTP | 1,500+ prebuilt connectors and MCP servers |
| Pricing shape | Self-host free; cloud $40 per seat per month plus usage | $200 per 25,000-credit pack per month, or pay-as-you-go |
| Where work lands | A change request a person merges | Inside Microsoft 365 apps |

## Source and ownership

Kortix is open source. The code is published, so a security review can read the implementation, and a team can fork it and roll any change back. Microsoft publishes no Copilot source code; Microsoft's documentation describes Copilot as a hosted service reached through Microsoft's apps and endpoints, so there is nothing to read, fork, or audit. That difference decides more than a preference: with Copilot, how a document is retrieved or an answer is formed is checkable only against Microsoft's documentation, and a behavior your team depends on can change in a Microsoft update.

Ownership shows up in where the configuration lives. A Kortix agent is `agents/<name>.md` for behavior, plus an `agents.<name>` block in `kortix.yaml` for governance, both versioned like code in a repo you clone. Copilot agents are built and managed inside Microsoft's interfaces.

## Self-hosting

Kortix runs as one Docker Compose stack, and self-host is free. The same file runs on a laptop, a VPS, or any cloud VM, and agent sessions run on a separate sandbox provider (Daytona by default; Platinum and E2B are also supported) that the API reaches over the network. It needs the internet, because the stack pulls images and every sandbox calls the API back, so it is not an air-gapped deployment. The [deployment walkthrough](deploying.md) covers the install, and the live [self-hosting walkthrough](https://opensourcecopilotalternative.com/self-hosting) summarizes the same three commands.

Microsoft documents Copilot as a service that runs in Microsoft's cloud, reached through Microsoft's apps and endpoints, and documents no self-host option for either Copilot or Copilot Studio. Data residency alignment, which Copilot Studio supports, keeps data in a chosen geography inside Microsoft's estate; it does not put the agent layer on your network. For a security review that needs the agent layer inside your own VPC or on-prem network, that is the boundary Kortix fills.

## Models

Kortix is model-agnostic. Bring your own API key from any major provider, use the ChatGPT subscription you already pay for, sign in with your OpenCode Console account for OpenCode Zen and Go, or point at your own OpenAI-compatible endpoint. You pick the model per agent, per session, or per message, and switch the day a better one lands.

Copilot's models are Microsoft's call. Microsoft's documentation says the experiences are powered by large language models for understanding and generation, notes that Anthropic subprocessors are available only in certain Microsoft 365 licensed experiences, and includes an automatic model selection. A team cannot bring its own provider or its own keys.

## Ecosystem lock-in

The lock-in is about where your company's context and configuration live. Copilot grounds answers in Microsoft Graph and Work IQ and scopes access by the signed-in person's permissions, which is its strength inside Microsoft 365. Copilot Studio agents are built and managed in Microsoft's tooling and publish into Microsoft's apps. The depth is real; the boundary is Microsoft's estate, and the configuration stays inside it.

Kortix keeps the same depth in a repo you own. Agents, skills, memory, connectors, and triggers are files in one git repo, versioned and reviewable like code, so you can `grep` the whole company, diff any change, and roll any part of it back. Reach is not limited to one vendor: 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL, and raw HTTP, with connector credentials brokered server-side so they never enter the machine.

## The decision rule

If the agent layer must run on infrastructure you own, with the configuration in your repo, any model on your keys, and a review gate before work lands, deploy Kortix. It is open source, it self-hosts free, and every session lands its work as a change request a person approves.

Microsoft Copilot is built for work inside Microsoft 365, per seat, on Microsoft's cloud. If the whole workday happens in Word, Excel, PowerPoint, Outlook, and Teams, and you want in-app assistance there, Copilot is built for exactly that. Where the boundary sits, inside one vendor's estate or in a repo you own, is the decision.

Teams weighing more options can read the [open-source Copilot alternatives](https://opensourcecopilotalternative.com/best-open-source-copilot-alternatives) guide, and teams evaluating Microsoft's agent builder can read the [Copilot Studio alternatives](https://opensourcecopilotalternative.com/copilot-studio-alternatives) page.
