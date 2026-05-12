# IELTS Speaking Skill

Personal IELTS Speaking practice system for managing current-season question banks, natural answer drafts, approved answer libraries, reusable Part 2 story materials, practice feedback, and spaced review schedules.

## Overview

This skill is designed for IELTS Speaking preparation. It helps you build a long-term personal speaking system instead of generating one-off model answers.

It focuses on:

- Current-season IELTS Speaking question management
- Natural spoken answer drafts
- Approved answer saving and versioning
- Personal profile and reusable facts
- Part 2 story-bank reuse
- Part 3 opinion modules
- Speaking feedback based on IELTS criteria
- Review scheduling for saved answers

The core principle is simple: generated answers are drafts first. They are not saved to the formal answer library unless you explicitly run `/ielts-speaking save`.

## Data Directory

By default, all data is stored in the current workspace under:

```text
speakingData/
```

Main files:

```text
speaking_profile.json
speaking_question_bank.json
speaking_answers.json
speaking_story_bank.json
speaking_phrase_bank.json
speaking_opinion_bank.json
speaking_practice_records.json
speaking_review.json
```

The skill uses JSON files, not JSONL files. Missing files are created when needed, while existing user data is preserved.

## Common Commands

### Daily Practice

```text
/ielts-speaking
/ielts-speaking today
```

Creates today's practice list from the question bank. It prioritizes due review questions, high-priority new questions, new Part 2 questions, and older saved or practiced questions.

### Add a Question

```text
/ielts-speaking add [part/category/question/cue points]
```

Adds one new question to the question bank. If important details are missing, the skill asks only for the missing information needed to create a clean record.

### Generate an Answer Draft

```text
/ielts-speaking answer [question_id or question text]
```

Generates an editable answer draft. It does not save the answer automatically.

The response includes:

- Draft answer
- Key words and spoken phrases
- Materials used, such as facts, stories, or opinion modules
- Next commands for editing or saving

### Edit a Draft

```text
/ielts-speaking edit [feedback]
```

Revises the latest unsaved draft. You can ask for changes such as:

- Make it shorter
- Make it more natural
- Add personal details
- Make it easier to say
- Reduce memorized-sounding phrases
- Improve vocabulary while keeping it spoken

### Save an Approved Answer

```text
/ielts-speaking save
```

Saves the latest approved draft into the formal answer library. It also updates the question status and review schedule.

For Part 2, the skill extracts or updates a reusable story in the story bank after the answer is saved.

### Review Saved Questions

```text
/ielts-speaking review
```

Shows due review questions first without showing the saved answer. After you answer, the skill can give feedback and then show the saved reference answer.

### Get Feedback

```text
/ielts-speaking feedback [your answer]
```

Scores and comments on your answer using IELTS Speaking criteria:

- Fluency and coherence
- Lexical resource
- Grammatical range and accuracy
- Pronunciation, when audio or reliable speech information is available
- Personal fit

Text-only responses cannot fully assess pronunciation.

### List and Stats

```text
/ielts-speaking list
/ielts-speaking stats
```

Use these commands to inspect question coverage, answer status, review workload, Part 2 story coverage, and weak categories.

## Question Statuses

The skill uses five statuses:

```text
new
saved
practiced
fluent
exam_ready
```

- `new`: no approved answer has been saved
- `saved`: an approved answer exists, but it is not fluent yet
- `practiced`: the user has practiced it, but it is still unstable
- `fluent`: the user can say it smoothly
- `exam_ready`: the answer is ready for exam use and only needs light review

## Part 2 Story Reuse

Part 2 answers are handled differently from Part 1 and Part 3.

When a Part 2 answer is approved and saved, the skill extracts the reusable story behind it and stores it in:

```text
speakingData/speaking_story_bank.json
```

Later, when a similar cue card appears, the skill can reuse the story if it genuinely fits the question and the user's real experience.

It should not force a story into an answer just because the topic category is similar.

## Recommended Workflow

```text
/ielts-speaking today
```

Choose one question from the daily list.

```text
/ielts-speaking answer [question_id]
```

Review the draft and ask for revisions.

```text
/ielts-speaking edit make it shorter and more natural
```

When satisfied, save it.

```text
/ielts-speaking save
```


Review due questions regularly.

```text
/ielts-speaking review
```

Practice later and get feedback.

```text
/ielts-speaking feedback [your spoken or typed answer]
```
