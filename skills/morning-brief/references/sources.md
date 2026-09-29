# Source rules

Map each profile section to what is connected. If nothing fits, say the section is unavailable in one line and suggest what to connect.

| Section | Preferred source | Fallback | Notes |
|---|---|---|---|
| calendar | Calendar connector (Google Calendar, Outlook) | none | List today's events in local time; flag overlaps and events with no gap for travel. |
| email | Email connector (Gmail, Outlook) | none | Search threads since the profile's `since`. Some connectors truncate long threads in search results — open the thread before saying "no new reply". Summarise; never quote full bodies. |
| tasks | Task connector (Linear, Asana, Notion, Todoist…) | Local markdown file: unchecked `- [ ]` items with dates | Due within `horizon_hours`, overdue first. |
| project_notes | File system | — | Read only the "status" / "to-do" / "next" sections of each file, not whole folders. |
| markets | Web fetch of a quote page or a finance API | Web search | Record price, change and the as-of time. Markets closed → say so and show the last close. |
| news | Web search / fetch | — | 3–5 items, one line each with source name. Skip anything you cannot open. |
| weather | Web fetch of a weather page | Web search | High/low, rain chance, one line. |
| follow_ups | File system | — | List entries dated yesterday or today by filename or front-matter date. |

General:
- Run `date` first; every "today"/"yesterday" is relative to the profile timezone.
- Parallelise independent fetches.
- A failed source never blocks the brief; note it and continue.
