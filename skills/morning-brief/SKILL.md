---
name: morning-brief
description: Build and run a personalised daily morning brief. First run (or when the user says "set up / change my morning brief") interviews the user with short multiple-choice questions and saves a brief profile; later runs generate the brief from that profile using whatever tools are connected (calendar, email, tasks, notes, web, markets, weather, local files). Use when the user asks for a morning brief, daily digest, daily briefing, "what do I need to know today", 早报, 每日晨报, or wants to schedule one.
---

# Morning Brief

Two modes. Pick by checking for a saved profile first.

| Situation | Mode |
|---|---|
| No profile file exists, or the user says "set up", "customise", "change my brief", 定制, 重新设置 | **Interview** → save profile → generate a first brief as a preview |
| Profile exists and the user asks for today's brief (or a scheduled run fires) | **Generate** |

Profile location (first match wins): the path the user gives → `./morning-brief.profile.md` in the current project → `~/.claude/morning-brief/profile.md`. Create the directory when saving.

## Interview mode

Goal: a profile the user can read and edit by hand, built in **at most 4 rounds of questions**. Ask with the AskUserQuestion tool when it is available (multiple choice, 2–4 options, multi-select where sensible); otherwise ask in plain text as a numbered list. Always offer a sensible default so a user can accept and move on. Never ask for something you can detect yourself (timezone from the system clock, connected tools from your tool list).

Before asking anything, **detect**: current date, time and timezone (run `date`), and which data sources are actually connected (calendar, email, task managers, notes, web search/fetch, file system, chat tools). Only offer sources that exist; mention the missing ones in one line ("connect Google Calendar to add your schedule").

Round 1 — who and what for
- Language of the brief (default: the language the user is writing in).
- What the brief is mainly for: work day planning / investing and markets / a project or team / personal life. Multi-select.

Round 2 — sections (multi-select, only from detected sources plus always-available ones)
- Today's schedule and conflicts (calendar)
- Emails needing a reply (email) — ask for VIP senders or labels as free text
- Tasks and deadlines in the next N hours (task tool or a local file) — ask for N, default 72
- Project status from local notes or a folder (ask for the path)
- Markets: a watchlist and indices (ask for tickers; web access required)
- News on chosen topics (ask for 3–5 topics; web access required)
- Weather for a city (web access required)
- Yesterday's incidents / follow-ups from a log file (ask for the path)

Round 3 — format
- Length: tight (≤15 lines) / standard (≤30 lines) / detailed.
- Order of sections; "top 3 things to do today" at the top (recommended) or not.
- Tone: neutral / direct / friendly.

Round 4 — delivery and schedule
- Where: this chat / a file (ask path) / a chat channel or email if a sending tool is connected.
- When: time of day for a scheduled run, or "on demand only". See `references/scheduling.md` for how to set it up; do not create any schedule, cron job or sending action until the user confirms it.

Then write the profile using `templates/profile.md`, show it to the user in full, and ask one confirmation question: save as is / edit something / start over. After saving, generate a first brief immediately as a preview.

## Generate mode

1. Read the profile. Run `date` for today's date, weekday and time in the profile's timezone. Never infer the date from memory.
2. For each section in profile order, gather data with the mapped source. Rules in `references/sources.md`. Fetch sections in parallel when the tools allow it.
3. Write the brief to the profile's length, language and tone. Put the "Top 3 today" block first when enabled: the three most time-sensitive items across all sections, each with a reason in a few words.
4. Deliver where the profile says. When sending to an external channel, send once and confirm the send result; never resend blindly.

Hard rules:
- **Never invent content.** Every calendar item, email, number and headline must come from a tool result in this run. If a source fails or returns nothing, write one line saying so ("Calendar: couldn't connect", "No emails from VIPs since yesterday") — do not fill the gap.
- Numbers (prices, counts, dates) carry their source and as-of time when they come from the web.
- Keep private data private: do not paste email bodies or personal details into public channels; summarise.
- A section with nothing to report collapses to one line. The whole brief respects the length limit; cut from the least urgent section first.
- End with a one-line footer: sources used and the generation time.

## Changing the brief later

When the user says "add X", "drop the weather", "make it shorter", edit only that part of the profile, show the diff in one or two lines, save, and regenerate. Re-run the full interview only when asked.
