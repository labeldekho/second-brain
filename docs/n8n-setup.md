# n8n Workflow Setup

This guide explains how to import and configure the automation engine.

## Prerequisites

*   A running n8n instance (Version 1.0+ recommended)
*   The `second-brain.json` file from this repository
*   Your 5 Notion Database IDs (from the [Notion Setup](/docs/notion-setup.md) guide)
*   Google Gemini API Key
*   Slack Webhook URL

---

## Step 1: Import the Workflow

1. Open your n8n dashboard.
2. Click **Add Workflow** (top right).
3. Click the three dots `...` menu → **Import from File**.
4. Select `second-brain.json`.

> [!NOTE]
> You will see a large workflow appear. Don't panic! We will configure it step-by-step.

---

## Step 2: Configure Credentials

You need to set up authentication for 3 services.

### 1. Slack
1. Double-click the **Slack** node.
2. Under "Credentials", select **Create New**.
3. Follow n8n's guide to connect your Slack Bot User.
4. **Crucial**: Ensure the bot is invited to your `#second-brain-inbox` channel (`/invite @your-bot-name`).

### 2. Google Gemini
1. Double-click the **Message a model** (Gemini) node.
2. Under "Credentials", select **Create New**.
3. Paste your Google AI Studio API Key.

### 3. Notion
1. Double-click any **Notion** node (e.g., "Inbox Log").
2. Under "Credentials", select **Create New**.
3. Choose **Internal Integration API**.
4. Paste the specific `secret_...` token you created in the Notion Setup guide.

---

## Step 3: Map Your Database IDs

The imported workflow contains **placeholder IDs** that point to the original author's private workspace. **You must replace these.**

### Locate the Nodes
Find the following nodes in the workflow editor:

1.  `Inbox Log` (There are 2 of these nodes)
2.  `People Database`
3.  `Projects Database`
4.  `Ideas Database`
5.  `Admin Database`

### Update Each Node
For **each** of the nodes list above:

1. Double-click the node to open settings.
2. Find the **Database** field.
3. Click the "List" icon or toggle to "By ID".
4. **Paste YOUR specific Database ID** (the 32-char string you saved earlier).

> [!WARNING]
> **If you skip this step, the workflow will fail with "Database not found" or "Access Denied".**

---

## Step 4: Understanding the Logic

You don't need to be a coder to understand what's happening. Here is the logic flow:

1.  **Extract Text**: We strip away Slack metadata to get just your typed message.
2.  **Message a Model (Gemini)**: We send your text + a strict prompt to Google's AI.
    *   *Prompt logic*: "Classify this as People, Project, Idea, or Admin. Return JSON."
3.  **Confidence Gate**:
    *   If AI is > 60% sure (`0.6`), it goes to the correct database automatically.
    *   If AI is unsure (`<= 0.6`), it goes to **Inbox Log** with a "Needs Review" flag.
4.  **Type Router**: A "Switch" node that directs traffic based on the classification.

---

## Step 5: Activate

1. Click **Save** in the top right.
2. Toggle **Active** to `True` (Green).
3. Send a test message in your Slack channel to verify!
