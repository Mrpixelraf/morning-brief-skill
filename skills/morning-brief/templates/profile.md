# Morning brief profile
<!-- Edit freely. The morning-brief skill reads this file on every run. -->

language: en            # en / zh / …
timezone: Europe/London # detected at setup
length: standard        # tight (≤15 lines) / standard (≤30) / detailed
tone: direct            # neutral / direct / friendly
top3: true              # put "Top 3 today" first

## Sections (in order)
- calendar:
    window: today
    flag: conflicts, travel time, events starting before 10:00
- email:
    since: yesterday 18:00
    vip: []             # names or addresses
    labels: []
- tasks:
    source: ""          # task tool name, or a local file path
    horizon_hours: 72
- project_notes:
    paths: []           # folders or files with status / to-do sections
- markets:
    watchlist: []       # tickers
    indices: [S&P 500, Nasdaq]
- news:
    topics: []
    max_items: 5
- weather:
    city: ""
- follow_ups:
    log_path: ""        # e.g. an incidents or journal folder; list yesterday/today entries

## Delivery
where: chat             # chat / file:<path> / channel:<name> / email:<address>
schedule: on-demand     # or a time like 08:30 with the method from references/scheduling.md
