---
layout: post
title: "Session 4: Generic Weekly Generator and Complete Folder Reorganization"
date: 2025-10-22 22:15:36 +0000
categories: planning automation
tags: [planning, tools, automation, claude-code]
---

# Session 4: Generic Weekly Generator and Complete Folder Reorganization

## Summary
Session 4: Generic Weekly Generator and Complete Folder Reorganization

## What Was Achieved
- Created generate_weekly_schedule.py - ONE script for ALL weeks
- Externalized week configuration to JSON (week5_config.json)
- Implemented automatic subfolder organization:
  * ICS files → Schedule_Files/
  * Markdown → Markdown/
  * JSON configs/tracking → Tracker/
- Moved calendar automation to folder 05/Google Calendar ICS Upload/
- Created comprehensive READMEs in both folders
- Archived 40+ obsolete files:
  * 20+ resolved issue docs → _Archive_Resolved_Issues/
  * 11 troubleshooting scripts → folder 05 archive
  * 8 old guides → _Archive_Old_Documentation/
- Reduced active files from 70+ to 25 essential files (65% reduction)

## Problems Solved
Week-Specific Generator Problem
- Would need 15 separate scripts (week5.py, week6.py, etc.)
- Week config hardcoded in Python
- Solution: ONE generic script + external JSON configs
- Usage: python generate_weekly_schedule.py --week 5

Folder Organization Chaos
- 70+ files scattered in root directory
- Outputs mixed with scripts
- No clear structure
- Solution: Logical subfolder hierarchy
  * _Documentation/ - Reference guides
  * Markdown/ - Generated schedules
  * Schedule_Files/ - ICS files
  * Tracker/ - Configs and tracking

Calendar Automation Separation
- Scripts were in schedule generation folder
- Should be in automation folder (05)
- Moved: ics_to_google_calendar.py, delete_weekly_events.py
- Created setup guide in folder 05

## Tools & Scripts Created
New: generate_weekly_schedule.py
- Loads week<N>_config.json automatically
- Integrates with catalogue for module extraction
- Auto-outputs to correct subfolders
- Supports --week flag or custom config path

New: Tracker/week5_config.json
- Externalizes all week configuration
- Structure: week_number, theme, dates, course_ids, events
- CORE events: subject, day_offset, start_hour, duration, type, url
- FLEX events: + why, alignment, best_fit, module_count

Reorganized Folders:
- Created: Markdown/, Schedule_Files/, Tracker/
- Moved: Calendar scripts to folder 05
- Archived: 40+ obsolete files

## Next Steps
1. Move _Archive_Troubleshooting to folder 05
2. Review Tracker folder contents (catalog.json, sessions.json)
3. Archive any remaining obsolete files
4. Create handoff document for Claude Desktop

---
*Session logged automatically from Claude Code VSCode extension*
