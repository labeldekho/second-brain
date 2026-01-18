# n8n + Notion Second Brain Reference Implementation

## What this is

This repository contains a reference implementation for a "Second Brain" knowledge management system. It demonstrates how to orchestrate data between a personal inbox (Slack) and a structured knowledge base (Notion) using n8n as the middleware. It uses AI (Google Gemini) to intelligently classify unstructured thoughts into structured database entries.

**This is a template pattern.** It is designed for engineers and power users to **fork, inspect, and deploy** into their own infrastructure. It is not a finished product, but a set of blueprints.

## What this is NOT

*   ❌ **This is NOT a hosted SaaS product.**
*   ❌ **This is NOT a shared workflow.** You cannot "connect" to my instance.
*   ❌ **This is NOT a plug-and-play app.** It requires you to own and manage your own n8n and Notion accounts.

> [!IMPORTANT]
> **DO NOT REQUEST ACCESS TO ANY NOTION PAGE.**
>
> If you see a "Request Access" screen, it means you have not set up your own Notion workspace correctly. This workflow assumes YOU own the destination databases.

## High-Level Architecture

The system follows a linear "Capture → Process → Store" pipeline:

1.  **Input**: You send a message to your own private Slack channel.
2.  **Processing**: A Webhook triggers a self-hosted n8n workflow.
3.  **Intelligence**: n8n sends the text to Google Gemini API to classify it (People, Project, Idea, or Admin) and extract structured metadata.
4.  **Routing**: Based on the classification, the data is routed to the correct destination.
5.  **Output**: The record is created in **YOUR** specific Notion database.

## Prerequisites

To use this workflow, you must provide:

*   **n8n Instance**: Either self-hosted or n8n Cloud. usage.
*   **Notion Account**: You must be an Admin of your own workspace to create Integrations.
*   **Google Gemini API Key**: For the AI classification step.
*   **Slack Workspace**: For the capture interface.

## Documentation & Setup

This repository is split into modular guides to help you build this yourself.

*   📄 **[1. Notion Setup](/docs/notion-setup.md)**: How to create the databases and security integrations.
*   ⚙️ **[2. n8n Setup](/docs/n8n-setup.md)**: Importing the workflow and configuring nodes.
*   🔧 **[3. Configuration](/docs/configuration.md)**: Environment variables and system tuning.

## Common Issues & Troubleshooting

### "I requested access to a Notion page"

**If you did this, you have clicked a link to a private database used in development.**

*   **Why this happens**: The JSON workflow file contains IDs pointing to the original author's private workspace.
*   **How to fix**: You must create YOUR OWN databases and replace the IDs in the n8n workflow.
*   **Action**: Go to [Notion Setup](/docs/notion-setup.md) to build your own backend. Nothing in this repo will work until you do this.

---

*This is a reference pattern provided "as is". You are responsible for the security and maintenance of your own infrastructure.*
