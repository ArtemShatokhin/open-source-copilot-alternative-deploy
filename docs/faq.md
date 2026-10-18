# Kortix is open source: questions buyers ask before a Copilot alternative rollout

Kortix is the open-source AI Operating System, and this repository deploys it. These are the questions security, IT, and platform teams ask before they move the agent layer off Microsoft's cloud. Every capability answer matches Kortix's own documentation.

## Is Microsoft Copilot open source?

No. Microsoft publishes no Copilot source code. Microsoft's own documentation describes Copilot as an AI assistant reached through Microsoft's apps and cloud endpoints, with grounding in Microsoft Graph and Work IQ and access scoped by each person's permissions ([Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/copilot/microsoft-365-copilot-overview)). With no published source, a security review can check behavior only against Microsoft's documentation, and there is nothing to fork.

## Can Copilot Studio run fully self-hosted?

No. Copilot Studio is a Microsoft-hosted platform for building custom agents, workflows, and apps. Microsoft documents data residency alignment, major compliance standards, and audit logs that flow into Microsoft Purview, plus a US Government Community Cloud plan, and sells it by Copilot Credit consumption ([Microsoft's Copilot Studio page](https://www.microsoft.com/en-us/copilot/products/copilot-studio)). Data residency keeps data in a chosen geography inside Microsoft's estate; it does not run the agent layer on your network.

## Can I self-host Kortix?

Yes. Run it on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host is free. It runs as one Docker Compose stack, and the same file runs on a laptop, a VPS, or a cloud VM. It needs the internet, because the stack pulls images and every sandbox calls the API back, so it is not air-gapped. The [deployment walkthrough](deploying.md) has the steps.

## Is Kortix a replacement for GitHub Copilot?

Not for suggestions while you type. GitHub Copilot is a coding assistant inside editors; Kortix runs agents in the cloud, one isolated sandbox per session, each on its own branch. Session work reaches `main` through a change request, and merge is default-deny for agents. The two share the Copilot name in the market, and they do different jobs.

## Which models can I use with Kortix?

Any model provider with your own keys, or the ChatGPT plan you already pay for, or sign in with your OpenCode Console account for OpenCode Zen and Go. You pick the model per agent, per session, or per message. Copilot's models are Microsoft's call, and a team cannot bring its own provider or keys.

## Does a Kortix agent ask before it acts?

Approval gates you set, off until you set them. Each tool call is set to allow, ask, or block, down to the arguments it was given. An Ask holds the call until a person approves it, then the agent resumes. Session work still lands through a change request a person merges, so nothing merges itself.

## What does it cost to run this deployment?

Self-host is free; the software and the compose stack cost nothing beyond the host you run it on and the model keys you bring. Paid plans start at $50 a month for 5,000 credits, and members are free, sharing one credit pool. Any provider, your keys.

## Is this repository affiliated with Microsoft or GitHub?

No. Copilot, GitHub Copilot, Copilot Studio, and Microsoft 365 Copilot are Microsoft products. This repository names them only to compare. The self-hosting walkthrough is at [self-hosting](https://opensourcecopilotalternative.com/self-hosting), the head-to-head is at [kortix-vs-copilot](kortix-vs-copilot.md), and the short version of Copilot is at [what Microsoft Copilot is](https://opensourcecopilotalternative.com/what-is-microsoft-copilot).
