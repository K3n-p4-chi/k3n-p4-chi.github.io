---
layout: post
title: "Session 2: Complete Schedule Generator Rebuild with Catalogue Integration"
date: 2025-10-22 22:14:56 +0000
categories: planning automation
tags: [planning, tools, automation, claude-code]
---

# Session 2: Complete Schedule Generator Rebuild with Catalogue Integration

## Summary
Session 2: Complete Schedule Generator Rebuild with Catalogue Integration

## What Was Achieved
- Completely rewrote generate_week5_schedule.py with catalogue integration
- Added load_catalogue() function to load phase1_master_catalogue.json
- Added get_course_by_id() to retrieve course data
- Created select_modules_for_duration() - intelligently fits modules into time slots
- Implemented ±10 min tolerance for module selection
- Generated new Week 5 schedule with ACTUAL module names (16 events)
- Deleted old Week 5 events (19 fabricated events)
- Uploaded clean schedule to Google Calendar

## Problems Solved
Complete System Overhaul
- Replaced ALL fabricated content with real module extraction
- select_modules_for_duration() solves the fitting problem:
  * Given target duration (e.g., 120 min)
  * Finds combination of modules that fit within ±10 min
  * Returns actual module names, not guesses
- FLEX event "ISO 27001 deep dive" renamed to "Agile & GDPR Fundamentals"
  * Now points to correct course
  * WHY/ALIGNMENT updated to match actual content
- All 16 events now have verified module names from catalogue

## Tools & Scripts Created
Updated Functions in generate_week5_schedule.py:
- load_catalogue(): Loads phase1_master_catalogue.json
- get_course_by_id(): Retrieves course data by ID
- select_modules_for_duration(): Intelligently selects modules
  * Accepts: course_id, start_index, target_duration, tolerance
  * Returns: (modules_list, actual_duration, next_index)
- Updated generate_ics() to remove metadata fields (LOCATION, STATUS, etc.)

## Next Steps
1. Fix ICS metadata showing in calendar descriptions
2. Clean up 03 Schedule folder - too many files
3. Create generic weekly generator (not week-specific)
4. Organize output files into subfolders

---
*Session logged automatically from Claude Code VSCode extension*
