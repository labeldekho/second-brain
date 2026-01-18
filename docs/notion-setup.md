# Notion Backend Setup

> [!CAUTION]
> **CRITICAL SECURITY WARNING**
>
> 1. **Do NOT** request access to any Notion pages you see linked in this repo. These are the author's private databases.
> 2. You must create **YOUR OWN** fresh databases.
> 3. This system relies on a "Bring Your Own Database" model.

## Overview

You need to create **5 separate databases** and **1 internal integration**.

1. **People** (Contacts)
2. **Projects** (Active work)
3. **Ideas** (Thoughts & concepts)
4. **Admin** (Tasks & chores)
5. **Inbox Log** (Audit trail for AI actions)

---

## Part 1: Create the Integration

Before creating databases, create the "robot" that will talk to them.

1. Go to [My Integrations](https://www.notion.so/my-integrations).
2. Click **New integration**.
3. Name it `Second Brain AI` (or similar).
4. Select the workspace you want to use.
5. **Capabilities**: Ensure "Read content", "Update content", and "Insert content" are checked.
6. **Submit** and copy the **Internal Integration Token** (starts with `secret_...`).
   * *Save this token; you will need it for n8n.*

---

## Part 2: Create the Databases

Create a new page in your Notion workspace called "Second Brain System". Inside that page, create these 5 databases.

### 1. People Database
*Stores contacts and relationship context.*

| Property Name | Type | Note |
| :--- | :--- | :--- |
| **Name** | Title | The person's name |
| **Context** | Rich Text | How you met, background |
| **Follow-ups** | Rich Text | Pending actions |
| **Last Touched** | Date | |
| **Tags** | Multi-select | Industry, Role, etc. |

### 2. Projects Database
*Active work streams.*

| Property Name | Type | Options (for Select) |
| :--- | :--- | :--- |
| **Name** | Title | |
| **Status** | Select | `Active`, `Waiting`, `Blocked`, `Someday`, `Done` |
| **Next Action** | Rich Text | |
| **Notes** | Rich Text | |
| **Tags** | Multi-select | |

### 3. Ideas Database
*Thoughts, invention, content ideas.*

| Property Name | Type | Note |
| :--- | :--- | :--- |
| **Name** | Title | |
| **One-liner** | Rich Text | Short summary |
| **Notes** | Rich Text | Full brain dump |
| **Tags** | Multi-select | |

### 4. Admin Database
*Chores, bills, scheduling.*

| Property Name | Type | Options |
| :--- | :--- | :--- |
| **Name** | Title | |
| **Due Date** | Date | |
| **Status** | Select | `Open`, `Done` |

### 5. Inbox Log
*The "Engine Room" - audits what the AI did.*

| Property Name | Type | Note |
| :--- | :--- | :--- |
| **Captured Text** | Title | The original message |
| **Filed To** | Select | `people`, `projects`, `ideas`, `admin` |
| **Confidence** | Number | 0.0 to 1.0 format |
| **Record ID** | Rich Text | Debug ID from n8n |
| **Needs Review** | Rich Text | Flag for manual fix |
| **Timestamp** | Date | |

---

## Part 3: Connect Integration to Databases

**Crucial Step**: By default, your integration cannot see these new databases. You must explicitly invite it.

For **EACH** of the 5 databases above:

1. Open the database page.
2. Click the `...` (three dots) menu in the top right.
3. Scroll down to **Connections**.
4. Search for and select your `Second Brain AI` integration.
5. Confirm the connection.

> [!TIP]
> **Verify Connectivity**
> If n8n says "Database not found" later, it is 99% because you skipped Part 3.

---

## Part 4: Get Database IDs

You will need the unique ID for each database to configure n8n.

1. Open the database as a full page.
2. Look at the URL:
   `https://www.notion.so/username/8123891238912389123?v=...`
3. The ID is the 32-character string between the `/` and the `?`.
   * Example: `8123891238912389123`
4. Copy these 5 IDs to a notepad. You will paste them into n8n in the next step.
