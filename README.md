# morning-brief · a Claude Code skill

A daily morning brief that **interviews you first** and then runs from a plain-text profile.

- **Asks, doesn't assume.** On first use it detects your timezone and connected tools, then asks up to four short rounds of multiple-choice questions: what the brief is for, which sections, format, and delivery.
- **Uses what you have.** Calendar, email, task tools, local notes, web (markets, news, weather) — it only offers sources that are actually connected.
- **Never makes things up.** Every item comes from a tool result in that run; a failed source becomes one honest line.
- **Editable.** Your answers are saved to `profile.md`; edit it by hand or say "drop the weather" / "make it shorter".
- **Schedulable.** Notes for Claude Code routines, cron and launchd in `references/scheduling.md`.

## Install

```bash
git clone https://github.com/Mrpixelraf/morning-brief-skill.git
mkdir -p ~/.claude/skills
cp -R morning-brief-skill/skills/morning-brief ~/.claude/skills/
```

Then in Claude Code: **"set up my morning brief"** (or 「帮我定制每日晨报」). Afterwards just ask **"morning brief"** / 「早报」 each day, or schedule it.

## Files

```
skills/morning-brief/
├── SKILL.md                  # interview + generate workflow
├── templates/profile.md      # the profile it writes for you
└── references/
    ├── sources.md            # how each section maps to tools
    └── scheduling.md         # routines, cron, launchd
```

## 中文说明

一个会先**问你问题**、再按你的回答定制的每日晨报 Skill。第一次使用说「帮我定制每日晨报」，它会检测你的时区和已连接的工具（日历、邮箱、任务、笔记、网页），用最多四轮选择题问清楚：晨报用途、要哪些板块、长度语气、发到哪里、几点发。回答保存成 `profile.md`，之后每天说「早报」即可；想改就说「去掉天气」「短一点」。所有内容都来自当次工具的真实结果，拿不到的数据会如实写一行，不编造。

License: MIT
