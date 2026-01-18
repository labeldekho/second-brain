# System Configuration

This document covers how to tune the Second Brain system to your personal needs.

---

## 🔧 Environment Variables

While n8n manages most credentials via its UI, this workflow relies on specific internal logic that you can tweak.

| Variable | Location | Default | Required? | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| `confidence` | n8n "Confidence Gate" Node | `0.6` | Yes | Threshold for auto-filing vs. manual review. |
| `max_words` | n8n "Message a model" Node | `150` | No | Length of the daily digest summary. |
| `triggerAtHour` | n8n "Schedule Trigger" Node | `9` (9 AM) | Yes | Hour of day to send the digest. |

---

## ⚙️ Logic Tuning

### 1. Adjusting the "Confidence Gate"

The workflow automatically files any classification with a confidence score > `0.6`.
*   **Increase to 0.8**: If you see too many "wrong" filings. The system becomes more conservative and will flag more items for review.
*   **Decrease to 0.4**: If you find the system works well and you want less manual review.

**How to change:**
1. Open the n8n workflow.
2. Double-click the **Confidence Gate** (If) node.
3. Change the Value `0.6` to your preferred number.

### 2. Changing the Daily Digest Time

The digest runs at 9:00 AM server time by default.

**How to change:**
1. Open the n8n workflow.
2. Double-click the **Schedule Trigger** node.
3. Change the **Hour** field.
4. *Tip: You can also add "Minute" to run at 9:30.*

### 3. Customizing the AI Persona

The AI uses a "System Prompt" to determine how to classify and summarize. You can change its personality.

**How to change:**
1. Double-click the **Message a model** node.
2. Scroll to **Options** -> **System Message**.
3. Edit the text.
   *   *Current*: "You are a specific JSON generator..."
   *   *Idea*: Add "You prefer concise, bulleted lists."

---

## 🛡️ Safe Defaults

This reference implementation comes with "Safe Defaults" designed to prevent data loss.

*   **Review First**: The `0.6` confidence threshold is intentionally conservative. It is better to flag a correct item for review than to misfile a critical item.
*   **Log Everything**: The **Inbox Log** database keeps a copy of *every single message* and how the AI interpreted it. If something goes wrong, you can always check the logs to see what happened.
*   **Read Only Digest**: The Daily Digest workflow only *reads* your databases; it never modifies them.

---

## 🧩 Advanced: Adding New Categories

If you want to add a "Reading List" category:

1.  **Notion**: Create a new "Reading" database.
2.  **n8n System Prompt**: Edit the Gemini prompt to include `- reading_list` as a valid type.
3.  **n8n JSON Schema**: Add the schema for the new type in the prompt.
4.  **n8n Switch**: Add a new routing rule for `reading_list`.
5.  **n8n Notion Node**: Add a new Notion node to specific the "Reading" database ID.
