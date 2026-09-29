# Scheduling a daily brief

Only set a schedule after the user confirms the exact time and delivery target.

**Claude Code cloud routine** — if the `/schedule` command or a scheduling tool is available, create a daily routine at the chosen time whose prompt is "Run the morning-brief skill with my saved profile". Best when the brief uses connectors (calendar, email) rather than local files.

**Local machine, macOS (launchd) or Linux (cron)** — run Claude Code headless:

```bash
claude -p "Run the morning-brief skill with the profile at ~/.claude/morning-brief/profile.md and output the brief." > ~/morning-brief/$(date +%F).md
```

- cron: `30 8 * * * <the command above>`
- launchd: a `StartCalendarInterval` plist with Hour/Minute; set `PATH` so `claude` resolves, and write stdout to a log.
- The machine must be awake at that time. Headless runs can only use tools that don't need an interactive login.

**Delivery from a scheduled run** — pipe the output to a webhook (Slack/Discord incoming webhook) or email command the user already has. Store webhook URLs in a file with `chmod 600`, never inside the profile or the prompt.
