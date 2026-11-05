---
name: daily-morning-brief-automation
description: Produce and email Sarul's Daily Morning Brief from Gmail - read the last 24 hours of email, apply one of seven labels to each thread, and send a colour-coded HTML summary with a quote to sarkoovich@gmail.com. Use when a routine or the user asks to run the daily morning brief.
---

# Daily Morning Brief Automation

Built from the two successful runs on 5 November 2026 (manual "Run now" at 1:30 PM and
the scheduled run at 7:46 AM Melbourne time). Email design approved from a test send the same day. Do every step the same way on every run.
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
   | `Son` | school, teachers, activities, doctors, anything about the user's son |
   | `Bills` | invoices, due dates, subscriptions (e.g. Apple subscription confirmations) |
   | `Needs Reply` | a real person asking the user something |
   | `Newsletters` | newsletters, job alerts (SEEK, LinkedIn, Michael Page, EY), CFA/market updates, account notifications |
   | `Review-to-Delete` | promotions, marketing, junk (retail promos, referral campaigns, free-trial offers) |

   Use `Rent/Housing`, not the older `Rent` label.
2. If one of the seven labels is missing, create it with `create_label` using the exact name above.
3. Call `label_thread` with that one label ID for each thread.

## Step 3 - Write the brief (HTML design approved on 5 Nov 2026)

Build an HTML email from the template below. Use inline styles only (Gmail strips `<style>` tags).
Keep each bullet to one line: **bold sender/topic** – what it is, plus any amount, deadline or action.

### Section order and colors

Show a section box only when it has items. Use exactly these headers, colors and icons:

| # | Header | Box background | Left border | Header color | What goes in it |
|---|---|---|---|---|---|
| 1 | `🔴 URGENT` | `#fef2f2` | `#dc2626` | `#b91c1c` | security alerts, payments on hold, deadlines, due dates (amounts, reference numbers, phone numbers exactly as in the email) |
| 2 | `✉️ NEEDS REPLY` | `#ffffff` | `#7c3aed` | `#6d28d9` | a real person asking something: who + what they want |
| 3 | `👦 SON` | `#ffffff` | `#059669` | `#047857` | school, activities, doctors |
| 4 | `🏦 FINANCE` | `#ffffff` | `#0d9488` | `#0f766e` | bank/card/transfer/tax items (not market newsletters) |
| 5 | `🏠 RENT / HOUSING` | `#ffffff` | `#0891b2` | `#0e7490` | landlord, rent, lease, utilities, repairs |
| 6 | `💳 BILLS &amp; SUBSCRIPTIONS` | `#fffbeb` | `#d97706` | `#b45309` | invoices, subscriptions, trial end / renewal dates |
| 7 | `💼 JOBS &amp; CAREER` | `#ffffff` | `#2563eb` | `#1d4ed8` | job alerts and profile activity (Michael Page, SEEK, LinkedIn, EY, etc.); one line per sender |
| 8 | `📰 FYI` | `#ffffff` | `#6b7280` | `#374151` | everything else worth knowing (market newsletters, CFA, events) |
| 9 | `🗑️ REVIEW-TO-DELETE (<count>)` | `#ffffff` | `#9ca3af` | `#4b5563` | senders separated by ` · `, in grey text `#6b7280` |

Section 7 is a display group only; those threads still get the `Newsletters` label.
Sections 2-5 with no items go in the single "Nothing new" line instead of a box.

### Template

```html
<div style="max-width:600px;margin:0 auto;font-family:Arial,Helvetica,sans-serif;color:#1f2937;font-size:15px;line-height:1.5">
  <div style="background:#1e3a5f;color:#ffffff;padding:20px 24px;border-radius:10px 10px 0 0">
    <div style="font-size:13px;opacity:.85">{Weekday}, {D Month YYYY}</div>
    <div style="font-size:22px;font-weight:bold">☀️ Good morning, Sarul</div>
    <div style="font-size:14px;margin-top:6px">{N} emails · {U} urgent · {R} need a reply · {D} to delete</div>
  </div>
  <div style="background:#f8fafc;padding:20px 24px;border:1px solid #e5e7eb;border-top:none;border-radius:0 0 10px 10px">

    <!-- one box per non-empty section, in the order above -->
    <div style="background:{BOX_BG};border-left:4px solid {BORDER};padding:12px 16px;border-radius:6px;margin-bottom:14px">
      <div style="font-weight:bold;color:{HEADER_COLOR};margin-bottom:4px">{ICON HEADER}</div>
      <div>• <b>{Sender/topic}</b> – {one-line summary}</div>
    </div>

    <div style="color:#6b7280;font-size:13px;margin-bottom:16px">✅ Nothing new: {empty sections from 2-5, joined with " · "}</div>

    <div style="text-align:center;padding:16px;border-top:1px solid #e5e7eb;font-style:italic;color:#374151">
      "{quote}"<br/><span style="font-style:normal;font-size:13px;color:#6b7280">– {author}</span>
    </div>
  </div>
</div>
```

Rules:
- If a box has a single item, the `• ` bullet may be omitted.
- If no section 2-5 is empty, leave out the "Nothing new" line.
- If URGENT has nothing, leave the box out (the header count shows `0 urgent`).
- Quote: one genuine, correctly attributed inspirational quote.

Also write a short plain-text version (for `body`): one line per non-empty section,
e.g. `URGENT: ...`, `BILLS: ...`, `JOBS: ...`, then the Nothing-new line and the quote. No Markdown.

## Step 4 - Send

Call `send_message` with:
- `to`: `["sarkoovich@gmail.com"]`
- `subject`: `Daily Morning Brief – <D Month YYYY>` (example: `Daily Morning Brief – 5 November 2026`)
- `htmlBody`: the HTML from Step 3
- `body`: the plain-text version from Step 3

If sending fails, stop and report the error. Do not retry with other tools.

## Step 5 - Report

Reply with one line: number of threads labeled, count per label, and whether the email was sent.
