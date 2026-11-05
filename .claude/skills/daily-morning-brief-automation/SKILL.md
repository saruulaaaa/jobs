---
name: daily-morning-brief-automation
description: Produce and email Sarul's Daily Morning Brief from Gmail - read the last 24 hours of email, apply one of seven labels to each thread, and send a plain-text summary with a quote to sarkoovich@gmail.com. Use when a routine or the user asks to run the daily morning brief.
---

# Daily Morning Brief Automation

Built from the two successful runs on 5 November 2026 (manual "Run now" at 1:30 PM and
the scheduled run at 7:46 AM Melbourne time). Do every step the same way on every run.
Do not add steps, sections or sources that are not listed here.

## Requirements

- **Connector:** Gmail only (account sarkoovich@gmail.com). No other connector, no web search, no code.
- **Gmail tools used:** `search_threads`, `get_thread`, `list_labels`, `create_label`, `label_thread`, `send_message`.
- If no Gmail tools are available, stop and report "Gmail connector is not attached". Do not send anything.

## Hard rules

- NEVER delete, trash, archive, or mark anything as spam. Only add labels.
- Never remove labels that are already on a thread.
- Send only to sarkoovich@gmail.com.

## Step 1 - Pull the last 24 hours

1. Call `search_threads` with query `newer_than:1d -in:sent -in:drafts`, `pageSize: 50`.
   Follow `pageToken` until there are no more pages.
2. Skip any thread whose subject starts with "Daily Morning Brief" (previous briefs).
3. For each remaining thread call `get_thread` with `messageFormat: PLAIN_TEXT` and read it.
   Pull: sender name and address, date, subject, what it is about, and any amount,
   due date, deadline, account/reference number, or action the user must take.

## Step 2 - Label every thread (exactly one label each)

1. Call `list_labels` and map these exact names to their label IDs:

   | Label | Use for |
   |---|---|
   | `Finance` | banks, cards, investments, taxes, transfers and receipts |
   | `Rent/Housing` | landlord, rent, lease, home utilities, repairs |
   | `Son` | school, teachers, activities, doctors, anything about her son |
   | `Bills` | invoices, due dates, subscriptions (e.g. Apple subscription confirmations) |
   | `Needs Reply` | a real person asking the user something |
   | `Newsletters` | newsletters, job alerts (SEEK, LinkedIn, Michael Page, EY), CFA/market updates, account notifications |
   | `Review-to-Delete` | promotions, marketing, junk (retail promos, referral campaigns, free-trial offers) |

   Use `Rent/Housing`, not the older `Rent` label.
2. If one of the seven labels is missing, create it with `create_label` using the exact name above.
3. Call `label_thread` with that one label ID for each thread.

## Step 3 - Write the brief

Plain text, uppercase section headers, `- ` bullets, a blank line between sections.
Use this exact order and wording for headers:

```
DAILY MORNING BRIEF – <Weekday>, <D Month YYYY>
<N> threads came in over the last 24 hours. All are labeled; nothing was deleted, archived or marked as spam.

URGENT
- <Sender> (<address if useful>), <D Mon>: <what happened, amount, deadline, what to do and by when>

NEEDS REPLY
- <Person>: <what they want>

SON
- <item>

FINANCE
- <item>

RENT / HOUSING
- <item>

BILLS / SUBSCRIPTIONS
- <item, including trial end / renewal dates to watch>

FYI
- <Sender>: <one-line summary; group several emails from the same sender into one line>

REVIEW-TO-DELETE: <count> emails
- <Sender> (<what it was>)

QUOTE OF THE DAY
"<quote>" – <author>
```

Content rules (as in the tested runs):
- **URGENT** = security alerts, payments on hold, deadlines, due dates. Include amounts, reference numbers, deadlines and phone numbers exactly as in the email. If nothing is due, write `- No deadlines, due dates or amounts due today.`
- An urgent item can still carry a different label (e.g. a Google security alert labeled `Newsletters`).
- Empty sections: keep the header and write `- Nothing came in.` (for NEEDS REPLY: `- Nothing. No real person is asking you anything.`).
- **QUOTE OF THE DAY** = one genuine, correctly attributed inspirational quote.

## Step 4 - Send

Call `send_message` with:
- `to`: `["sarkoovich@gmail.com"]`
- `subject`: `Daily Morning Brief – <D Month YYYY>` (example: `Daily Morning Brief – 5 November 2026`)
- `body`: the plain-text brief from Step 3 (no Markdown, no `htmlBody`)

If sending fails, stop and report the error. Do not retry with other tools.

## Step 5 - Report

Reply with one line: number of threads labeled, count per label, and whether the email was sent.
