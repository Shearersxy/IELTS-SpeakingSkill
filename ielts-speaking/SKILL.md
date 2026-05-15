---
name: ielts-speaking
description: IELTS speaking question-bank and material system. Use when the user asks for IELTS Speaking Part 1, Part 2, or Part 3 current-season question management, adding one question to the bank, personalized answer generation, editable answer drafts, answer level settings, approved answer saving, daily practice, answer revision, speaking feedback, review scheduling, progress stats, or commands such as /ielts-speaking, /ielts-speaking today, /ielts-speaking add, /ielts-speaking answer, /ielts-speaking level, /ielts-speaking edit, /ielts-speaking save, /ielts-speaking review, /ielts-speaking feedback, /ielts-speaking list, or /ielts-speaking stats.
---

# IELTS Speaking

## Purpose

Manage a personalized IELTS speaking system built around the current-season question bank, approved answer library, reusable personal materials, daily practice, and spaced review.

Do not treat IELTS speaking like IELTS writing. Speaking work should optimize for natural spoken output, personal fit, flexible reuse, and exam-room recall.

## Core Principles

- Generate answers as editable conversation drafts first. Save nothing to the formal answer library unless the user explicitly runs `/ielts-speaking save` or clearly asks to save the current answer.
- Keep generated answers natural, personal, and easy to say. Avoid essay-style linking, dense clauses, memorized templates, and unrealistic advanced wording.
- Use the user's real profile, facts, stories, phrases, and opinion modules whenever they genuinely fit. Do not force a reusable material into an answer just because the topic is similar.
- Default generated answers to IELTS Speaking band `7.0-7.5` unless the user sets another target level.
- For `/ielts-speaking answer`, inspect reusable materials first and let the user confirm which material to use before drafting when suitable options exist.
- For Part 2, use story-based answers and preserve reusable story value. After the user approves and saves a Part 2 answer, extract or update the story in `speaking_story_bank.json`.
- For feedback, judge the user's response against IELTS Speaking band descriptors and personal fit, not against whether it matches a saved answer.
- Preserve existing user data. Create missing data files only when needed, and avoid overwriting user-approved content.

## Status Model

For allowed question statuses and shared data rules, read `references/data-contracts.md` when a command reads or updates data.

## Data Entry Points

Default to the current workspace `speakingData/` directory.

- `speakingData/speaking_profile.json` - user background, preferences, constraints, and reusable personal facts.
- `speakingData/speaking_question_bank.json` - current-season IELTS speaking questions.
- `speakingData/speaking_answers.json` - approved saved answers only.
- `speakingData/speaking_story_bank.json` - reusable personal stories, mainly for Part 2.
- `speakingData/speaking_phrase_bank.json` - natural spoken expressions.
- `speakingData/speaking_opinion_bank.json` - Part 3 idea modules.
- `speakingData/speaking_practice_records.json` - practice attempts and feedback.
- `speakingData/speaking_review.json` - review schedule for saved questions.

Before creating or modifying data files, read `references/schemas.md` and `references/data-contracts.md`.

## Command Entry Points

Before running a command workflow, read only the matching workflow file:

- `/ielts-speaking` or `/ielts-speaking today` - read `references/workflows/today.md`.
- `/ielts-speaking answer [question_id or question text]` - read `references/workflows/answer.md`.
- `/ielts-speaking level [target band]` - read `references/workflows/level.md`.
- `/ielts-speaking add [part/category/question/cue points]` - read `references/workflows/add.md`.
- `/ielts-speaking edit [feedback]` - read `references/workflows/edit.md`.
- `/ielts-speaking save` - read `references/workflows/save.md`.
- `/ielts-speaking review` - read `references/workflows/review.md`.
- `/ielts-speaking feedback [user answer]` - read `references/workflows/feedback.md`.
- `/ielts-speaking list` - read `references/workflows/list.md`.
- `/ielts-speaking stats` - read `references/workflows/stats.md`.
- Current-season question import - read `references/workflows/import.md`.

## Reference Files

- `references/schemas.md` - data schemas and allowed field values.
- `references/data-contracts.md` - shared data meanings, defaults, status values, review intervals, and save/draft contracts.
- `references/workflows/` - command-specific workflows. Load the matching file only.
